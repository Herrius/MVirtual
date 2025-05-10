```
kerbrute userenum --dc 10.10.92.61 -d thm.local /usr/share/seclists/Usernames/top-usernames-shortlist.txt

    __             __               __     
   / /_____  _____/ /_  _______  __/ /____ 
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/                                        

Version: dev (n/a) - 05/07/25 - Ronnie Flathers @ropnop

2025/05/07 13:20:30 >  Using KDC(s):
2025/05/07 13:20:30 >   10.10.92.61:88

2025/05/07 13:20:30 >  [+] VALID USERNAME:       guest@thm.local
2025/05/07 13:20:30 >  [+] VALID USERNAME:       administrator@thm.local
```

```
smbmap -H 10.10.92.61 -u "guest" -p ""  

    ________  ___      ___  _______   ___      ___       __         _______
   /"       )|"  \    /"  ||   _  "\ |"  \    /"  |     /""\       |   __ "\
  (:   \___/  \   \  //   |(. |_)  :) \   \  //   |    /    \      (. |__) :)
   \___  \    /\  \/.    ||:     \/   /\   \/.    |   /' /\  \     |:  ____/
    __/  \   |: \.        |(|  _  \  |: \.        |  //  __'  \    (|  /
   /" \   :) |.  \    /:  ||: |_)  :)|.  \    /:  | /   /  \   \  /|__/ \
  (_______/  |___|\__/|___|(_______/ |___|\__/|___|(___/    \___)(_______)
-----------------------------------------------------------------------------
SMBMap - Samba Share Enumerator v1.10.5 | Shawn Evans - ShawnDEvans@gmail.com
                     https://github.com/ShawnDEvans/smbmap

[*] Detected 1 hosts serving SMB                                                                                                  
[*] Established 1 SMB connections(s) and 1 authenticated session(s)                                                      
                                                                                                                             
[+] IP: 10.10.92.61:445 Name: thm.local                 Status: Authenticated
        Disk                                                    Permissions     Comment
        ----                                                    -----------     -------
        ADMIN$                                                  NO ACCESS       Remote Admin
        C$                                                      NO ACCESS       Default share
        IPC$                                                    READ ONLY       Remote IPC
        NETLOGON                                                NO ACCESS       Logon server share 
        SYSVOL                                                  NO ACCESS       Logon server share 

```

```
nslookup -type=SRV _ldap._tcp.thm.local 10.10.92.61
Server:         10.10.92.61
Address:        10.10.92.61#53

_ldap._tcp.thm.local    service = 0 100 389 labyrinth.thm.local.

                                                                                                                                                     
┌──(kali㉿kali)-[~]
└─$ nslookup -type=SRV _kerberos._tcp.thm.local 10.10.92.61
Server:         10.10.92.61
Address:        10.10.92.61#53

_kerberos._tcp.thm.local        service = 0 100 88 labyrinth.thm.local.

                                                                                                                                                     
┌──(kali㉿kali)-[~]
└─$ nslookup -type=SRV _gc._tcp.thm.local 10.10.92.61
Server:         10.10.92.61
Address:        10.10.92.61#53

_gc._tcp.thm.local      service = 0 100 3268 labyrinth.thm.local.

```