# Network Scanning Lab

## Objetivo

Realizar actividades de reconocimiento y enumeración sobre una máquina vulnerable en un entorno controlado con el fin de identificar hosts activos, puertos abiertos, servicios expuestos y el sistema operativo del objetivo.

## Entorno de Laboratorio
### Herramientas

- Kali Linux
- Nmap
- Metasploitable 2
- VirtualBox

### Configuracion

| Componente            | Descripción         |
| --------------------- | ------------------- |
| Máquina de análisis   | Kali Linux          |
| Máquina objetivo      | Metasploitable2     |
| Dirección IP objetivo | 192.168.1.11        |
| Tipo de red           | Laboratorio virtual |


## Alcance

> Red de laboratorio: 192.168.1.0/24

> Objetivo analizado: 192.168.1.4

## Resultados Esperados

- Descubrimiento de hosts
- Identificación de servicios
- Enumeración de puertos
- Deteccin de sistema perativo

-------------------------------------------------------------------------------------
## 1. Descubrimiento de Host

Comando utilizado:
```bash
nmap -sn 192.168.1.0/24
```
### Resultado 
Se identificaron 7 hosts activos dentro del segmento de red analizado.
Se localizo la maquina Metasploitable2 como objetivo del laboratorio.


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

Estado : 7 hosts Up
```


### Analisis 

Nos permitio indentificar los dispositivos activos presentes en la red. Esta fase es el primer paso para cualquier proceso de reconocimiento.

--------------------------------------------------------------------------------
## 2. Escaneo de Puertos
Comando utilizado:
```bash
nmap -sS 192.168.1.4
```
### Resultado
Se identificaron 23 puertos TCP abiertos en el host objetivo.


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

### Analisis

La presencia de múltiples servicios abiertos, algunos de ellos desactualizados o con configuraciones de seguridad débiles, incrementa significativamente la superficie de ataque del sistema y podría facilitar accesos no autorizados si no se aplican controles de seguridad adecuados.

-----------------------------------------------------------------------------
## 3. Deteccion de Servicios
Comando utilizado:
```bash
nmap -sV 192.168.1.4
```
### Resultado 
Se identificaron diversas aplicaciones y versiones ejecutándose en el sistema.


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

### Analisis
Se evidencio la enumeracion de diferentes tipos de servicios en el que pudimos identificar software antiguo y potencialmente vulnerable. Servicios como Telnet, Samba, MySQL, PostgreSQL y servidores web en Apache y Tomcat. 
Tambien se indetifico un servicio de shell privilegiada expuesta en el puerto 1524/TCP.

-----------------------------------------------------------------------------
## 4. Deteccion de Sistema Operativo
Comando utilizado:
```bash
nmap -O 192.168.1.4
```
### Resultado
Sistema Operativo detectado:
- Linux 2.6.x
- Linux 2.6.9 - 2.6.33

![SistemaOperativo](screenshots/Deteccion%20de%20SO.png)

### Analisis
Se detecto el sistema operativo del objetivo en la que se identifico una distribucion Linux basada en un kernel antiguo. Sistemas obsoletos hacen que sean mas vulnerables a ataques explotando sus vulnerabilidades de seguridad.


---------------------------------------------------------------------------
## Hallazgos Relevantes

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

## Recomendaciones para esta maquina 

- Dashabilitar Telnet y usar SSH
- Restringir acceso a bases de datos
- Actualizar servicios
- Implementar firewall
- Limitar exposiciones de RPC
- Segmentar red

## Conclusion

El análisis de reconocimiento permitió identificar múltiples servicios expuestos en el host objetivo. Se detectaron protocolos inseguros, servicios desactualizados y componentes intencionalmente vulnerables propios de Metasploitable2. La combinación de estos factores incrementa significativamente la superficie de ataque del sistema y podría facilitar accesos no autorizados si no se aplican controles de seguridad adecuados.

