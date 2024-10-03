*Sirve para [[Concurrente/Prácticas/Práctica 3]]*
# Estructura y comunicación

```java
Monitor nombre {
// Variables permanentes del monitor

	Procedure uno () 
	// Variables locales al proceso
	{ ... }

	// Código de inicialización del monitor
	{
		// Inicialización del monitor
	}
}

```

- No existen las variables compartidas
- Las variables permanentes del monitor sólo se pueden usar dentro del mismo.
- De existir un código de inicialización, las llamadas a procedures no se ejecutan hasta que este termine. (Nota sobre la cátedra, en general no se usan los códigos de inicialización)
- Los procesos interactuan entre ellos y con los recursos compartidos mediante llamados a los *procedures* de los monitores.
- Dentro de un *procedure*, un monitor puede hacer un llamado a otro *procedure* de otro monitor. Mientras que se está ejecutando el segundo *procedure*, el primer monitor está demorado en el punto del llamado, lo que demora tambien a todos los procesos que quieran acceder al primer monitor.
- Cuando el monitor está libre, el acceso al mismo no es por orden de llegada.


# Sincronización
*La exclusión mútua es conseguida implicitamente, pero la sincronización se debe manejar*

La sincronización por condición es explícita por medio de variables *Condición* usadas **SOLO** dentro de los monitores.

**Variable condition**:  Colas de organización de procesos demorados, lo que permite liberar el monitor para otros procesos sin requerir que el *procedure* termine.

**Uso**:
- Wait: Duerme al proceso en la cola asociada a la variable condición (al final de la cola).
- Signal: Despierta al primer proceso dormido para que compita nuevamente para acceder al monitor, y cuando lo haga el proceso despertado continuará con la instrucción después del wait.
- Signal_all: Despierta a **todos** los procesos dormidos en la variable condición.

# Ejercicio 1

- N personas desean utilizar un cajero automático.
- No se debe tener en cuenta el orden de llegada.
- Suponga una función *usarCajero()* que simula la acción.

*Estructura del programa*
	Únicamente se requiere que el recurso sea utilizado con exclusión mútua, no se respeta ningún orden ni prioridad.

```python
Monitor Cajero {
	Procedure PasarAlCajero() {
		UsarCajero();
	}
}

Process Persona[id: 0..N-1] {
	Cajero.PasarAlCajero()
}

```

# Ejercicio 2
- Idem al caso anterior.
- Se debe respetar el orden de llegada.

Se requiere un monitor donde se forme la cola

```python
Monitor Cajero {
	bool libre = true;
	cond cola;

	Procedure Pasar() {
		if (not libre) { wait(cola) }
		libre = false
	}

	Procedure Salir() {
		libre = true;
		signal(cola)
	}
}

Process Persona[id: 0..N-1] {
	Cajero.Pasar();
	usarCajero();
	Cajero.Salir();
}
```
*En este caso, no se respeta el orden dado que cuando se realiza signal, no se asegura que sea ese proceso el que acceda a la SC*

Modificación pertinente:
```python
Monitor Cajero {
	bool libre = true;
	cond cola;
	int esperando = 0;

	Procedure Pasar() {
		if (not libre) { 
			esperando++;
			wait(cola)
		} else {
			libre = false
		}
	}

	Procedure Salir() {
		if (esperando > 0) {
			esperando--;
			signal(cola);
		} else {
			libre = true;
		}
	}
}

Process Persona[id: 0..N-1] {
	Cajero.Pasar();
	usarCajero();
	Cajero.Salir();
}
```
*Teóricamente se podría utilizar la operación empty(), pero no en la práctica donde se simula con un entero contador de procesos dormidos.*

# Ejercicio 3
- Idem anterior.
- Si llega una persona anciana tiene prioridad.

*La cola debe estar ordenada, donde la operación insertar depende de la edad del usuario*

```python
Monitor Cajero {
	bool libre = true;
	cond espera[N];
	int idAuxiliar, esperando = 0;
	ColaOrdenada fila;

	Procedure Pasar(idProceso, edad: in int) {
		if (not libre) { 
			insertar(fila, idProceso, edad);
			esperando++;
			wait(espera[idProceso])
		} else {
			libre = false
		}
	}

	Procedure Salir() {
		if (esperando > 0) {
			esperando--;
			sacar(fila, idAuxiliar)
			signal(espera[idAuxiliar]);
		} else {
			libre = true;
		}
	}
}

Process Persona[id: 0..N-1] {
	int edad = ...;
	Cajero.Pasar(id, edad);
	usarCajero();
	Cajero.Salir();
}
```
