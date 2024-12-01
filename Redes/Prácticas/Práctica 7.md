Aquí tienes las respuestas a las preguntas relacionadas con redes:

# 1
 **¿Qué servicios presta la capa de red? ¿Cuál es la PDU en esta capa? ¿Qué dispositivo es considerado solo de la capa de red?**
    
- **Servicios:** La capa de red es responsable del enrutamiento, direccionamiento lógico, y fragmentación de paquetes para el transporte de datos entre redes diferentes.
- **PDU (Unidad de Datos de Protocolo):** En esta capa, la PDU es el **paquete**.
- **Dispositivo de la capa de red:** El dispositivo que opera exclusivamente en la capa de red es el **router**.
- 

---
# 2 
**¿Por qué se lo considera un protocolo de mejor esfuerzo?**

- Protocolos como el IP se consideran de "mejor esfuerzo" porque no garantizan la entrega de datos, el orden de los mismos ni la ausencia de duplicados. Simplemente intentan transmitir los datos lo mejor posible sin asegurar confiabilidad.

---
# 3
 **¿Cuántas redes clase A, B y C hay? ¿Cuántos hosts como máximo pueden tener cada una?**

- **Clase A:**
	- Redes posibles: 2⁷ - 2 = 126 (excluyendo las direcciones reservadas 0.0.0.0 y 127.0.0.0).
	- Hosts por red: 2²⁴ - 2 = 16,777,214.
- **Clase B:**
	- Redes posibles: 2¹⁴ = 16,384.
	- Hosts por red: 2¹⁶ - 2 = 65,534.
- **Clase C:**
	- Redes posibles: 2²¹ = 2,097,152.
	- Hosts por red: 2⁸ - 2 = 254.

---
# 4
**¿Qué son las subredes? ¿Por qué es importante siempre especificar la máscara de subred asociada?**
    
- **Subredes:** Son divisiones lógicas de una red IP mayor en redes más pequeñas. Esto permite optimizar el uso de direcciones IP y mejorar la administración de la red.
- **Importancia de la máscara:** La máscara de subred define los límites de cada subred al especificar qué parte de la dirección IP corresponde a la red y cuál a los hosts. Sin ella, no se puede interpretar correctamente la estructura de la red.


---
# 5
**¿Cuál es la finalidad del campo Protocol en la cabecera IP? ¿A qué campos de la capa de transporte se asemeja en su funcionalidad?**
    
- **Finalidad del campo Protocol:** Identifica el protocolo de la capa superior (como TCP o UDP) al cual se debe entregar el paquete IP.
- **Semejanza en la capa de transporte:** Se asemeja al campo **port** (puerto), ya que ambos son responsables de identificar al destinatario dentro de la pila de protocolos.
  
---
# 6
**Para cada una de las siguientes direcciones IP (172.16.58.223/26, 163.10.5.49/27, 128.10.1.0/23, 10.1.0.0/24, 8.40.11.179/12) determine: 
a. ¿De qué clase de red es la dirección dada (Clase A, B o C)? 
b. ¿Cuál es la dirección de subred? 
c. ¿Cuál es la cantidad máxima de hosts que pueden estar en esa subred? 
d. ¿Cuál es la dirección de broadcast de esa subred? 
e. ¿Cuál es el rango de direcciones IP válidas dentro de la subred?**

##### **1. Dirección IP: 172.16.58.223/26**

- **a. Clase de red:** Es clase B porque el primer octeto está entre 128 y 191.
- **b. Dirección de subred:**
    - Máscara de subred: /26 = 255.255.255.192.
    - Intervalo de subredes: Cada subred cubre 64 direcciones (256 - 192 = 64).
    - La dirección pertenece al rango **172.16.58.192 - 172.16.58.255**. La dirección de subred es **172.16.58.192**.
- **c. Máximo de hosts:** 2⁶ - 2 = 62 hosts.
- **d. Dirección de broadcast:** Es la última dirección del rango, **172.16.58.255**.
- **e. Rango de direcciones válidas:** Desde **172.16.58.193** hasta **172.16.58.254**.

##### **2. Dirección IP: 163.10.5.49/27**

- **a. Clase de red:** Clase B, ya que el primer octeto está entre 128 y 191.
- **b. Dirección de subred:**
    - Máscara de subred: /27 = 255.255.255.224.
    - Intervalo de subredes: Cada subred cubre 32 direcciones (256 - 224 = 32).
    - La dirección pertenece al rango **163.10.5.32 - 163.10.5.63**. La dirección de subred es **163.10.5.32**.
- **c. Máximo de hosts:** 2⁵ - 2 = 30 hosts.
- **d. Dirección de broadcast:** **163.10.5.63**.
- **e. Rango de direcciones válidas:** Desde **163.10.5.33** hasta **163.10.5.62**.

##### **3. Dirección IP: 128.10.1.0/23**

- **a. Clase de red:** Clase B, porque el primer octeto está entre 128 y 191.
- **b. Dirección de subred:**
    - Máscara de subred: /23 = 255.255.254.0.
    - Intervalo de subredes: Cada subred cubre 512 direcciones (256 × 2 = 512).
    - La dirección pertenece al rango **128.10.0.0 - 128.10.1.255**. La dirección de subred es **128.10.0.0**.
- **c. Máximo de hosts:** 2⁹ - 2 = 510 hosts.
- **d. Dirección de broadcast:** **128.10.1.255**.
- **e. Rango de direcciones válidas:** Desde **128.10.0.1** hasta **128.10.1.254**.

##### **4. Dirección IP: 10.1.0.0/24**

- **a. Clase de red:** Clase A, porque el primer octeto está entre 0 y 127.
- **b. Dirección de subred:**
    - Máscara de subred: /24 = 255.255.255.0.
    - Intervalo de subredes: Cada subred cubre 256 direcciones.
    - La dirección pertenece al rango **10.1.0.0 - 10.1.0.255**. La dirección de subred es **10.1.0.0**.
- **c. Máximo de hosts:** 2⁸ - 2 = 254 hosts.
- **d. Dirección de broadcast:** **10.1.0.255**.
- **e. Rango de direcciones válidas:** Desde **10.1.0.1** hasta **10.1.0.254**.

##### **5. Dirección IP: 8.40.11.179/12**

- **a. Clase de red:** Clase A, porque el primer octeto está entre 0 y 127.
- **b. Dirección de subred:**
    - Máscara de subred: /12 = 255.240.0.0.
    - Intervalo de subredes: Cada subred cubre 1,048,576 direcciones (2²⁰).
    - La dirección pertenece al rango **8.0.0.0 - 8.15.255.255**. La dirección de subred es **8.0.0.0**.
- **c. Máximo de hosts:** 2²⁰ - 2 = 1,048,574 hosts.
- **d. Dirección de broadcast:** **8.15.255.255**.
- **e. Rango de direcciones válidas:** Desde **8.0.0.1** hasta **8.15.255.254**.

---
# 7 
**Su organización cuenta con la dirección 128.50.10.0. Indique: 
a. ¿Es una dirección de red o de host? 
b. Clase a la que pertenece y máscara de clase. 
c. Cantidad de hosts posibles. d. Se necesitan crear, al menos, 513 subredes. Indique: 
	i. Máscara necesaria. 
	ii. Cantidad de redes asignables. 
	iii. Cantidad de hosts por subred. 
	iv. Dirección de la subred 710. 
	v. Dirección de broadcast de la subred 710.**

##### **A**
Es una dirección de red, dado que el ultimo octeto está en 0.

##### **B**
La red pertenece a la clase **B** y su máscara es **/16

##### **C**
La cantidad de hosts se calcula como $2^{32}-2^{máscara}-2$, entonces se pueden referenciar $2^{32-16}-2=2^{16}-2=65534$ hosts.

- i:
	- No se pueden utilizar los bits de redes predeterminados para las nuevas subredes, por lo que se suman los ***n*** bits tal que ***n*** es el menor N donde $2^n\leq{cant subredes}$.
	- Se quieren agregar 513 redes, se necesitan **10 bits** mas.
	- **La nueva máscara sería /16+10, /26**
- ii:
	- Cantidad de subredes: $2^{10}=1024$
- iii:
	- Cantidad de hosts por subred $2^{32-26}-2=62$
- iv:
	- Para encontrar la dirección de una subred específica, seguimos estos pasos:
		- Hay una subred cada 64 hosts, por lo que multiplicamos 710 * 64 para llegar a la dirección de la subred 710.
		- 710 * 64 = 45440
		- Valores:
			- 45440 / 255 = Valor del tercer octeto
			- 45440 % 255 = Valor del cuarto octeto
		- **Resultado** = 128.50.187.64 
- v: 
	- La dirección broadcast es la última de la subred:
		- 128.50.187.64 + 63 = 128.50.187.127

---
# 8
**Si usted estuviese a cargo de la administración del bloque IP 195.200.45.0/24 
a. ¿Qué máscara utilizaría si necesita definir al menos 9 subredes?
b. Indique la dirección de subred de las primeras 9 subredes. 
c. Seleccione una e indique dirección de broadcast y rango de direcciones asignables en esa subred.**

- a
	- $2^4=16\geq9$, con 4 bits mas me alcanzan.
	- La máscara seria de /24 + 4 = **/28**.
- b
	- 195.200.45.(0000)0000 es la red original, donde (0000) son los bits para subredes.
	- Las direcciones de red de las subredes son:
		- 195.200.45.(0000)0000
			- 195.200.45.0
		- 195.200.45.(0001)0000
			- 195.200.45.16
		- 195.200.45.(0010)0000
			- 195.200.45.32
		- 195.200.45.(0011)0000
			- 195.200.45.48
		- 195.200.45.(0100)0000
			- 195.200.45.64
		- 195.200.45.(0101)0000
			- 195.200.45.80
		- 195.200.45.(0110)0000
			- 195.200.45.96
		- 195.200.45.(0111)0000
			- 195.200.45.112
		- 195.200.45.(1000)0000
			- 195.200.45.128
- c
	- Para la red 195.200.45.16
	- broadcast:
		- 195.200.45.16 + 15 = 195.200.45.31
	- rango de direcciones asignables:
		  195.200.45.17 a 195.200.45.30

---
# 9
*Dan un gráfico*
![[Pasted image 20241130170602.png]]
**a. Verifique si es correcta la asignación de direcciones IP y, en caso de no serlo, modifique la misma para que lo sea. 
b. ¿Cuántos bits se tomaron para hacer subredes en la red 10.0.10.0/24? ¿Cuántas subredes se podrían generar? 
c. Para cada una de las redes utilizadas indique si son públicas o privadas.**

- a
	- En el switch A se asigna la dirección broadcast .3, no se puede.
	- En el router C se asigna el host 17 y al B con la 14, donde el problema es el siguiente:
		- La red 172.17.10.0/28 tiene $2^4-2$ hosts por cada red, por lo que los rangos van de a 14 hosts.
		- Entonces la IP 172.17.10.17 y la172.17.10.14 no estan en la misma red.
- b
	- Se tomaron 24 bits para redes, dejando lugar para $2^8-2$ hosts (254).
	- Como hay dos hosts se podrian agregar 6 bits mas para subredes, dejando la mascara en /30, con lugar para 2 hosts.
- c
	- 192.168 es privada.
	- 10.0.10 es privada
	- 191.26.145.0 es pública
	- 172.26.22.0 es pública
	- 172.17.10.0 es pública

---
# 10
**¿Qué es CIDR (Class Interdomain routing)? ¿Por qué resulta útil?**

**CIDR** (Classless Inter-Domain Routing) es un método para asignar y enrutar direcciones IP, desarrollado como reemplazo del sistema de clases tradicional (A, B, C). En lugar de depender de clases fijas, CIDR utiliza **máscaras de subred flexibles** para dividir o agregar direcciones IP de manera eficiente. 

Tambien conocido como supernetting, junta varias subredes en una red mas grande.

---
# 11
**¿Cómo publicaría un router las siguientes redes si se aplica CIDR? 
a. 198.10.1.0/24 
b. 198.10.0.0/24 
c. 198.10.3.0/24 
d. 198.10.2.0/24**

Estas redes se pueden **agregar (supernetting)**, combinándolas en un único bloque CIDR si comparten un patrón común en sus bits iniciales.

En los bits binarios, el patrón común se extiende hasta los primeros **22 bits**:
- (11000110.00001010.000000)00

---
# 14
**¿Qué es y para qué se usa VLSM?**

Es dividir la red en subredes segun sea necesario para las distintas partes de la red que asi lo requieran.

---
### **15. Mecanismo para dividir subredes utilizando VLSM**

El proceso para dividir subredes con VLSM implica seguir estos pasos:

**Paso 1: Identificar la red base**

Empieza con el bloque de direcciones asignado, especificando su tamaño inicial (máscara de red). En este caso, por ejemplo, 205.10.192.0/19205.10.192.0/19205.10.192.0/19.

**Paso 2: Determinar los requisitos de cada subred**

Para cada subred, define cuántas direcciones son necesarias. Considera que siempre debes reservar 2 direcciones por subred: una para el **broadcast** y otra para la **dirección de red**.

**Paso 3: Asignar la máscara adecuada a cada subred**

Para cada subred, calcula el número mínimo de bits necesarios para cubrir el tamaño. Usa la fórmula:

Nuˊmero de direcciones necesarias=2(32−nueva maˊscara)\text{Número de direcciones necesarias} = 2^{(32 - \text{nueva máscara})}Nuˊmero de direcciones necesarias=2(32−nueva maˊscara)

Elige una máscara adecuada, redondeando siempre hacia arriba al próximo bloque de potencia de 2.

**Paso 4: Asignar direcciones consecutivas**

Asigna direcciones de forma consecutiva. Comienza con la dirección más baja disponible y avanza en incrementos según el tamaño de la subred.

**Paso 5: Verificar solapamiento**

Asegúrate de que las subredes no se solapen. Esto se logra ajustando correctamente las máscaras y empezando cada nueva subred después del bloque de direcciones de la anterior.