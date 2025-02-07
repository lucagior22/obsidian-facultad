# IPv4
- No orientado a conexión
- Best-effort
	- No confiable
- PDU = Datagrama
- Funcionalidades:
	- Direccionamiento
	- Ruteo/forwarding/switching
	- Mux/demux
	- Fragmentación
	- Detección de errores
- Estructura del datagrama:
	- Version / Header length / DS (Prioridades en la red) / ECN (Indica congestión en la red) / Total Length 
	- ID (Utilizado para fragmentación) / Flags (Fragmentación) / Fragment offset (identificador de cada fragmento)
		- El datagrama puede fragmentarse según el tamaño máximo permitido por la red.
		- Todos los fragmentos comparten ID.
		- Los flags indican la manera de proceder.
	- TTL / Protocol (capa superior) / Checksum
	- IP Destino / IP Destination
	- Payload

# Direccionamiento IP
- Identifica unívocamente un punto de acceso a la red.
	- A un puerto/interfaz, NO a un dispositivo !!
- Numeros de 32 bits en notación decimal separada por puntos byte a byte.
	- $2^{32}$ direcciones
- Se mapean las IPs con nombres de dominio DNS
- Son direcciones lógicas.
- Codificadas en dos partes:
	- Red
	- Host
- Tipos de direcciones
	- Unicast.
	- Broadcast.
	- Multicast.
		- Destinada a un grupo de hosts en una red o varias redes.
	- Anycast.
		- Destinada al primero que resuelva.
- Redes privadas:
	- No deberian pasar a internet.
	- Filtradas por routers borde.
- Direccionamiento fijo:
	- No contempla el cambio de máscara.
	- Desventajas:
		- Desperdicio de espacios de direcciones.
		- Falta de escalabilidad.

# CIDR
*Classless Inter Domain Routing*
- Agrupa subredes contiguas en una red grande.
- Supernetting ISP
	- Asignación por regiones geográficas.
		- ! Supernetting no solo es solo para este uso.

# Ruteo
- El **ruteo** diseña las rutas en toda la red, mientras que el **forwarding** mueve los paquetes individuales en esas rutas.
- Tareas de ruteo:
	- Validación de datagrama.
		- Se valida la estructura del datagrama.
	- Cálculo del checksum.
	- Leer IP destino.
	- Mapear IP destino en la tabla de ruteo.
		- Buscando el best-match.
	- Decrementar TTL.
		- Se decrementa en uno el TTL.
		- Si TTL te llega en 0 y no sos la IP destino se descarta el paquete.
	- Fragmentar (alternativo)
		- Si es necesario
	- Transmitir o descartar.
		- De no mapear una ruta válida se descarta el paquete, sino se transmite.
	- Generar ICMP (alternativo)
		- De ocurrir un error (TTL en 0 o ruta inválida) se notifica al remitente mediante un mensaje ICMP.

# IPv6
![[Pasted image 20241211091425.png]]
| Cabecera IPv4 | Cabecera IPv6 |
| --- | --- |
| TTL | Hop Limit |
| Extensible | Tamaño fijo |
| Checksum | Sin checksum |

