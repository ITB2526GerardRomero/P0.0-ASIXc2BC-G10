**Resumen de la red y equipo **

**Hostname**: DN10 / D-N10

**Interfaz y dirección IP estática:** enp2s0 / 192.168.10.14/24

**Puerta de enlace (Gateway):** 192.168.10.10 (R-N10)

**Servicios instalados: **bind9 (DNS) - isc-dhcp-server (DHCP)

**Archivos de configuración**

**Configuración de Red (Netplan):**

**Archivo**: /etc/netplan/50-cloud-init.yaml

**Contenido**: 

| network:     version: 2     ethernets:         enp1s0:             dhcp4: true         enp2s0:             dhcp4: false             addresses: [192.168.10.14/24]             routes:               - to: default                 via: 192.168.10.10             nameservers:                addresses: [127.0.0.1] | 
| --- | 

**Ajuste del puerto DNS (systemd-resolved):**

**Archivo**: /etc/systemd/resolved.conf

*La linea que libera el puerto 53 para BIND.*

| DNSStubListener=no | 
| --- | 

**Configuración general de BIND9:**

**Archivo: **/etc/bind/named.conf.options

**Contenido: **Los forwarders (8.8.8.8, 1.1.1.1) y los permisos de escucha (allow-query { any; }).

| options {         directory "/var/cache/bind";         forwarders {                 8.8.8.8;                 1.1.1.1;         };         allow-query { any; };         listen-on { any; }; }; | 
| --- | 

**Declaración de zonas DNS:**

**Archivo: **/etc/bind/named.conf.local

**Contenido**: Las zonas "R" y "R-N10" vinculadas a sus respectivos ficheros de zona.

| zone "R" {     type master;     file "/etc/bind/db.R"; };  zone "R-N10" {     type master;     file "/etc/bind/db.R-N10"; }; | 
| --- | 

**Ficheros de datos de las zonas DNS:**

**Archivos: **/etc/bind/db.R **y **/etc/bind/db.R-N10

**Contenido: **

/etc/bind/db.R 

| $TTL 604800 @ IN SOA DN10.local. root.DN10.local. ( 1 604800 86400 2419200 604800 ) @ IN NS DN10.local. @ IN A 192.168.10.10 | 
| --- | 

/etc/bind/db.R-N10

| $TTL 604800 @ IN SOA DN10.local. root.DN10.local. ( 1 604800 86400 2419200 604800 ) @ IN NS DN10.local. @ IN A 192.168.10.10 | 
| --- | 

**Interfaz asignada al DHCP:**

**Archivo:** /etc/default/isc-dhcp-server

**Contenido: La línea INTERFACESv4="enp2s0".**

| INTERFACESv4="enp2s0" | 
| --- | 

**Configuración de la subred y rango DHCP:**

**Archivo: **/etc/dhcp/dhcpd.conf

**Contenido: **Añadimos al final del bloque, subnet 192.168.10.0 netmask ... con el rango (.100 a .200), router (.10) y servidor DNS (.14).

| subnet 192.168.10.0 netmask 255.255.255.0 {     range 192.168.10.100 192.168.10.200;     option routers 192.168.10.10;     option domain-name-servers 192.168.10.14; } | 
| --- | 

**Comandos de verificación**

**Comprobación del estado de los servicios:**

sudo systemctl status named

sudo systemctl status isc-dhcp-server

**Comprobación de resolución de nombres DNS:**

dig @127.0.0.1 R +short

dig @127.0.0.1 R-N10 +short

**Comprobación de conectividad con el router:**

ping 192.168.10.10

**Evidencia**:



![Imagen](./images/image_5.png)



