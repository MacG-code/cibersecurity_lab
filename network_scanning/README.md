# Network Scanning Lab

## Objetivo

Realizar reconocimiento de red en un entorno controlado utilizando Nmap.

## Herramientas

- Kali Linux
- Nmap
- Metasploitable 2

## Alcance

Red de laboratorio: 192.168.1.0/24

Objetivo analizado: 192.168.1.4

## Resultados Esperados

- Descubrimiento de hosts
- Identificación de servicios
- Enumeración de puertos


## 1. Descubrimiento de Host

```bash
nmap -sn 192.168.1.0/24
```
![HostsEncontrados](screenshots/Descubrimiento%20de%20Hosts.png)

Hosts encontrados: 

```bash
192.168.1.1
192.168.1.2
192.168.1.4   <--- Host Objetivo
192.168.1.5
192.168.1.6
192.168.1.8
192.168.1.12

```
Estado : 7 hosts Up


## 2. Escaneo de Puertos
```bash
nmap -sS 192.168.1.4
```
![PuertosAbiertos](screenshots/Escaneo%20de%20Puertos.png)

Puertos abiertos

21/tcp   open  ftp
22/tcp   open  ssh
23/tcp   open  telnet
25/tcp   open  smtp
53/tcp   open  domain
80/tcp   open  http
111/tcp  open  rpcbind
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
512/tcp  open  exec
513/tcp  open  login
514/tcp  open  shell
1099/tcp open  rmiregistry
1524/tcp open  ingreslock
2049/tcp open  nfs
2121/tcp open  ccproxy-ftp
3306/tcp open  mysql
5432/tcp open  postgresql
5900/tcp open  vnc
6000/tcp open  X11
6667/tcp open  irc
8009/tcp open  ajp13
8180/tcp open  unknown


## 3. Deteccion de Servicios

```bash
nmap -sV 192.168.1.4
```
![Servicios](screenshots/Deteccion%20de%20Servicios.png)

### Servicios 

SERVICE     VERSION
ftp         vsftpd 2.3.4
ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
telnet      Linux telnetd
smtp        Postfix smtpd
domain      ISC BIND 9.4.2
http        Apache httpd 2.2.8 ((Ubuntu) DAV/2)
rpcbind     2 (RPC #100000)
netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
exec?
login       OpenBSD or Solaris rlogind
tcpwrapped
java-rmi    GNU Classpath grmiregistry
bindshell   Metasploitable root shell
nfs         2-4 (RPC #100003)
ftp         ProFTPD 1.3.1
mysql       MySQL 5.0.51a-3ubuntu5
postgresql  PostgreSQL DB 8.3.0 - 8.3.7
vnc         VNC (protocol 3.3)
X11         (access denied)
irc         UnrealIRCd
ajp13       Apache Jserv (Protocol v1.3)
http        Apache Tomcat/Coyote JSP engine 1.1


## 4. Deteccion de Sistema Operativo

```bash
nmap -O 192.168.1.4
```
![SistemaOperativo](screenshots/Deteccion%20de%20Sistema%20Operativo.png)

### Sistema Operativo

OS: Linux 2.6.x

OS details: Linux 2.6.9 - 2.6.33


## Analisis de Hallazgos

| Puerto  | Servicio   | Riesgo                                                               |
| ------- | ---------- | -------------------------------------------------------------------- |
| 21      | FTP        | Puede permitir acceso no autorizado si existen credenciales débiles. |
| 23      | Telnet     | Transmite credenciales en texto plano.                               |
| 25      | SMTP       | Posible enumeración de usuarios.                                     |
| 139/445 | Samba      | Superficie de ataque para compartición de archivos.                  |
| 3306    | MySQL      | Posible acceso remoto a bases de datos.                              |
| 5432    | PostgreSQL | Exposición de información sensible.                                  |
| 5900    | VNC        | Acceso remoto al escritorio.                                         |
| 1524    | tcp        | Shell privilegiadaa expuesta                                         |

## Riesgos

- Telnet habilitado
- Shell expuesta en el puerto 1524
- Servicios obsoletos 
- Samba espuesto
- Bases de datos accesibles desde red
- RPC expuestos
- DNS accesible

## Mitigaciones 

- Dashabilitar Telnet y usar SSH
- Restringir acceso a bases de datos
- Actualizar servicios
- Implementar firewall
- Limitar exposiciones de RPC
- Segmentar red

## Conclusion

```bash
El análisis de reconocimiento permitió identificar múltiples servicios expuestos en el host objetivo. Se detectaron protocolos inseguros, servicios desactualizados y componentes intencionalmente vulnerables propios de Metasploitable2. La combinación de estos factores incrementa significativamente la superficie de ataque del sistema y podría facilitar accesos no autorizados si no se aplican controles de seguridad adecuados.
```
