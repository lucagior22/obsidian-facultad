# **Arquitectura de memoria distribuida**
*procesadores + memoria local al procesador + red de comunicaciones + mecanismo de comunicación/sincronización*

## **Intercambio de mensajes**
*Mecanismo de comunicación/sincronización*

Cuando un programa implementa este mecanismo es un **Programa Distribuido**.
	Estos programas comparten **SOLO** canales (físicos o lógicos)

*Primitivas de pasaje de mensajes*
Interfaz con el sistema de comunicaciones
- Semáforos
- Datos
- Sincronización

## **Canales**
*Los procesos SOLO comparten los canales, cada variable es local al proceso.*

*Carácteristicas/variantes de canales compartidos*
- **Mailbox, input port, link.**
	- Mailbox
		- Todos los procesos conocen los canales.
		- Los procesos pueden enviar y recibir.
	- Input port
		- Un único proceso puede ser receptor.
		- Todos los demas emisores.
	- Link (punto a punto)
		- El canal conecta un proceso a un proceso.
		- Para comunicar a *n* procesos entre si se necesitarian $2^n$ canales.
		- 
- **Uni o bidireccionales.**
- **Sincrónicos o asincrónicos.**

- La exclusión mutua no requiere mecanismo especial.
- Los procesos se interactúan comunicandose.
- Los canales son accedidos por primitivas de envío y recepción.

## **Mecanismos para el procesamiento distribuido**
- **PMA**: Pasaje de mensajes asincrónicos.
	- Mailbox.
	- Asincrónicos.
	- Unidireccionales.
- **PMS**: Pasaje de mensajes sincrónicos.
	- Link.
	- Sincrónicos.
	- Unidireccionales.
- **RPC**: Llamado a procedimientos remotos.
	- Link.
	- Sincrónicos.
	- Bidireccional.
- **Rendezvouz**
	- Link.
	- Sincrónicos.
	- Bidireccional.

## **Patrones de interacción**
- **Productores y consumidores**
	- *Procesos filtros*.
		- Procesan información.
	- *Procesos pipe*.
		- Reciben el producto.
- **Cliente/servidor**
	- *Procesos cliente*
		- Realiza pedidos o solicitudes.
	- *Proceso servidor*
		- Recibe peticiones y responde a cada cliente.
- **Pares que interactuan**
	- *Todos los procesos del mismo nivel*

## **Relación entre mecanismos de sincronización**
![[Pasted image 20240926105348.png]]
