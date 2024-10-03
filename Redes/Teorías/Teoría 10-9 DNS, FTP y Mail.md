
## Tipos de servidores

- Raiz: Delega a todos Top Level Domains
- Autoritativo: TIene una zona o sub-dominio de nombres a cargo
- Servidor Local/ Resolver Recursivo: Un servidor que es consultado dentro de una red. 
	- Mantiene una caché. 
	- Puede ser *Servidor Autoritativo*. 
	- Permite recursivas internas.
	- Tambien llamado *Caching Name Server*
- Open Name Servers: Servidores de DNS que funcionan como locales para cualquier cliente
	- Por ejemplo: 8.8.8.8 y 8.8.4.4 (Google)


## FTP
*File Transfer Protocol*

- Modelo cliente/servidor
- Se utilizan 2 conexiones TCP
	1. Conexión de control
	2. Conexión de transferencia de datos
- La conexión de datos se crea y se cierra bajo demanda.
- El estado de cada operación se transmite por el canal de control.
- Protocolo con estados

**Comandos FTP**
- RETR: Obtener archivo
- STOR: Enviar archivo.
- LIST: Simil ls
- DELE: Borrar un archivo del servidor
- SIZE, STAT: Obtener datos
- CD, PWD, RMD, MKD

**Modos de ejecución de FTP**
	*Activo*
		- Se diferencia como maneja la conexión de datos: El servidor de forma activa se conecta al cliente para generar la conexión de datos.
	*Pasivo*
		- El servidor indica al cliente a que nuevo puerto debe conectarse
		
![[Pasted image 20240909190403.png]]

Comandos de FTP que requieren conexión de datos:
- STOR: Enviar un archivo al servidor
- RETR: Traer un archivo desde el servidor

Diferencias entre FTP y HTTP
- FTP cuenta con estado
- HTTP puede tener conexión persistente mientras que la conexión de datos de FTP no.
- El contenido de FTP puede ser binario o ASCII, HTTP2 tiene contenido binario.
- FTP no maneja Headers.
- FTP cuenta con dos conexiones.
- Para que el funcionamiento de FTP sea correcto se debe configurar Firewall y NAT. 

![[Pasted image 20240909191244.png]]

Alternativas a FTP
- FTPS: Version segura de FTP en SSL/TSL
- SFTP: Integrada con la suite de Open-SSH


## Mensajería E-mail

**Componentes**:
- MUA: User Agent
	- Cliente con su UI
	- Agrega la mayoría de headers (To, From, Date, Subject)
- MSA: Submission Agent
	- Servidor
	- Habitualmente integrado en el *MTA*
	- Recibe el mensaje del *MUA* y lo pre-procesa antes de pasarlo al *MTA*
- MTA: Transport Agent
- MDA: Delivery Agent
- MAA: Access Agent

*Componentes secundarios:*
- Servidor de autenticación externo
- Web Mail
- Anti-Virus
- Anti-Spam

**Protocolo SMTP**
*Simple mail transport protocol*

- Cliente/Servidor