
## **PMA**
*Canales/colas de mensajes enviados y aún no recibidos*

### **Declaración**
`chan ch (id1: tipo1, ..., idn : tipon)`

### **Operaciones**
- **Send**: Un proceso agrega un mensaje al final de la cola de un canal.
	- No bloquea al emisor.
	- `send ch(expr1, ..., exprn)`
- **Receive**: Un proceso toma un mensaje de la cola de un canal.
	- El proceso es bloqueado hasta que haya al menos un mensaje.
	- `receive ch(var1, ..., var2)`
- **Empty**: Determina si la cola de un canal está vacia.
	- Permite no bloquearse con el receive.
	
- Las variables deben matchear para comunicar procesos.
- Las operaciones son atómicas.
- Las operaciones respetan FIFO.
- Se supone que los mensajes no se pierden ni se modifican.
- 

## **Características de los canales**
*Segun son declarados globales, pero segun su utilización se pueden considerar tanto Mailbox, Link o input port*.

### **Productores y consumidores**
*Red de ordenación*
- **Filtro
	- Recibe a uno o mas canales de entrada.
	- Envia mensajes a uno o mas canales de salida.
 