# 1
**a) Asignar IPs a las redes Desa, Invitados y DMZ, desperdiciando la menor cantidad de direcciones. Recordar que: Red infraestructura: DMZ 35 hosts, Red Desa: 62 hosts y Red Invitados: 119 hosts. Las tres deben tener redes clase A, públicas.**

### Red invitados
85.85.84.0/23, es pública, clase A y cuenta con una máscara para $2^9-2=510$ hosts.
Podemos aumentar la máscara hasta /25, con espacio para $2^7-2=126$ hosts, suficientes.
Por lo que:
	Red invitados cuenta con la red: **85.85.84.0/25**

### Red Desa
Podemos utilizar la siguiente red de la invitados:
	**85.85.84.0/25** -> la siguiente es -> **85.85.84.128/25**
Pero además podemos utilizar un bit mas para subnetting:
	**85.85.84.128/26**
Gracias a esto ajustamos a $2^6-2=62$ hosts, suficientes.  
Por lo que:
	Red desa cuenta con la red: **85.85.84.128/26**

### Red DMZ
Podemos utilizar la subred siguiente de la *Red Desa*:
	 **85.85.84.128/26** -> la siguiente es -> **85.85.84.128/26**  + cant de hosts por subred = 64 = **85.85.84.192/26** 
 Por lo que:
	 Red DMZ cuenta con la red: **85.85.84.192/26** 

b) Asignar IPs a todas las redes entre routers, (ignore RBORDE/Internet), desperdiciando la menor cantidad de direcciones, utilizando redes clase B, privadas. 

**172.30.128.0/28** es la única privada de la clase B

### RBORDE-RDMZ-RINVITADOS
Necesito 3 hosts por lo que con /29 es suficiente.
**172.30.128.0/29** es esta red.

### RBORDE-RDESA
Utilizo la siguiente red despues de la **172.30.128.0/29** que es la **172.30.128.8/29**
Puedo subnettear porque solo requiero 2 hosts:
**172.30.128.8/30** es la red. 

c) Asignar IPs a todos los dispositivos del gráfico en todas sus interfaces (ignore RBORDE/Internet). Para las redes públicas la primera IP disponible debe ir al router. 

### Red Invitados
**85.85.84.0/25**
n21 = 85.85.84.1/25
n18 = 85.85.84.2/25
n19 = 85.85.84.6/25
n20 = 85.85.84.7/25
RINVITADOS/eth1 = 85.85.84.14/25

### Red Desa
**85.85.84.128/26**
n9 = **85.85.84.129/26**
n10 = **85.85.84.130/26**
n11 = **85.85.84.131/26**
RDESA/eth2 = **85.85.84.136/26**

### Red DMZ
**85.85.84.192/26**
ServerMail = **85.85.84.193/26**
ServerDNS = **85.85.84.194/26**
ServerWeb = **85.85.84.195/26**
RDMZ/eth0 = **85.85.84.200/26**

### Red RBORDE-RDMZ-RINVITADOS
**172.30.128.0/29**
RDMZ/eth1 = **172.30.128.1/29**
RINVITADOS/eth0 = **172.30.128.2/29**
RBORDE/eth2 = **172.30.128.3/29**

### Red DESA-BORDE
**172.30.128.8/30**
RDESA/eth1 = **172.30.128.9/30**
RBORDE/eth0 = **172.30.128.10/30**

d) ¿Cuántos dominios de broadcast hay en el diagrama? (Ignore RBORDE/Internet) 
Hay tantos broadcast como redes, entonces hay **5**
- **85.85.84.127/25**
- **85.85.84.191/26**
- **85.85.84.255/26**
- **172.30.128.7/29**
- **172.30.128.11/30**

e) ¿Cuántos dominios de colisión hay en el diagrama? (Ignore RBORDE/Internet)**
**17** ya que:
switch1 = 4
switch2 = 3
switch3 = 4
switch5 = 3
switch6 = 2 (el e1 no cuenta dado que va a un hub)
hub1 = 1

# 3
##### ARP request:
Trama Ethernet
Mac origen: mac_dmz_eth1 mac destino: FF:FF:FF:FF:FF:FF

Trama ARP:
Mac origen: mac_dmz_eth1 ip origen:172.30.128.1
Mac destino: 00:00:00:00:00:00 Ip destino: 172.30.128.2

##### ARP reply:
Trama ethernet
Mac origen: mac_invitados_eth0 MAc destino: mac_dmz_eth1

Trama ARP:
Mac origen: mac_invitados_eth0 ip origen:172.30.128.2
Mac destino: mac_dmz_eth1 ip destino:172.30.128.1

# 4
a)
	GET *recurso:* **a.html** *versión:* **1.1**
	HOST : *url:* **https://mail.redes.unlp.edu.ar/**
	**User-agent** : Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:127.0) Gecko/20100101 Firefox/127.0
b)
	Aplicación: **HTTP 1.1**, **DNS**
	Transporte: **TCP**, **UDP/TCP**
	Red: **IP**
*Nosotros entendemos que antes de ejecutar la petición HTTP, se debe resolver el nombre con DNS.*
c)
	i) Si
	ii) No, porque no se cumple con el header If-Modified-Since
	iii) El cliente puede continuar usando la misma versión almacenada en su caché.

# 6
a) Falso, todos los puertos tienen un dominio de colisión distinto, caso contrario ocurre con los hubs.
b) Falso, el campo TTL se utiliza para determinar la cantidad de saltos que puede hacer un paquete IP hasta que se descarte.
c) Falso, 01010101.01010101.01010101.01010101 AND 11111111.11111111.11111111.11100000 = 01010101.01010101.01010101.01000000
	La dirección correcta es 85.85.85.95/27
d) Falso, el orden de los pasos es incorrecto, se consulta el SPF para verificar autenticidad y luego se consulta el registro MX.
e) Falso, puede darse el caso de que uno de los dos siga enviando o recibiendo paquetes.