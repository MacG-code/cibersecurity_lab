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

| Puerto   | Estado   | Servicio       |
|----------|----------|----------------|
| 21/tcp   | Open     | FTP            |
| 22/tcp   | Open     | SSH            |
| 23/tcp   | Open     | Telnet         |
| 25/tcp   | Open     | SMTP           |
| 53/tcp   | Open     | DNS (Domain)   |
| 80/tcp   | Open     | HTTP           |
| 111/tcp  | Open     | RPCBind        |
| 139/tcp  | Open     | NetBIOS-SSN    |
| 445/tcp  | Open     | Microsoft-DS   |
| 512/tcp  | Open     | Exec           |
| 513/tcp  | Open     | Login          |
| 514/tcp  | Open     | Shell          |
| 1099/tcp | Open     | RMI Registry   |
| 1524/tcp | Open     | Ingreslock     |
| 2049/tcp | Open     | NFS            |
| 2121/tcp | Open     | CCPROXY-FTP    |
| 3306/tcp | Open     | MySQL          |
| 5432/tcp | Open     | PostgreSQL     |
| 5900/tcp | Open     | VNC            |
| 6000/tcp | Open     | X11            |
| 6667/tcp | Open     | IRC            |
| 8009/tcp | Open     | AJP13          |
| 8180/tcp | Open     | Unknown        |


La presencia de múltiples servicios abiertos, algunos de ellos desactualizados o con configuraciones de seguridad débiles, incrementa significativamente la superficie de ataque del sistema y podría facilitar accesos no autorizados si no se aplican controles de seguridad adecuados.


## 3. Deteccion de Servicios

```bash
nmap -sV 192.168.1.4
```
![Servicios](screenshots/Deteccion%20de%20Servicios.png)

### Servicios 

| Servicio     | Versión |
|-------------|----------|
| FTP         | vsftpd 2.3.4 |
| SSH         | OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0) |
| Telnet      | Linux telnetd |
| SMTP        | Postfix smtpd |
| DNS         | ISC BIND 9.4.2 |
| HTTP        | Apache httpd 2.2.8 ((Ubuntu) DAV/2) |
| RPCBind     | 2 (RPC #100000) |
| NetBIOS-SSN | Samba smbd 3.X - 4.X (WORKGROUP) |
| Exec        | Desconocido |
| Login       | OpenBSD or Solaris rlogind |
| TCPWrapped  | Servicio protegido por tcpwrappers |
| Java RMI    | GNU Classpath grmiregistry |
| Bind Shell  | Metasploitable root shell |
| NFS         | 2-4 (RPC #100003) |
| FTP         | ProFTPD 1.3.1 |
| MySQL       | MySQL 5.0.51a-3ubuntu5 |
| PostgreSQL  | PostgreSQL DB 8.3.0 - 8.3.7 |
| VNC         | VNC (protocol 3.3) |
| X11         | Access denied |
| IRC         | UnrealIRCd |
| AJP13       | Apache Jserv (Protocol v1.3) |
| HTTP        | Apache Tomcat/Coyote JSP engine 1.1 |


## 4. Deteccion de Sistema Operativo

```bash
nmap -O 192.168.1.4
```
![SistemaOperativo](screenshots/Deteccion%20de%20SO.png)

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
