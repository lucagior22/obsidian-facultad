# 1- Defina y diferencie programa concurrente, programa distribuido y programa paralelo. 

| Concurrente                                                | Distribuido                                                | Paralelo                                                         |
| ---------------------------------------------------------- | ---------------------------------------------------------- | ---------------------------------------------------------------- |
| División de un problema en múltiples tareas independientes | Programa concurrente que se comunica por medio de mensajes | Ejecución de las tareas de un problema concurrente en simultáneo |


# 2- Marque al menos 2 similitudes y 2 diferencias entre los pasajes de mensajes sincrónicos y asincrónicos. 

- Similitudes:
	- No hacen uso de variables compartidas.
	- Se basan en las operaciones send y receive.
- Diferencias:
	- PMA Basado en canales, PMS no conoce ese concepto.
	- En PMA el *send* no es bloqueante.
# 3- Analice qué tipo de mecanismos de pasaje de mensajes son más adecuados para resolver problemas de tipo Cliente/Servidor, Pares que interactúan, Filtros, y Productores y Consumidores. Justifique claramente su respuesta. 
- PMS:
	- Mejor en la eficiencia de la memoria distribuida.
		- No existen las estructuras de canales.
- PMA:
	- Mejor en la concurrencia.
		- El *send* no es bloqueante, por ende se puede enviar un mensaje y seguir trabajando.

# 4- Indique por qué puede considerarse que existe una dualidad entre los mecanismos de monitores y pasaje de mensajes. Ejemplifique 
En pasaje de mensajes los procesos hacen llamados a canales de una manera similar a como se comportan los llamados a procedures de un monitor.
# 5- ¿En qué consiste la comunicación guardada (introducida por CSP) y cuál es su utilidad? Describa cómo es la ejecución de sentencias de alternativa e iteración que contienen comunicaciones guardadas. 
La comunicación guardada es una forma de condicionar la ejecución de una comunicación. Se aplica la espera selectiva, a través de una condición booleana y del conocimiento de si una comunicación puede ser ejecutada inmediatamente o no.
# 6- Modifique la solución con mensajes sincrónicos de la Criba de Eratóstenes para encontrar los números primos detallada en teoría de modo que los procesos no terminen en deadlock. 
pass

# 7- Explique brevemente los 7 paradigmas de interacción entre procesos en programación distribuida vistos en teoría. En cada caso ejemplifique, indique qué tipo de comunicación por mensajes es más conveniente y qué arquitectura de hardware se ajusta mejor. Justifique sus respuestas. 
- **Master-Worker**
	- Derivado de "Cliente-Servidor".
	- Concepto de *Bag of Tasks*
		- Master agrega tareas a la bolsa.
		- Workers toman tareas de la bolsa.
- **Heartbeat**
	- Derivado de "Pares que interactuan".
	- Se forman grupos de procesos, donde cada proceso tiene 2 vecinos, uno del que recibe y otro al que envia.
- **Pipeline**
	- Derivado de "Productor-Consumidor"
	- División en etapas. 
- **Probes & Echoes**
	- Para recorrido de arboles o grafos.
	- Se basa en el envio de un mensaje de prueba (*probe*) y la espera de una respuesta (*echo*). 
	- Análogo concurrente de DFS.
- **Broadcast**
	- Pasaje de mensaje global.
- **Token Passing**
	- Existe un tipo especial de mensaje que otorga permisos de acceso a determinada información.
	- Procesos intermedios que controlan los permisos y administran el token.
- **Servidores Replicados**

# 8- Describa el paradigma “bag of tasks”. ¿Cuáles son las principales ventajas del mismo? 
El concepto de bag of tasks usando variables compartidas supone que un conjunto de workers comparten una “bolsa” con tareas independientes. Los workers sacan una tarea de la bolsa, la ejecutan, y posiblemente crean nuevas tareas que ponen en la bolsa.

# 9- Suponga n2 procesos organizados en forma de grilla cuadrada. Cada proceso puede comunicarse solo con los vecinos izquierdo, derecho, de arriba y de abajo (los procesos de las esquinas tienen solo 2 vecinos, y los otros en los bordes de la grilla tienen 3 vecinos). Cada proceso tiene inicialmente un valor local v. 
>a) Escriba un algoritmo heartbeat que calcule el máximo y el mínimo de los n2 valores. Al terminar el programa, cada proceso debe conocer ambos valores. (Nota: no es necesario que el algoritmo esté optimizado). 
>b) Analice la solución desde el punto de vista del número de mensajes. 
>c) Puede realizar alguna mejora para reducir el número de mensajes? 
>d) Modifique la solución de a) para el caso en que los procesos pueden comunicarse también con sus vecinos en las diagonales. 

# 10- Marque similitudes y diferencias entre los mecanismos RPC y Rendezvous. Ejemplifique para la resolución de un problema a su elección. 
| RPC                                      | Rendezvous                               |
| ---------------------------------------- | ---------------------------------------- |
| División del programa en módulos.        | División del programa en módulos.        |
| Comunicación bidireccional y sincrónica. | Comunicación bidireccional y sincrónica. |
| Los procesos se crean bajo una demanda.  | Los procesos existen previo al llamado.  |

# 11- Considere el problema de lectores/escritores. Desarrolle un proceso servidor para implementar el acceso a la base de datos, y muestre las interfaces de los lectores y escritores con el servidor. Los procesos deben interactuar: 
>a) con mensajes asincrónicos; 
>b) con mensajes sincrónicos; 
>c) con RPC; 
>d) con Rendezvous. 

# 12- Suponga que N procesos poseen inicialmente cada uno un valor. Se debe calcular la suma de todos los valores y al finalizar la computación todos deben conocer dicha suma. 
> a) Analice (desde el punto de vista del número de mensajes y la performance global) las soluciones posibles con memoria distribuida para arquitecturas en estrella (centralizada), anillo circular, totalmente conectada, árbol y grilla bidimensional. 
> b) Escriba las soluciones para las arquitecturas mencionadas. 

# 13- Describa sintéticamente las características de sincronización y comunicación de ADA.

- **Comunicación**
	- Bidireccional.
	- *Entry call* y *accept*.
	- Sincrónica.
- **Sincronización**
	- Exclusión mutua implícita.
	- Por condición.