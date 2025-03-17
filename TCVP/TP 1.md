# Ejercicio 1

### 1
- $\Sigma^* \cap \ {L}= {L}$  
	- L está contenido en Sigma estrella, por lo tanto, la intersección es igual a L.
- $\Sigma^* \cup \ {L}= \Sigma^*$  
	- Sigma estrella contiene a L, por lo tanto, la unión es igual a Sigma estrella.
- $L^C = \Sigma^* - L$ 
	- El lenguaje complemento de L son todas las cadenas de Sigma estrella que no cumplen con las condiciones de L (${a^n\ b^n\ c^n | n ≥ 0}$), por ejemplo, "a", "abcc", etc.

### 2. Definir el problema de la satisfactibilidad de las fórmulas booleanas en la forma de problema de búsqueda (visión de MT calculadora), de decisión (visión de MT reconocedora), y de enumeración (visión de MT generadora).

- **Búsqueda**:
	- Encuentra **alguna** solución que haga satisfactible a la fórmula y la deja en cinta.
	- En caso de no existir una solución, se determina insatisfactible.
- **Decisión**:
	- Determina si la fórmula es satisfactible o no (termina en $q_A$ o $q_R$)
	- No se deja en cinta la solución.
- **Enumeración (generadora)**:
	- Lista todas las fórmulas satisfactibles, es decir, todos los elementos del lenguaje que acepta.

### 3. ¿Qué postula la Tesis de Church-Turing?
- Todo dispositivo computacional físicamente realizable puede ser simulado por una MT.
- Se denota entonces que la computadora moderna no tiene mayor capacidad de cómputo que una MT convencional (despreciando la eficiencia).

### 4. ¿Cuándo dos MT son equivalentes? ¿Cuándo dos modelos de MT son equivalentes?

- Dos MT son equivalentes cuando su lenguaje es el mismo.
- Todos los modelos de MT son equivalentes ya que todos tienen la misma capacidad de cómputo.

### 5. ¿En qué se diferencian los lenguajes recursivos, los lenguajes recursivamente enumerables no recursivos, y los lenguajes no recursivamente enumerables?

| Recursivo (R)                                                         | Recursivamente enumerables no recursivos (RE - R)                            | no recursivamente enumerables (L - RE)                                                  |     |
| --------------------------------------------------------------------- | ---------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- | --- |
| Siempre se puede decidir si una cadena pertenece o no al lenguaje.    | Si una cadena pertenece al lenguaje, es aceptada.                            | No se puede garantizar que si la cadena pertenece al lenguaje sea aceptada.             |     |
| Existe al menos una MT que siempre se detiene y reconoce el lenguaje. | Existe al menos una MT que reconoce el lenguaje, pero no siempre se detiene. | No existe una MT que acepte todas las cadenas del lenguaje, es decir, que lo reconozca. |     |

### 6. Probar $R \subseteq$ RE $\subseteq$ 𝔏.
*Ayuda: usar las definiciones*

R y RE están necesariamente contenidos en 𝔏, dado que R y RE son conjuntos de lenguajes y 𝔏 es el conjunto de todos los lenguajes.

$R \subseteq$ RE ya que, RE es el conjunto de lenguajes que cuentan con al menos una máquina que acepta todas sus cadenas, mientras que R es el conjunto de lenguajes que cuentan con al menos una máquina que acepta todas sus cadenas y además siempre se detiene. Vemos entonces, que un lenguaje que cumple las condiciones de R, cumple necesariamente las de RE también. 

### 7. Explicar por qué (a) el lenguaje Ʃ* de todas las cadenas, (b) el lenguaje vacío ∅, y (c) cualquier lenguaje finito, son recursivos. Alcanza con dar la idea general. 

Ayuda para (c): por cada cadena del lenguaje podría definirse un conjunto específico de transiciones.

Para cada caso, con crear una MT que reconozca el lenguaje y siempre se detenga es suficiente para demostrar que están contenidos en R.

(a) Simplemente con crear una MT que reconozca el lenguaje y se detenga siempre lo puedo demostrar. Es posible crear una MT *M* que cuente con una única transición, aceptar la cadena de entrada directamente, por lo tanto $L(M) = Ʃ*$.

(b) Se utiliza el mismo razonamiento que con el inciso anterior. Dada una MT *M* cuya única transición es rechazar la cadena de entrada, entonces $L(M) = ∅$

(c) Para este caso, se puede crear una MT que compare la cadena de entrada con todas las cadenas del lenguaje y acepte si encuentra una coincidencia o rechace en caso contrario, asegurando que siempre se detendrá. 

### 8. Explicar por qué no es correcta la siguiente prueba:
si L ∈ RE, también $L^C$ ∈ RE: 
dada una MT M que acepta L, entonces la MT M’, igual que M pero con los estados finales permutados, acepta LC.

Esta prueba no es correcta ya que, dada una MT *M* con $L(M) \in RE$, podemos decir que M no se detiene para todos los casos de rechazo (puede rechazar "loopeando"). Entonces, dada la MT M' que permuta los estados finales de M, sabemos que:
M' rechaza con $q_R$ a todas las cadenas de $L(M)$, pero no reconoceria el complemento de $L(M)$ ya que no se detiene para todas las cadenas que pertenecen al lenguaje (las que M rechaza "loopeando"). 

# Ejercicio 2
La MT que en un paso no puede simultáneamiente modificar un símbolo y moverse, puede simular una MT que sí lo puede hacer simplemente utilizando 2 estados, uno que modifica el simbolo en cinta y otro que se encarga del movimiento. Esta MT va alternando entre estos estados, separando en 2 pasos la operación.

# Ejercicio 3
Para poder aceptar el lenguaje $L = a^nb^nc^n | n ≥ 0$, una MT de 3 cintas realiza los siguientes pasos:
1. Lee las "A", avanzando en la cinta 1 hacia la derecha y copiando cada "A" que encuentra en la cinta 2.
2. Lee las "B", avanzando en la cinta 1 hacia la derecha y copiando cada "B" que encuentra en la cinta 3. Mientras se recorren las "B", se pueden recorrer las "A" de la cinta 2 para poder detectar un error en la cadena sin recorrerla enteramente, haciendo más eficiente el rechazo.
3. Recorre las "C" en la cinta 1 y las "A" previamente copiadas en la cinta 2 hasta encontrar blanco en ambas, si la cadena es correcta, o cualquier otra combinación si la cadena es incorrecta.

# Ejercicio 4
Probar: 
	1. La clase R es cerrada con respecto a la operación de unión. *Ayuda: la prueba es similar a la desarrollada para la intersección.* 
	2. La clase RE es cerrada con respecto a la operación de intersección. *Ayuda: la prueba es similar a la desarrollada para la clase R.*

1. Dados dos lenguajes L1 y L2 pertenecientes a R, se puede afirmar que el L3 = l1 U l2 tambien pertenecerá a este conjunto. Para que este lenguaje L3 esté en R hay que demostrar que es reconocible y decidible, y se puede hacer de forma sencilla. Primero se corre la MT que reconoce L1 sobre la entrada, si esta la acepta simplemente se acepta la entrada (lo reconoció y se detuvo), luego si la primer MT (con lenguaje L1) rechazó la entrada, entonces se corre la segunda MT, cuyo lenguaje es L2. Si la misma acepta la entrada entonces el string pertenece a L3, y si la rechaza no. De esta forma, si creamos una MT que se encargue de ejecutar la MT con lenguaje L1 y la MT con lenguaje L2 (siguiendo los pasos indicados) se estaría reconociendo L3, deteniendose para todos los casos, probando que la unión de L1 y L2 también pertenece a R. 
2. Siguiendo la lógica aplicada en el inciso anterior, dados dos lenguajes L1 y L2 que pertenecen a RE, sabemos que sus MT reconocedoras pueden no detenerse para todas las cadenas. Si creamos una $MT_3$  que corra las MT reconocedoras de L1 y L2 en orden, esta $MT_3$ debería aceptar las cadenas que acepten ambas MT reconocedoras y en el caso de que cualquiera de las dos rechace debería rechazar también. Esto no es un problema, ya que si alguna de las MT reconocedoras loopea, $MT_3$ loopea también, rechazando la entrada de forma correcta, demostrando entonces que $L(MT_3) = L1 \cap L2$ está en RE.  

### Ejercicio 5
Sean L1 y L2 dos lenguajes recursivamente numerables de números naturales codificados en unario (por ejemplo, el número 5 se representa con 11111). 
Probar que también es recursivamente numerable el lenguaje L = {x | x es un número natural codificado en unario, y existen y, z, tales que y + z = x, con y ∈ L1, z ∈ L2}. 
*Ayuda: la prueba es similar a la vista en clase, de la clausura de la clase RE con respecto a la operación de concatenación.*

Teniendo en cuenta que en el lenguaje unario, la suma es lo mismo que una concatenación, demostramos de manera similar:

Dadas las MT M1, $L(M1) = L_1$, y M2, $L(M2) = L_2$, definimos una MT M, $L(M) = L$, que hará los siguientes pasos.

- Paso 1: M corre M1 sobre los primeros 0 simbolos de la entrada y M2 sobre los últimos N simbolos de la entrada, si ambas MT aceptan, entonces M acepta, de lo contrario sigue. 
- Paso 2: M corre M1 sobre el primer símbolo de la entrada y M2 sobre los últimos (N - 1) simbolos de la entrada, si ambas MT aceptan, entonces M acepta, de lo contrario sigue. 
- ...
- Paso N: M corre M1 sobre los primeros n simbolos de la entrada y M2 sobre los últimos 0 simbolos de la entrada, si ambas MT aceptan, entonces M acepta, de lo contrario rechaza.

De esta manera, se demuestra que el lenguaje **L es reconocible**, pero dado que L1 y L2 están en RE, puede darse el caso de que M1 o M2 no se detengan, por consiguiente **M podría no detenerse**, definiendo entonces a L como recursivamente enumerable (RE).

### Ejercicio 6
1.  Para encontrar al menos una cadena que M1 acepta, M2 debería generar todas las posibles cadenas dadas por el alfabeto (se debería generar $\Sigma^*$) y correr a M1 sobre cada una, evitando que M1 loopee haciendo loopear a M2. Para evitar que M1 loopee, se deben limitar la cantidad de pasos que esta puede simular sobre una determinada cadena. Entonces, M2 podría correr M1 sobre cada entrada limitada a unos k pasos antes de pasar a la siguiente. Sin embargo, con esta configuración limitaríamos a todas las cadenas a ser resueltas en k pasos, lo que es incorrecto. Para evitar esto, podemos hacer que M2 itere de la siguiente manera:
	   - M2 simula M1 con las primeras 1 cadenas de $\Sigma^*$ con k pasos o hasta que rechaza, si M1 acepta entonces M2 acepta, sino continua.
	   - M2 simula M1 con cada una de las primeras 2 cadenas de $\Sigma^*$ con k * 2 pasos o hasta que rechaza, si M1 acepta alguna entonces M2 acepta, sino continua.
	   - ...
	   - M2 simula M1 con las primeras i cadenas de $\Sigma^*$ con k * i pasos o hasta que rechaza, si M1 acepta alguna entonces M2 acepta, sino continua.
	De esta manera, nos aseguramos que M1 reciba todas las cadenas posibles y con un número de pasos incremental, evitando loops y probando todo $\Sigma^*$

2. No se puede construir una MT M3, utilizando la MT M1, que acepte, cualquiera sea su cadena de entrada, sii la MT M1 acepta a lo sumo una cadena, ya que para esto se deberían probar **todas** las cadenas de $\Sigma^*$ lo cual es imposible dado que $\Sigma^*$ es infinito. Cabe recalcar que la MT M3 podría rechazar en casos donde encuentre 2 cadenas que M1 acepta, pero M3 no podría aceptar nunca ya que no se puede verificar con una MT que no existe una segunda cadena aceptada. 