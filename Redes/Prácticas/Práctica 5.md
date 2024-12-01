
---
# 1
La función de la capa de transporte es la de proveer un conexión lógica entre procesos.

---
# 2
**TCP:**
Se divide en campo de cabecera y campo de datos.
Cabecera
- Puerto de origen + Puerto de destino:
	- Se utilizan para multiplexar y demultiplexar los datos de y para las aplicaciones de la capa superior.
- Número de secuencia y número de Acknowledge/reconocimiento:
	- Campos de 32 bit utilizados por emisor y receptor para proveer fiabilidad en la transmisión.
- Suma de comprobación
- Tamaño ventana:
	- Utilizado para control de flujo.
- Longitud de cabecera:
	- Define la cantidad de palabras de 32 bits que mide la cabecera.
- Flags:
	- bit ACK se utiliza para indicar que el valor transportado en el campo de reconocimiento es válido; es decir, el segmento contiene un reconocimiento para un segmento que ha sido recibido correctamente.
	- Los bits RST, SYN y FIN se utilizan para el establecimiento y cierre de conexiones.
	- La activación del bit PSH indica que el receptor deberá pasar los datos a la capa superior de forma inmediata.
	- El bit URG se utiliza para indicar que hay datos en este segmento que la entidad de la capa superior del lado emisor ha marcado como “urgentes”.
- Puntero de datos urgentes:
	- Campo que indica la posición de este último byte de los datos urgente.

**UDP**
- Número de puerto de origen: 
	- Identifica el puerto del host que envía el mensaje. Este campo es opcional, y puede ser cero si no se necesita respuesta.
-  Número de puerto de destino:
	- Indica el puerto del host que recibe el mensaje. Permite que el host de destino pase los datos de la aplicación al proceso apropiado que está ejecutándose en el sistema terminal de destino.
-  Longitud: 
	- Especifica la longitud total del segmento UDP en bytes, incluyendo la cabecera y el campo de datos. Este valor es necesario debido a que el tamaño del campo de datos puede variar.
-  Suma de comprobación: 
	- Proporciona un mecanismo de detección de errores. Calcula el complemento a uno de todas las palabras de 16 bits del segmento UDP. La suma resultante se almacena en este campo para que el receptor pueda verificar si los datos llegaron sin alteraciones.

---
# 3
Los puertos permiten que múltiples aplicaciones en un mismo dispositivo compartan una única dirección IP. Cada aplicación que envía o recibe datos utiliza un número de puerto específico, que la identifica dentro del sistema. Esto permite que la capa de transporte entregue los datos al proceso correcto en el host receptor mediante un mecanismo llamado "multiplexación y demultiplexación"

---
# 4
 - Confiabilidad:
	 - TCP es un protocolo mas confiable que UDP dado que este cuenta con mas mecanismos para asegurar la correcta recepción de los datos y su orden, como el campo de acknowledgement con número de secuencia, retransmisiones, etc.
 - Multiplexación:
	 - TCP tiene en cuenta 4 elementos (IP y puerto de origen, IP y puero de destino) a la hora de multiplexar, lo que le permite ser mas específico en la dirección hacia el socket adecuado, mientras que UDP utiliza unicamente 2 elementos (IP destino y puerto destino).
 - Orientado a la conexión:
	 - TCP es orientado a la conexión, estableciendo y manteniendo la misma hasta su fin mediante el proceso de *hansdshake*, mientras que UDP no requiere establecimiento previo de comunicación para el envío de datos. 
 - Controles de congestión:
	 - TCP implementa un control de congestión que adapta la velocidad de transmisión según la congestión de la red. Por otro lado, UDP no incorpora control de congestión.
 - Utilización de puertos:
	 - Ambos protocolos utilizan puertos para identificar aplicaciones destino. 

---
# 5
Para simplificar la terminología, nos referiremos a los paquetes de la capa de transporte como segmentos. No obstante, tenemos que decir que, en textos dedicados a Internet, como por ejemplo los RFC, también se emplea el término segmento para hacer referencia a los paquetes de la capa de transporte en el caso de TCP, pero a menudo a los paquetes de UDP se les denomina datagrama. Para simplificar la terminología, nos referiremos a los paquetes de la capa de transporte como segmentos. No obstante, tenemos que decir que, en textos dedicados a Internet, como por ejemplo los RFC, también se emplea el término segmento para hacer referencia a los paquetes de la capa de transporte en el caso de TCP, pero a menudo a los paquetes de UDP se les denomina datagrama.
*Página 156 de Redes de Computadoras, 7ma Edición - James F. Kurose.pdf*

---
# 6
El *three-way handshake* consiste en:
- Primer paso:
	- El cliente envía un segmento especial sin datos con el bit **SYN** activado y un numero de secuencia inicial, indicando que se desea establecer una conexión.
	- Este segmento especial se referencia como un segmento **SYN**
- Segundo paso:
	- Recibido el segmento **SYN**, el servidor asigna recursos para la conexión y envía un segmento con el bit **SYN** activado y el numero de acknowledgement.
	- Este segmento de conexión concedida está diciendo, en efecto, “*He recibido tu paquete SYN para iniciar una conexión con tu número de secuencia inicial, cliente_nsi. Estoy de acuerdo con establecer esta conexión. Mi número de secuencia inicial es servidor_nsi*”. El segmento de conexión concedida se conoce como segmento **SYNACK**.
- Tercer paso:
	- Al recibir el SYN-ACK, el cliente envía un último segmento ACK al servidor, confirmando la conexión establecida. Este segmento tiene el bit **SYN** en 0 y un número de reconocimiento/acknowledgement servidor_nsi+1. A partir de este momento, la conexión está completamente establecida, y ambos hosts pueden comenzar a intercambiar datos​
UDP no establece una conexión por lo que no cuenta con esta característica.
---
# 7
El **ISN** (Initial Sequence Number o Número de Secuencia Inicial) es un valor inicial asignado aleatoriamente en una conexión TCP para establecer la numeración de bytes en la transmisión de datos. Su función es asegurar la integridad y la seguridad de la comunicación, evitando que paquetes de conexiones anteriores sean interpretados incorrectamente en una nueva conexión, lo que podría resultar en errores o ataques de suplantación.

1. **El cliente envía un SYN** (sincronización) que incluye su propio ISN, señalando el inicio de la conexión.
2. **El servidor responde con un SYN-ACK**, confirmando el ISN del cliente y proporcionando su propio ISN, que el cliente debe reconocer.
3. **El cliente envía un ACK** (acuse de recibo) que confirma la recepción del ISN del servidor y asegura que ambos extremos están sincronizados para la transmisión de datos, completando la conexión establecida.

---
# 8
El **MSS** (Maximum Segment Size o Tamaño Máximo de Segmento) es la cantidad máxima de datos que un host TCP está dispuesto a recibir en un solo segmento. El MSS ayuda a optimizar la transmisión al evitar la fragmentación y se negocia durante el proceso de establecimiento de conexión, específicamente en el saludo de tres vías (three-way handshake).

**Negociación del MSS:**
1. Durante el envío del primer mensaje SYN en el saludo de tres vías, el host cliente envía su valor de MSS al host servidor, indicando el tamaño máximo de segmento que puede recibir.
2. El host servidor, en su respuesta SYN-ACK, también incluye su propio valor de MSS. Este intercambio permite que cada host ajuste el tamaño de los segmentos que enviará en función de las capacidades de recepción del otro.

Esta negociación garantiza que los segmentos de datos transmitidos no superen la capacidad de procesamiento de cada dispositivo, optimizando la eficiencia y evitando la fragmentación en la red.

---
# 9
1. **Listar las comunicaciones TCP establecidas:**
   ```bash
   ss -t state established
   ```

2. **Listar las comunicaciones UDP establecidas:**
   ```bash
   ss -u state established
   ```

3. **Obtener solo los servicios TCP que están esperando comunicaciones:**
   ```bash
   ss -t state listening
   ```

4. **Obtener solo los servicios UDP que están esperando comunicaciones:**
   ```bash
   ss -u state listening
   ```

*Para consultar además el proceso relacionado a cada conexión, se agrega `-p`*

Con `netstat`:

1. **Listar las comunicaciones TCP establecidas:**
   ```bash
   netstat -t --tcp --program
   ```

2. **Listar las comunicaciones UDP establecidas:**
   ```bash
   netstat -u --udp --program
   ```

3. **Obtener solo los servicios TCP que están esperando comunicaciones:**
   ```bash
   netstat -t --tcp --program --listening
   ```

4. **Obtener solo los servicios UDP que están esperando comunicaciones:**
   ```bash
   netstat -u --udp --program --listening
   ```

---
# 10