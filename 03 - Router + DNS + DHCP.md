# 03 - Router + Servidor DNS + DHCP

## - Introducció

# Configuración del Router, DHCP, DNS y Cortafuegos (`nftables`)

En el router **R-N10**, se configura el enrutamiento IPv4, los servicios DHCP y DNS (`bind9`), y el filtrado de paquetes con `nftables` definiendo las políticas de paso y bloqueo entre las zonas de red (**WAN**, **Intranet** y **DMZ**), integrando las reglas de NAT/enmascaramiento para mantener la salida a Internet.

---

## 1. Configuración inicial del Router

### Creación y permisos del usuario `bchecker`
Añadimos el usuario `bchecker` al grupo `sudo` para administrar el sistema:

![Creación del usuario bchecker](../imagenes/01-crear-usuario-bchecker.png)

### Activación del reenvío IPv4 (`ip_forward`)
Habilitamos el reenvío de paquetes entre interfaces mediante `sysctl`:

![Activación de ip_forwarding](../imagenes/02-activar-ip-forward.png)

### Actualización del sistema e instalación de paquetes base
Actualizamos los repositorios e instalamos `iptables-persistent`:

![Actualización e instalación de paquetes](../imagenes/03-instalar-iptables-persistent.png)

### Reglas iniciales de filtrado y persistencia
Definimos las reglas iniciales para el tráfico DNS (puerto 53 UDP/TCP) en la interfaz DMZ (`enp3s0`) y guardamos la configuración:

![Reglas iniciales y persistencia](../imagenes/04-reglas-iptables-dns-persistencia.png)

---

## 2. Configuración del servidor DHCP (`isc-dhcp-server`)

### Instalación del servicio DHCP
Instalamos el paquete `isc-dhcp-server`:

![Instalación de isc-dhcp-server](../imagenes/05-instalar-isc-dhcp-server.png)

### Configuración de la interfaz de escucha
Editamos `/etc/default/isc-dhcp-server` para indicar que el servicio escuche en la interfaz de la Intranet (`enp2s0`):

![Configuración de interfaz DHCP](../imagenes/06-configurar-interfaz-dhcp.png)

### Definición de la subred y rango de direcciones
Configuramos `/etc/dhcp/dhcpd.conf` para el dominio `empresa.local`, la subred `192.168.20.0/24` (rango `.30` a `.100`), puerta de enlace `192.168.20.10` y servidor DNS `192.168.20.10`:

![Configuración de dhcpd.conf](../imagenes/07-configurar-dhcpd-conf.png)

### Reinicio y habilitación del servicio DHCP
Reiniciamos el servicio y lo habilitamos en el arranque del sistema:

![Reinicio y habilitación de isc-dhcp-server](../imagenes/08-reiniciar-habilitar-dhcp.png)

---

## 3. Configuración del servidor DNS (`bind9`)

### Instalación de `bind9`
Instalamos los paquetes necesarios para el servidor DNS (`bind9`, `bind9utils`, `bind9-doc`):

![Instalación de bind9](../imagenes/09-instalar-bind9.png)

### Configuración de reenviadores (*forwarders*)
Editamos `/etc/bind/named.conf.options` añadiendo los servidores DNS externos (`8.8.8.8` y `8.8.4.4`):

![Configuración de forwarders en named.conf.options](../imagenes/10-configurar-forwarders-dns.png)

### Declaración de la zona directa `empresa.local`
Configuramos la zona maestra en `/etc/bind/named.conf.local`:

![Declaración de zona en named.conf.local](../imagenes/11-declarar-zona-empresa-local.png)

### Creación del archivo de zona
Copiamos la plantilla `db.local` para crear `/etc/bind/db.empresa.local`:

![Copia de plantilla db.local](../imagenes/12-copiar-plantilla-db-local.png)

### Registros DNS de la zona `empresa.local`
Definimos los registros `SOA`, `NS`, `A` y `CNAME` para el router (`R-N10`), clientes de Intranet (`win-cli`, `lin-cli`) y servidores de la DMZ (`web`, `bbdd`):

![Archivo de zona db.empresa.local](../imagenes/13-registros-dns-empresa-local.png)

### Validación de la sintaxis de la zona
Verificamos que la zona `empresa.local` no contiene errores con `named-checkzone`:

![Comprobación con named-checkzone](../imagenes/14-validar-zona-named-checkzone.png)

---

## 4. Comprobaciones de red y clientes

### Configuración de red en cliente Linux (Intranet)
Comprobamos la asignación por DHCP en el cliente Ubuntu (`192.168.20.32`, puerta de enlace `192.168.20.10` y DNS `192.168.20.10`):

![Configuración de red cliente Ubuntu](../imagenes/15-red-dhcp-cliente-ubuntu.png)

### Prueba de salida a Internet desde cliente Linux
Verificamos la conectividad hacia `8.8.8.8`:

![Ping a 8.8.8.8 desde cliente Ubuntu](../imagenes/16-ping-internet-cliente-ubuntu.png)

### Configuración de red y salida a Internet en cliente Windows
Comprobamos `ipconfig /all` (`192.168.20.31`, dominio `empresa.local`) y la conectividad `ping 8.8.8.8`:

![Configuración IP y ping en cliente Windows](../imagenes/17-ipconfig-ping-cliente-windows.png)

### Resolución DNS hacia Internet desde cliente Windows
Probamos la resolución de nombres externa con `ping www.google.es`:

![Ping a www.google.es desde cliente Windows](../imagenes/18-ping-dns-google-windows.png)

---

## 5. Interfaces del Router y transición a `nftables`

Verificamos las interfaces de red del router (`enp1s0` WAN, `enp2s0` Intranet `192.168.20.10/24`, `enp3s0` DMZ `192.168.10.10/24`), limpiamos las tablas heredadas de `iptables` y habilitamos el servicio `nftables`:

![Interfaces del router y activación de nftables](../imagenes/19-interfaces-router-activar-nftables.png)

---

[← 02 - Direccionament de IPs](<02 -  Direccionament de IPs.md>) ------------------------------------------------------------------------------------------------- [04 - Servidor Web →](<04 - Servidor Web.md>)
