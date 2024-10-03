
## Componentes de un Sistema de Comunicación
- ==Fuente==
- Emisor/transmisor
- Medio de transmisión/Dispositivos intermedios
- Procesos intermedios que tratan la información
- Receptor
- ==Destino==

## Protocolos
Conjunto de reglas que los dispositivos y programas deben seguir para comunicarse entre si.
*Protocolo de red*
Protocolo orientado al campo informático.

## Combinación de Protocolos
Surge los **Modelos de Organización**, en especial **Modelo en capas**:
- Abstracción entre componentes
- Reusabilidad de capas independientes
- Protección frente a cambios

## Modelo OSI:
- Modelo de referencia.
- Se divide en 7 capas.
	- Aplicacion
	- Presentacion
	- Sesión
	- Transporte
	- Red
	- Enlace de datos
	- Fisica

## Modelo TCP
- Dividido en 5 capas
	- Aplicación
	- Transporte
	- Red
	- Enlace de datos
	- Física

## Capa de aplicación
- Interfaz al usuario
- *Elementos*:
	- Programas
- Capa de presentación:
	- Procesa los datos.

---
# Protocolo HTTP
*Funcionamiento*
- Cliente/servidor
- Request/response
- Stateless
- Sobre TCP
- Mensajes en ASCII

*Clientes*: Browsers
*Servidores*: Empresas que dan servidores (Apache, Google, etc)

# HTTP 0.9
- Solo una forma de Req (GET)
- Solo una forma de Res
- Una conexión TCP/requerimiento

# HTTP 1.0
- Se incorporan nuevos métodos de Req
- Define códigos de Res
- Por default NO se utilizan conexiones persistentes
- *Métodos*:
	- **GET**: Obtener el documento requerido, pasando parámetros por la URL.
	- **HEAD**: *GET* pero solo obteniendo los *Headers* de la respuesta.
	- **POST**: Hace un requerimiento de un documento, pasando información en el body.
	- **PUT**: Utilizado para reemplazar un documento en el servidor.
	- **DELETE**: Utilizado para borrar un documento en el servidor.
	- **LINK/UNLINK**: Manejar relaciones entre documentos.
- *Autenticación*
	- Se maneja con header WWW-Authenticate

# HTTP 1.1
- Nuevos métodos:
	- OPTIONS
	- TRACE
	- CONNECT
- Pipelining.
	- No necesita esperar la respuesta para realizar otro req.
	- Solo con conexiones persistentes.
	- Se debe mantener el orden de los objetos que se devuelven.
- Conexiones persistentes por omisión.

## Cookies
- Simula un estado.
- El cliente hace una request.
- Header *Set-Cookie* establece la cookie en el cliente.

## HTTPS
- Etapa de negociación previa, donde se establece la *Secret Key.*
- Cifrado de los mensajes.

## Web Cache
- Permite almacenar recursos utilizados para reducir los tiempos de respuesta.

# HTTP 2.0
*Problemas de HTTP 1.0 1.1*:
- Posible HOL

## *Funcionamiento*
- Protocolo binario en los mensajes
- Multiplexación de mensajes
	- Se divide en streams.
		- Estos contienen frames.
		- Pueden tener diferentes *weight*/prioridad
- Compresión de encabezado
- Priorización de datos


# HTTP 3.0
- Incorpora QUIC

---
# DNS
- Elementos:
	- Espacio de nombres
	- Base de datos distribuida
	- Servidores
	- Protocolos de búsqueda

## *FQDN (Fully-Qualified Domain Name)*
- Lista de labels separadas por puntos
- A la izquierda está la hoja y a la derecha la raiz (.)

## *TLD (Top-Level-Domains)*
- **Genéricos**: Orientadas a actividades particulares.
	- Sponsored: Patrocinado especifico orientado a las reglas para su uso (.edu, .gov, .mil, etc.)
	- Unsponsored: No requieren un patrocinador (.com, .net, .org, .info, etc)
- **Paises**: Reservados para paises. (.ar, .uk, .br, etc.)
- **Arpa**

## *Base de Datos Distribuida*
- Mantener gran cantidad de información.
- Control de la información distribuido.
- Varios lugares donde se almacena la información.
	- Sigue la estructura de arbol.
	- Existen 13 *ROOT SERVERS*.

## *Funcionamiento DNS*
- Cliente/servidor
- Req/res
- Corre sobre **UDP** y **TCP**

## *Tipos de Servidores DNS*
- **Raiz**: Delega a todos TLD.
- **Autoritativo**: Dueño del recurso original (*No cacheado*).
- **Local/Resolver Recursivo**: Se le consulta dentro de una red.
	- Mantiene una caché. 
	- Conecta con la red no local.
- **Open Name Servers**: Servidores de DNS ajenos a la red local pero que admiten conexiones como tales.
	- *Cuando el Local Server se conecta con un OPS, la conexión entre ambos deja de ser iterativa, ya que el OPS se encarga de buscar el requerimiento.*
- **Forwarder Name Server**: Conecta al Local Server con los servidores DNS externos, reduciendo la carga sobre el mismo.
	- *Ocurre lo mismo entre Local-FNS como aclarado entre Local-OPS*
- **Servidores secundarios y primarios**: Distribución de carga, redundancia, seguridad, etc.

## *Estructura de mensaje DNS:*
![[Pasted image 20240925120642.png]]

### *Registros almacenados por los servidores DNS*
- **A / AAAA**: Nombre mapeado con IP / Nombre mapeado con IPv6
- **PTR**: IP mapeado con Nombre
- **CNAME**: Nombre mapeado con Nombre (*Cuello de botella donde N dominios redirigen a 1 dominio, por ej "thefacebook.com" redirige a "facebook.com*)
- **MX**: Nombre-dom a mail-server (*Dado un dominio, el servidor responde con el servidor/exchanger de mail que puede avanzar en la consulta.*)
- **NS**: Nombre-dom a dns-server (*Lo mismo que el MX pero para servidores DNS.*)
- **SOA** (Start Of Authority): Parámetros de dominio.
- **TTL** (Time to live): Tiempo que se puede almacenar una dirección en caché. 

---
# FTP
- Es un protocolo para copiar archivos completos, a diferencia de otros que brindan acceso a archivos.
- Mensajes codificados en ASCII estándar.
- Modelo:
	- *Cliente/Servidor.*
	- *Command/Response.*
- Corre sobre **TCP**
- Los clientes no requieren interfaz gráfica.


## Funcionamiento
- Utiliza dos conexiones **TCP**:
	- **Control**: puerto 21.
		- Requiere delay minimo.
		- Es por donde se transmite el estado de cada operación.
	- **Transferencia de datos**.
		- Requiere throughput máximo.
		- Se crea y cierra bajo demanda.
- El cliente escoge cualquier puerto *no privilegiado* (n > 1023) y genera conexión de control contra el puerto 21 del servidor.
- El servidor recibe los comandos por la conexión de **control** y responde/recibe datos por la conexión de **datos**.


## Comandos
- **RETR**: Obtiene un archivo desde el servidor.
	- Requiere conexión de datos.
	- El comando que lo inicia a nivel de interfaz de usuario es el *GET*.
- **STOR**: Envia una archivo al servidor.
	- Requiere conexión de datos.
	- El comando que lo inicia a nivel de interfaz de usuario es el *PUT*.
- **LIST**: Envia una petición de listar los archivos del directorio actual en el servidor.
	- No requiere conexión de datos.
	- El comando que lo inicia a nivel de interfaz de usuario es el *LS* o *DIR*.
- **DELE**: Envia una petición de eliminar un archivo en el servidor.
	- No requiere conexión de datos.
- **STRU**: Especifica el formato para transferencia de datos.
- **MODE**: Especifica una codificación adicional sobre los datos transmitidos.
- **Otros comandos**: *SIZE, STAT, CD, PWD, RMD, MKD*
	- No requieren conexión de datos.

## Modalidades
- **FTP Activo**:
	- Puertos de conexión:
		- Control: 21.
		- Datos: 20.
	- El servidor de forma **activa** se conecta al cliente para generar la conexión de datos.
- **FTP Pasivo**:
	- Puertos de conexión:
		- Control: 21
		- Datos: Port no privilegiado.
	- El servidor de forma pasica le indica al cliente a que nuevo puerto debe conectarse.
- Ejemplo: ![[Pasted image 20240930090442.png]]


## Formato de datos
*Debido a los diferentes tipos de plataformas, existen varias representaciones en las que un archivo puede ser convertido.*
- El formato default es **ASCII**.
	- El más común hoy en dia es **Image**
- FTP define estructuras para transportar datos.
	- Formato **FILE**

## Alternativas a FTP
- Más seguras
	- **FTPS**: FTP sobre SSL/TSL.
- Integradas con *Open-SSH*
	- **SCP**
	- **SFTP**
- Aplicación para compartir recursos
	- **NFS**
	- **SMB/CIFS**

## FTP vs HTTP

|                     FTP                      | HTTP                              |
| :------------------------------------------: | --------------------------------- |
| Formato ASCII/Binario o elección del cliente | Files, Content-Type, mas flexible |
|              No maneja headers               | Maneja headers                    |
|               Command/Response               | Pipelining                        |
|          Con firewall mas complejo           | Mas amigable frente a firewalls   |
|        Conexiones de datos y control         | Una única conexión                |
|            Soporta autenticación             | Soporta autenticación             |


---
# EMAIL

**Componentes**:
- **MUA**: User Agent
	- Cliente con su UI.
	- Agrega la mayoría de headers (To, From, Date, Subject)
	- Integra un **MTA local**.
		- Este se comunica con el servidor de mail saliente (**MTA** que hace *relay*)
	- Integra un **MRA**.
		- Este se comunica con el servidor de mail entrante (**MAA**)
- **MSA**: Submission Agent
	- Servidor
	- Habitualmente integrado en el **MTA**.
	- Recibe el mensaje del **MUA** y lo pre-procesa antes de pasarlo al **MTA**.
- **MTA**: Transport Agent
	- Cliente y servidor.
	- Recibe el email desde el **MSA**.
- **MDA**: Delivery Agent
	- Servicio interno o servidor.
	- Tambien llamado **LDA**.
	- Protocolos:
		- **POP**
		- **IMAP**
- **MAA**: Access Agent
	- 
- **Otros componentes**: Servidor de autenticación externo, web mail, antivirus, antispam, etc.
**Protocolo SMTP**
*Simple mail transport protocol*
