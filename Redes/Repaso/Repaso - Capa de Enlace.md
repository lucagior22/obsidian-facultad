### Temas
- No entra WIFI
- Detección de errores, saber conceptualmente.

### Intro
- Nodo = Routers y Hosts
- Links/Enlace = Comunicaciones entre nodos
- PDU = Frame/Trama

### Servicios
- **Básico**: Mover un datagrama desde un nodo a otro **adyacente** a través de un único enlace de comunicaciones.
- **Entramado**: Se encapsulan los datos de la capa de Red en las tramas de la capa de enlace.
	- En arp se ve que en el paquete de capa de enlace se guardan datos correspondientes a la capa de red.
- **Acceso al enlace**: Un protocolo MAC permite la coordinación en la transmisión de las tramas a múltiples nodos.
- **Entrega fiable**: Este servicio proporciona reconocimientos o retransmisiones para proveer conexiones sin errores.
- **Control de flujo**: Los nodos tienen buffers con una capacidad limitada de almacentamiento. Se puede proveer de mecanismos de control de flujo para evitar que un nodo abrume al otro.
- **Detección de errores**: Mediante bits de control, se puede detectar si una trama contiene datos alterados.
- **Corrección de errores:** Detección de errores sumado a un mecanismo para identificar los sectores erróneos.
- **Semiduplex y fullduplex:** 
	- *Fullduplex*: ambos nodos de un enlace pueden transmitir paquetes al mismo tiempo.
	- *Semiduplex*: un mismo nodo no puede transmitir y recibir al mismo tiempo.

### Donde se implementa la capa de enlace
- **NIC**: (Network Interface Card) adaptador de red.
	- Son los que representan a los nodos.
- **Comunicación entre adaptadores:** 
	- Emisor: Encapsula el datagrama en una trama y agrega bits de chequeo de error, rdt, control de flujo, etc.
	- Receptor: Busca errores, rdt, control de flujo, y extrae el datagrama y lo pasa a las capas superiores.

### Detección de errores
- **EDC**: redundancia
- **D**: Chequeo de errores. No es completamente confiable.
- **Chequeo de paridad**:
	- 1 dimension (1 bit): Detecta errores en 1 bit, segun si el esquema es par o impar, se agrega un bit que representa la paridad de la cantidad de 1s en los datos.
	- 2 dimension (2 bits): Como el 1 dimension, pero se agrega 1 bit por fila y 1 por columna, haciendo mas preciso el chequeo.
- **Internet Checksum**: Como en capa de transporte, suma de bits y comprobación.
- **CRC**: (Cyclic Redundancy Check) Chequeo de errores ciclico. Los nodos acuerdan un polinomio (código binario) que se utiliza en el calculo aplicado sobre los datos crudos que el emisor quiere enviar. Aplicado el cálculo sobre los datos, se obtiene el **CRC** y se agrega al final de los datos. El receptor conoce el polinomio y recibe los datos+CRC, calcula dividiendo sobre lo recibido y si el resto es 0 la transmisión no tiene errores.
	- Es un método para la **detección de errores cíclicos** en la transmisión de datos. Los nodos (o dispositivos) acuerdan un **polinomio generador** (representado en forma binaria) que se utiliza para realizar una operación de división polinómica sobre los datos que el emisor desea enviar. El resultado de esta operación, conocido como **CRC**, se añade al final del mensaje. El receptor, que conoce el polinomio generador, recibe el mensaje compuesto por los datos y el CRC, y realiza la misma división. Si el residuo obtenido es cero, se asume que la transmisión se realizó sin errores; de lo contrario, se detecta la presencia de errores.

### Protocolo de acceso múltiple
- Existen dos tipos de enlaces: 
	- Punto a punto
	- Broadcast/difusión
- Colisión: Dos nodos o más transmiten tramas al mismo tiempo.
- Clasificaciones de protocolos de acceso múltiple:
	- *Protocoles de particionamiento del canal*
	- *Protocolos de acceso aleatorio*
	- *Protocolos de toma de turnos*

### Protocolo de acceso múltiple: particionamiento del canal
- Dos técnicas:
	- TDM: Multiplexación por división en el tiempo.
		- Se divide el tiempo en **marcos temporales** y estos se subdividen en *N* **particiones de tiempo.**
		- Cada partición de tiempo se asigna a uno de los *N* nodos.
		- Inconvenientes:
			- No tiene en cuenta que hay nodos que requieren mas uso que otras, todos tienen el mismo tiempo.
	- FDM: Multiplexación por división de frecuencia.
		- Se divide el canal en *N* frecuencias (sub-banda independiente, como en una radio 10mhz-20mhz, etc)
		- Inconveniente:
			- Se asignan recursos a nodos que pueden no necesitarlos.

### Protocolo de acceso múltiple: acceso aleatorio
- Cada nodo transmisor usa el canal siempre a la máxima velocidad.
- Cuando se produce una colisión, cada uno de los nodos implicado espera un **tiempo aleatorio** antes de retransmitir la trama. 
- El tiempo aleatorio es elegido independientemente por cada nodo, por lo que es baja la probabilidad de que ambos nodos tengan el mismo retardo y vuelvan a colisionar.