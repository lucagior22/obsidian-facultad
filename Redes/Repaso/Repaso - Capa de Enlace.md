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

### Protocolo de acceso múltiple: toma de turnos
- Passing de baton

### Redes locales conmutadas
- Utilización de switches.
- La conmutación (Switches) permite que los datos se envien directamente de origen a destino, mejorando la eficiencia.
	- Por eso, un hub no conmuta, ya que envia los paquetes a todos sus adyacentes.

### Ethernet
- No orientado a conexión.
- No confiable.
- Protocolo MAC: CSMA/CD
- Estructura del datagrama:
	- Preambulo: utilizado para despertar al receptor y sincronizar los relojes de emisor y receptor.
- Direcciones:
	- 6 bytes cada una.
- Type:
	- 2 bytes.
	- Multiplexación, indica el protocolo de capa superior.
- Data:
	- 46 a 1500 bytes.
- CRC:
	- 4 bytes.
	- Para calcularlo se usa todo menos el preamble.

---
## Dispositivos
### Hubs
- Repetidor tonto.
- La información que entra por un enlace sale por todos los otros enlaces.

### Switch
- Permite multiples conexiones simultáneas.
- Tiene un rol activo en la red:
	- Pueden almacenar.
	- Multiplexación permite un envio selectivo de la trama a uno o mas enlaces de salida.
- Transparente para los hosts.
- Self-learning
	- Aprende que hosts puede ser alcanzado a través de qué interfaces.
	- Mapea interfaces con hosts a medida que recibe tramas.
- Forwarding de tramas:
	- Busca la MAC destino en su tabla de switch.
	- Si encuentra una entrada para el destino envia los datos por el enlace correspondiente, sino emite un broadcast.
- Técnicas de conmutación de tramas:
	- De que manera un Switch redirige una trama desde la entrada hasta la salida.
	- **Cut through**:
		- Sólo espera a la *destination address*, no a la trama completa para conmutar.
		- El switch comienza a retransmitir la trama tan pronto como recibe la dirección de destino, sin esperar a que llegue la trama completa.
		- No realiza Frame-Check-Sequence.
		- **Ventaja**: Latencia baja, ya que el switch comienza a reenviar la trama de inmediato.
	- **Store & Forward**:
		- Espera toda la trama.
		- Realiza chequeo de la trama.
		- **Ventaja**: Mejora la fiabilidad, ya que solo se transmiten tramas válidas.
	- **Fragment-free**:
		- Lee los primeros 64 bytes.
		- **Los primeros 64 bytes incluyen**:
			- Preambulo y SFD (8 bytes).
			- Direcciones MAC origen y destino (12 bytes).
			- Tipo/Longitud (2 bytes).
			- **Parte de los datos de la carga útil (mínimo 42 bytes si la trama tiene el tamaño mínimo de 64 bytes).**
- Comparativa con *Router*:
	- Ambos Store-and-forward.
	- Tablas:
		- Router -> Tablas de ruteo.
		- Switch -> Tablas de switch.

| Capa de Red | Capa de Enlace      |     |
| ----------- | ------------------- | --- |
| Router      | Hub, Switch, Bridge |     |

---
### Protocolo Spaning-Tree (STP)
- Protocolo de gestión de capa de enlace que pone a disposición la *redundancia* de caminos pero previene de posibles *loops* en la red de *switches*.
- Permite aprovechar los numerosos caminos de una red redundante, evitando loops.
- Convierte un grafo en un arbol.
- Múltiples caminos asegura disponibilidad en caso de fallos.
- Existen loops *físicos* pero no *lógicos*.

---
### VLAN
- Virtual Lan
- Permite crear "Switches virtuales" en uno o mas switches y de esa forma separar dominios de broadcast.
- Permite segmentar redes de forma lógica dentro de la misma infraestructura física, mejorando **rendimiento, seguridad y gestión** de la red.
- **Analogía:**
	- Imagina un **hotel grande** con múltiples **salas de conferencias** donde diferentes empresas realizan reuniones privadas.
		- **La red física del hotel** es como el edificio, con pasillos y puertas conectando todas las salas.
		- **Cada empresa que usa una sala** representa una VLAN diferente. Aunque todas las empresas están dentro del mismo hotel (misma red física), sus reuniones están separadas y solo pueden comunicarse entre sí dentro de su propia sala.
		- **Los empleados de una empresa dentro de su sala** pueden hablar entre ellos sin interferencias de otras empresas (dispositivos dentro de una VLAN pueden comunicarse entre sí).
		- **Si alguien de una empresa necesita hablar con otra empresa en otra sala**, debe pasar por la recepción del hotel (equivalente a un router o un switch de Capa 3) para gestionar la comunicación.
- **Ventajas**: mejoran la seguridad, el rendimiento y la administración de la red.
- **Desventajas**: Pero requieren hardware adecuado y una configuración cuidadosa para evitar problemas.

---
### Protocolo Punto a Punto
- Un solo enlace, mucho mas simple que comunicaciones broadcast.
- PPP:
	- Simple.
	- Multiplexación:
		- Porta datos de capa de red.
	- Detección de errores, no corrección. 

---

### Dominio de colisión
- Hasta donde pueden extenderse las colisiones.
- Repetidores/hubs extienden un dominio de colisión.

### Hubs
- Tipos:
	- Pasivos: **Solo reenvian** la señal por todos los puertos restantes.
	- Activos: **Regeneran** la señal, mayor alcance.
	- Inteligentes: Pueden **detectar problemas**.

### Bridge
- Unifica dos protocolos de nivel de enlace.
- Divide dominio de colisión.
- Implementado por software.

### Switch
- Bridge multipuerto que trabaja con la misma tecnología de enlace y física en c/u.
- Separa dominios de colisión.
- Funciones del switch:
	- **Aprender direcciones MAC**.
		- Mapea MAC con interfaces en su tabla de switch.
	- **Reenviar / filtrar paquetes**.
	- **Evitar bucles en capa 2**:
		- Administran los bucles de redundancia con *STP*.

---
# ARP

### Características
- Trabaja conjuntamente con Ethernet.
- Trabaja de forma dinámica, auto aprendizaje.
- Es de capa de enlace, a veces considerado de capa de red.
	- Es "helper" de IP.
- Mapea Direcciones IP a Direcciones MAC.

### Paquete ARP
- Se encapsula dentro de una trama Ethernet.
- Estructura (lo importante para nosotros):
	- Sender Hardware Address
		- MAC Origen
	- Sender Protocol Address
		- IP Origen
	- Receiver Hardware Address
		- MAC Destino
	- Receiver Protocol Address
		- IP Destino
### Aprendizaje de direcciones
- Un **HOST-A** quiere enviar un mensaje a **HOST-D**, pero **no tiene su direccion IP mappeada a una MAC**.
	- Al armar la trama ethernet a enviar, la dirección MAC (en la trama ethernet, no la IP contenida) destino es desconocida.
- **HOST-A** debe realizar un **ARP Request** para conseguir la dirección **MAC** asociada a la **IP** de **HOST-D**.
	- Una trama ethernet viaja con un paquete ARP contenido dentro.
		- La dirección MAC destino es broadcast (ff:ff:ff:ff:ff:ff)
			- Ethernet requiere una **MAC** válida, no se puede poner 0.
		- El paquete **ARP** se conforma de:
			- **IP** Origen y **MAC** Origen
			- **IP** Destino/Buscada y **MAC** Destino/Buscada  00:00:00:00:00:00 (se desconoce).
	- El mensaje es enviado en broadcast.
- Todos los host que reciban la petición y su **IP** no corresponda a la destino del paquete ARP, descartarán la trama.
- **HOST-D** recibe el paquete y responde con **ARP Reply**.
	- Se tienen todos los valores y se envia en **Unicast** (directo al destino).
	- La trama ethernet viaja con un paquete ARP contenido dentro.
		- La dirección MAC destino es MAC-HOST-A.
		- El paquete **ARP** se conforma de:
			- **IP** Origen (HOST-D) y **MAC** Origen (HOST-D).
			- **IP** Destino (HOST-A) y **MAC** Destino (HOST-A)

---
| Concepto                                   | Palabras Clave                                                                                    | Definición                                                                                                                                                                         | Diferencias Clave                                                                                                                                             | Consideraciones Adicionales                                                                                                                                                                                                 |
| ------------------------------------------ | ------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Uplink (Conecta)**                       | - Conectividad ascendente<br>- Puerto uplink<br>- Enlace jerárquico<br>- Conexión entre capas     | Conexión que permite la comunicación entre dispositivos de red de diferentes niveles jerárquicos, típicamente desde un dispositivo de nivel inferior hacia uno de nivel superior.  | - Enfocado solo en la conectividad física<br>- No maneja segmentación lógica<br>- Conexión punto a punto entre dispositivos                                   | - Puede ser un puerto especial en switches<br>- Generalmente tiene mayor ancho de banda<br>- Se configura como puerto de acceso (modo access) o troncal (modo trunk)<br>- Crítico para la topología jerárquica de red       |
| **Uplink + Trunking (Conecta + Segmenta)** | - Troncal<br>- 802.1Q<br>- VLANs<br>- Etiquetado<br>- Multiplexación                              | Combinación de conectividad física (uplink) con capacidad para transportar tráfico de múltiples VLANs sobre un mismo enlace físico (trunking), manteniendo la segmentación lógica. | - Mantiene la segmentación lógica<br>- Transporta múltiples VLANs<br>- Utiliza etiquetado de tramas<br>- Mayor complejidad de configuración                   | - Requiere protocolos de etiquetado (802.1Q, ISL)<br>- Puede configurarse con VLAN nativa (sin etiquetar)<br>- Permite escalabilidad sin aumentar conexiones físicas<br>- Optimiza el uso de puertos físicos                |
| **Router on a Stick (Comunica)**           | - InterVLAN routing<br>- Subinterfaces<br>- Gateway<br>- Encapsulamiento 802.1Q<br>- Enrutamiento | Configuración donde un único puerto físico de router se conecta a un switch y se configura con múltiples subinterfaces virtuales para enrutar tráfico entre diferentes VLANs.      | - Enfocado en comunicación entre VLANs<br>- Usa un solo puerto físico del router<br>- Depende de subinterfaces virtuales<br>- Función principal: enrutamiento | - Potencial cuello de botella en redes grandes<br>- Económico en hardware<br>- Limitado por el ancho de banda del enlace físico<br>- Alternativa: switches capa 3<br>- Requiere configuración de subinterfaces en el router |


---
# Preguntas GPT

### Preguntas Verdadero/Falso

1. **La capa de enlace garantiza la entrega fiable de datos entre dos nodos cualesquiera de una red.** (V)
2. **El protocolo CRC es un método para la detección de errores basado en la suma de bits, similar al Internet Checksum.** (F)
3. **En un enlace semidúplex, los nodos pueden transmitir y recibir datos al mismo tiempo.** (F)
4. **Los switches operan en la capa de red y utilizan direcciones IP para reenviar paquetes.** (F)
5. **Los hubs son dispositivos inteligentes que pueden almacenar y reenviar datos según la dirección MAC de destino.** (F)
6. **El protocolo STP ayuda a evitar loops en una red de switches redundante.** (V)
7. **En un esquema de acceso aleatorio, los nodos transmiten siempre a la máxima velocidad y esperan un tiempo fijo en caso de colisión.** (F)
8. **El protocolo Ethernet es un protocolo confiable orientado a conexión.** (F)
9. **El método ‘Cut Through’ en switches tiene menor latencia que ‘Store & Forward’, pero no realiza verificación de errores.** (V)
10. **Las VLAN permiten segmentar una red de forma lógica, pero no mejoran la seguridad.** (F)

---

### Preguntas de Opción Múltiple

#### 11. ¿Cuál es la función principal de la capa de enlace? (a o b)

a) Transportar datagramas entre nodos a través de múltiples enlaces.  
b) Gestionar la comunicación entre dispositivos físicos en la misma red.  
c) Asegurar la entrega de datos a nivel de aplicación.  
d) Asignar direcciones IP a dispositivos en la red.

#### 12. ¿Qué protocolo de acceso múltiple divide el canal en franjas de tiempo fijas para cada nodo? (b)

a) CSMA/CD  
b) TDM  
c) FDM  
d) STP

#### 13. ¿Cuál de las siguientes técnicas de detección de errores implica la división de datos por un polinomio generador? (c)

a) Paridad bidimensional  
b) Internet Checksum  
c) CRC  
d) STP

#### 14. ¿Cuál de las siguientes afirmaciones sobre switches es correcta? (b)

a) Operan en la capa de red y utilizan direcciones IP.  
b) Son dispositivos transparentes que pueden aprender direcciones MAC.  
c) Reenvían tramas a todos los puertos de salida como un hub.  
d) No almacenan información sobre dispositivos conectados.

#### 15. ¿Qué hace un switch cuando recibe una trama con una dirección MAC de destino desconocida? (b)

a) Descarta la trama.  
b) La envía a todos los puertos excepto al de origen.  
c) La reenvía solo al puerto de origen.  
d) La almacena indefinidamente hasta que el dispositivo destino se comunique.

#### 16. ¿Cuál de las siguientes técnicas de conmutación introduce la menor latencia? (b)

a) Store & Forward  
b) Cut Through  
c) Fragment-Free  
d) Todas tienen la misma latencia

#### 17. ¿Cuál es la principal ventaja de las VLAN? (b)

a) Permiten dividir físicamente una red sin cambiar la infraestructura.  
b) Mejoran la seguridad y reducen el tráfico de broadcast.  
c) Incrementan la cantidad de nodos que pueden conectarse a un switch.  
d) Reducen la necesidad de direcciones MAC en la red.

#### 18. ¿Qué característica define a una red Ethernet? (b)

a) Es orientada a conexión y confiable.  
b) No es orientada a conexión ni confiable.  
c) Solo permite conexiones semidúplex.  
d) Funciona únicamente en redes inalámbricas.

#### 19. ¿Qué protocolo evita loops en redes con switches redundantes? (c)

a) CSMA/CD  
b) VLAN  
c) STP  
d) CRC

#### 20. ¿Cuál de las siguientes afirmaciones sobre hubs es correcta? (c)

a) Son dispositivos que reenvían tramas solo a los nodos destino.  
b) Funcionan en la capa de red y utilizan direcciones IP.  
c) Repiten las señales a todos los puertos, menos el origen, sin filtrar.  
d) Son más eficientes que los switches en redes modernas.

---

### Respuestas

1. **F**
    
2. **F**
    
3. **F**
    
4. **F**
    
5. **F**
    
6. **V**
    
7. **F**
    
8. **F**
    
9. **V**
    
10. **F**
    
11. **b**
    
12. **b**
    
13. **c**
    
14. **b**
    
15. **b**
    
16. **b**
    
17. **b**
    
18. **b**
    
19. **c**
    
20. **c**

---

---

---
# Preguntas Grok

### **Servicios de la Capa de Enlace**

**Pregunta 1: ¿Qué servicio de la capa de enlace se encarga de limitar la cantidad de datos enviados para evitar saturar al receptor?**  
a) Entramado  
b) Control de flujo  
c) Detección de errores  
d) Acceso al enlace

--- Respuesta más abajo ---

**Respuesta**: b) Control de flujo  
_Explicación_: El control de flujo regula la velocidad de transmisión para que el receptor no se vea abrumado por los datos enviados.

---

**Pregunta 2: Verdadero o Falso: La capa de enlace siempre proporciona corrección de errores automática.**  
Verdadero/FalsoVerdadero / FalsoVerdadero/Falso

--- Respuesta más abajo ---

**Respuesta**: Falso  
_Explicación_: La capa de enlace detecta errores (como con CRC), pero la corrección suele depender de capas superiores o no se realiza.

---

**Pregunta 3: ¿Qué significa 'entramado' en la capa de enlace?**  
a) Dividir los datos en paquetes más pequeños  
b) Encapsular datos en tramas con encabezados y colas  
c) Corregir errores en los bits recibidos  
d) Asignar direcciones IP a los nodos

--- Respuesta más abajo ---

**Respuesta**: b) Encapsular datos en tramas con encabezados y colas  
_Explicación_: El entramado organiza los datos en tramas, añadiendo información como direcciones y bits de control.

---

### **Implementación en NIC y Comunicación entre Adaptadores**

**Pregunta 4: ¿Qué hace el adaptador de red (NIC) cuando recibe una trama?**  
a) La enruta a otra red  
b) Verifica errores y pasa los datos a la capa superior si es válida  
c) La retransmite a todos los nodos conectados  
d) La convierte en paquetes IP

--- Respuesta más abajo ---

**Respuesta**: b) Verifica errores y pasa los datos a la capa superior si es válida  
_Explicación_: El NIC receptor revisa la trama (por ejemplo, con CRC) y, si no hay errores, entrega los datos a la capa de red.

---

**Pregunta 5: Verdadero o Falso: Los adaptadores de red se comunican directamente a través de direcciones IP.**  
Verdadero/FalsoVerdadero / FalsoVerdadero/Falso

--- Respuesta más abajo ---

**Respuesta**: Falso  
_Explicación_: Los adaptadores usan direcciones MAC en la capa de enlace; las IP son para la capa de red.

---

### **Detección de Errores**

**Pregunta 6: ¿Qué método de detección de errores suma todos los bytes de datos y usa el complemento a uno?**  
a) Paridad simple  
b) Checksum  
c) CRC  
d) Paridad bidimensional

--- Respuesta más abajo ---

**Respuesta**: b) Checksum  
_Explicación_: El checksum suma los datos y usa el complemento para verificar errores, común en protocolos como IP.

---

**Pregunta 7: ¿Cuál es una limitación del chequeo de paridad simple?**  
a) No detecta errores en ráfagas  
b) Requiere hardware complejo  
c) No funciona en tramas largas  
d) Solo detecta errores en encabezados

--- Respuesta más abajo ---

**Respuesta**: a) No detecta errores en ráfagas  
_Explicación_: La paridad simple solo detecta errores en un bit aislado, no en múltiples bits cercanos.

---

**Pregunta 8: Verdadero o Falso: El CRC puede detectar todos los errores de un solo bit en una trama.**  
Verdadero/FalsoVerdadero / FalsoVerdadero/Falso

--- Respuesta más abajo ---

**Respuesta**: Verdadero  
_Explicación_: El CRC está diseñado para detectar todos los errores de un solo bit y muchos errores múltiples, según el polinomio usado.

---

### **Protocolos de Acceso Múltiple**

**Pregunta 9: ¿Qué protocolo de acceso múltiple asigna frecuencias específicas a cada nodo?**  
a) FDM  
b) CSMA/CA  
c) Token Bus  
d) Slotted ALOHA

--- Respuesta más abajo ---

**Respuesta**: a) FDM  
_Explicación_: FDM (Frequency Division Multiplexing) divide el espectro en canales de frecuencia para cada nodo.

---

**Pregunta 10: En ALOHA puro, ¿qué sucede si dos nodos transmiten al mismo tiempo?**  
a) Ambos paquetes se entregan correctamente  
b) Se produce una colisión y los nodos retransmiten tras un tiempo aleatorio  
c) El nodo más rápido gana el acceso  
d) Se detiene la transmisión de ambos nodos permanentemente

--- Respuesta más abajo ---

**Respuesta**: b) Se produce una colisión y los nodos retransmiten tras un tiempo aleatorio  
_Explicación_: En ALOHA, las colisiones son comunes y los nodos intentan de nuevo tras un retraso aleatorio.

---

**Pregunta 11: Verdadero o Falso: Los protocolos de toma de turnos garantizan un uso equitativo del medio.**  
Verdadero/FalsoVerdadero / FalsoVerdadero/Falso

--- Respuesta más abajo ---

**Respuesta**: Verdadero  
_Explicación_: En protocolos como Token Ring, cada nodo tiene su turno, asegurando acceso justo.

---

### **Redes Locales Conmutadas y Dispositivos**

**Pregunta 12: ¿Qué dispositivo de red opera únicamente en la capa física?**  
a) Switch  
b) Bridge  
c) Hub  
d) Router

--- Respuesta más abajo ---

**Respuesta**: c) Hub  
_Explicación_: Un hub retransmite señales a todos los puertos sin procesar las tramas (capa 1).

---

**Pregunta 13: ¿Qué ventaja tiene un switch sobre un hub?**  
a) Mayor velocidad física  
b) Reducción de colisiones al crear dominios de colisión individuales  
c) Capacidad de enrutar paquetes entre redes  
d) Menor costo de implementación

--- Respuesta más abajo ---

**Respuesta**: b) Reducción de colisiones al crear dominios de colisión individuales  
_Explicación_: Un switch envía tramas solo al puerto correcto, aislando los dominios de colisión.

---

**Pregunta 14: Verdadero o Falso: Un switch almacena direcciones MAC en una tabla para reenviar tramas.**  
Verdadero/FalsoVerdadero / FalsoVerdadero/Falso

--- Respuesta más abajo ---

**Respuesta**: Verdadero  
_Explicación_: Los switches aprenden las direcciones MAC de los dispositivos conectados para reenviar tramas eficientemente.

---

### **Ethernet**

**Pregunta 15: ¿Qué longitud mínima tiene una trama Ethernet para garantizar la detección de colisiones en CSMA/CD?**  
a) 32 bytes  
b) 64 bytes  
c) 128 bytes  
d) 256 bytes

--- Respuesta más abajo ---

**Respuesta**: b) 64 bytes  
_Explicación_: Una trama de 64 bytes asegura que el emisor detecte colisiones antes de terminar su transmisión.

---

**Pregunta 16: ¿Qué campo de la trama Ethernet se usa para verificar la integridad de los datos?**  
a) Preambulo  
b) Dirección de destino  
c) CRC (FCS)  
d) Type

--- Respuesta más abajo ---

**Respuesta**: c) CRC (FCS)  
_Explicación_: El Frame Check Sequence (CRC) verifica si hay errores en la trama recibida.

---

**Pregunta 17: Verdadero o Falso: Ethernet soporta transmisión fullduplex en conexiones modernas.**  
Verdadero/FalsoVerdadero / FalsoVerdadero/Falso

--- Respuesta más abajo ---

**Respuesta**: Verdadero  
_Explicación_: Con switches y cables adecuados, Ethernet permite envío y recepción simultáneos.

---

### **Protocolos Avanzados (STP, VLAN, PPP)**

**Pregunta 18: ¿Qué hace STP cuando detecta un enlace redundante?**  
a) Lo elimina físicamente  
b) Lo desactiva lógicamente para evitar bucles  
c) Lo usa para duplicar el tráfico  
d) Lo convierte en una VLAN

--- Respuesta más abajo ---

**Respuesta**: b) Lo desactiva lógicamente para evitar bucles  
_Explicación_: STP bloquea puertos redundantes, reactivándolos solo si falla otro enlace.

---

**Pregunta 19: ¿Qué protocolo avanzado permite autenticación en conexiones punto a punto?**  
a) STP  
b) VLAN  
c) PPP  
d) ARP

--- Respuesta más abajo ---

**Respuesta**: c) PPP  
_Explicación_: PPP incluye opciones como PAP o CHAP para autenticar nodos en enlaces seriales.

---

**Pregunta 20: Verdadero o Falso: Las VLANs requieren switches compatibles para funcionar.**  
Verdadero/FalsoVerdadero / FalsoVerdadero/Falso

--- Respuesta más abajo ---

**Respuesta**: Verdadero  
_Explicación_: Los switches deben soportar VLAN tagging (como IEEE 802.1Q) para segmentar la red lógicamente.

---

### **Dominio de Colisión**

**Pregunta 21: ¿Qué dispositivo separa dominios de colisión?**  
a) Hub  
b) Switch  
c) Repetidor  
d) Cable cruzado

--- Respuesta más abajo ---

**Respuesta**: b) Switch  
_Explicación_: Un switch crea un dominio de colisión por puerto, a diferencia de hubs o repetidores.

---

**Pregunta 22: Verdadero o Falso: En una red fullduplex, no hay dominios de colisión.**  
Verdadero/FalsoVerdadero / FalsoVerdadero/Falso

--- Respuesta más abajo ---

**Respuesta**: Verdadero  
_Explicación_: En fullduplex, los nodos no compiten por el medio, eliminando colisiones.

---

### **Protocolo ARP**

**Pregunta 23: ¿Qué hace un host cuando recibe un ARP Reply?**  
a) Actualiza su tabla ARP con la dirección MAC recibida  
b) Envía un nuevo ARP Request  
c) Descarta la respuesta si no la esperaba  
d) Inicia una conexión TCP

--- Respuesta más abajo ---

**Respuesta**: a) Actualiza su tabla ARP con la dirección MAC recibida  
_Explicación_: El ARP Reply proporciona la MAC solicitada, que se guarda para futuras comunicaciones.

---

**Pregunta 24: ¿Qué tipo de mensaje ARP se envía como broadcast?**  
a) ARP Reply  
b) ARP Request  
c) ARP Acknowledgment  
d) ARP Probe

--- Respuesta más abajo ---

**Respuesta**: b) ARP Request  
_Explicación_: El ARP Request se envía a todos los nodos para encontrar la MAC correspondiente a una IP.

---

**Pregunta 25: Verdadero o Falso: ARP solo se usa dentro de una misma subred.**  
Verdadero/FalsoVerdadero / FalsoVerdadero/Falso

--- Respuesta más abajo ---

**Respuesta**: Verdadero  
_Explicación_: ARP resuelve direcciones MAC dentro de una red local; entre subredes, se usan routers.