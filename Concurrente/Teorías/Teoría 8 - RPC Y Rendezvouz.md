# Caraterísticas

### Compartidas
- Apuntan a solucionar los problemas que requieren comunicación bidireccional.
- Proveen una interfaz de métodos/operaciones que pueden resolver.

### Diferencias
| RPC                                                                                                       | Rendezvouz                                                                                      |
| --------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| Para responder a una llamada se crea un nuevo proceso que resuelve esa llamada específica, y luego muere. | Se utiliza un proceso existente, que es mapeado con el proceso llamante según el requerimiento. |
| Símil monitores                                                                                           | Símil PMS                                                                                       |

# RPC
*Remote Procedure Call*
*Es un mecanismo de comunicación, no de sincronización.*
*No tiene un mecanismo incorporado de exclusión mutua o sincronización para el acceso a las variables locales al módulo.*

### Composición
- Módulos
	- Variables locales.
	- Código de inicialización.
	- Implementación de procesos.
		- Procedures exportados.
		- Procesos locales o backgrounds.
	- Header de procedures visibles.
		- `op opname(formales)[returns result]`

### Funcionamiento
- Llamada intermódulo.
	- Se crea un nuevo proceso que va a actuar para resolver la consulta.
	- Nuevo proceso llamado **Proceso Servidor**.
- Enfoques de sincronización entre módulos dependiendo de si los mismos se ejecutan: 
	- Exclusión mutua.
		- Dentro de un mismo modulo, no se pueden ejecutar mas de un proceso al mismo tiempo.
	- Concurrentemente.

*RPC es similar a RMI en JAVA.*

# Rendezvouz
*Combina comunicación y sincronización.*

### Funcionamiento
- Un proceso cliente/llamante realiza un **call** a una de las operaciones exportadas.
- Un proceso servidor usa una **sentencia de entrada** para esperar por un **call** y actuar.
- Las operaciones se atienden secuencialmente dentro del mismo proceso.

### Ejemplos
- Buffer limitado
	- Un módulo que actua como buffer limitado.
		- Resuelve dos acciones:
			- Depositar.
			- Retirar.
		- Comunicación guardada.
			- Depositar
				- Expresión booleana = "cantidad < n" *donde cantidad es len() y n el límite*
			- Retirar
				- Expresión booleana = "cantidad > 0"
- Filósofos centralizados
	- Un modulo **Mesa**.
		- Exporta:
			- Tomar.
			- Dejar.
		- Proceso *mozo*.
			- Comunicación guardada.
				- Tomar
					- Expresión booleana = "No está comiendo el filósofo a mi izquierda ni el que está a mi derecha."
						- *Esta solución no se podria implementar en ADA ya que no se puede consultar valores de parámetros en la expresión booleana.*
				- Dejar
					- Expresión booleana = No hay
	- Cinco Modulos **Persona**.
		- Proceso *filósofo*.

# ADA con Rendezvouz

### Características
- Los programas de ADA corren sobre memoria compartida.
- En vez de procesos, ada tiene ***tasks*** o tareas que se ejecutan simultaneamente.
- Los puntos de invocación a una *task* se denominan ***entrys***.
	- Declarados en el header de la tarea.
- Una tarea acepta la comunicación o no mediante la primitiva ***accept***.
- Se pueden crear un ***type task*** y luego crear instancias de tareas en base a el mismo.

### Tipos de Entry Call
- Call convencional:
	- Llamo y me demoro hasta recibir la respuesta.
	- `tarea.entry(parametros)`
- Call condicional:
	- Intento llamar ahora y si no me responde lo pospongo.
	- `select <entry call>; <sentencias adicionales>; else; <sentencias>; end select;`
- Call temporal:
	- No quiero perder tiempo, pero establezco un tiempo que puedo demorar hasta que desista del llamado.
	- `select <entry call>; <sentencias adicionales>; or delay <tiempo>; <sentencias>; end select;`

### Accept
`accept <nombre> (<parametros formales>) do <sentencias> end <nombre>;`
- Los parámetros deben ir acompañados por su tipo (IN, OUT, INOUT)
- El proceso caller se demora hasta que el *cuerpo del accept* o sentencias se termina.

# Ejemplos

### Mailbox

```c
TASK TYPE Mailbox IS
	Entry Depositar (msg: IN mensaje);
	Entry Retirar (msg: OUT mensaje);
END Mailbox

A, B, C : Mailbox;

TASK BODY Mailbox IS
	dato : mensaje;
BEGIN
	LOOP
		ACCEPT Depositar(msg : IN mensaje) DO dato := msg; END Depositar;
		ACCEPT Retirar (msg: OUT mensaje) DO msg := dato; END Retirar
	END LOOP;
END Mailbox;
```
- No tener un SELECT implica que se hace una sentencia dentras de la otra. 
- Para transformarlo en un Mailbox para N mensajes, agregando un SELECT y las correspondientes expresiones booleanas para cada operación.