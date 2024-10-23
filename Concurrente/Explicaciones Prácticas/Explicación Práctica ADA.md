
# Composición de un programa
- Un cuerpo principal.
- TASKs: Código de ejecución concurrente.

# TASKs y TASK TYPEs
- Composición:
	- Especificación
	- Entrys: *Protocolo* o métodos aceptados.

# Rendezvouz
*Encuentro*

- Es el único mecanismo de comunicación de ADA que se puede utilizar en la materia.
- Cómunicación **bidireccional** entre dos procesos mediante un **entry call** y un **accept**.
- Entry call:
	- Simple
		- Bloquea la tarea hasta que finaliza el accept del receptor. 
	- Temporal
		- Espera a lo sumo un tiempo antes de descartar el llamado.
		- Estructura de control select or delay.
	- Condicional
		- Si no se acepta la entry call entonces la descarto.
		- Estructura de control select or delay
- Accept:
	- Wait selectivos:
		- **Select (accepts) or when(cond)**
- Count:
	- Solo la TASK que contiene a un entry a puede conocer la cantidad de llamados pendientes del mismo.
	- !ERROR:
		- when(entryCall´count > 0) => accept entryCall
		- No tiene sentido dado que si no hubieran pedidos entonces no se aceptaría.

# Ejercicios
*Siempre que se pueda aprovechar la comunicación bidireccional de rendezvouz*

### Ejericio 1
- Tasks
	- Cliente
	- Empleado
- Acciones requeridas
	- Esperar atención/Encolarse
	- Esperar respuesta
	  
### Ejercicio 3
*Manejo de prioridades*
- Los prioritarios tienen su entry call separado.
- El entry call no prioritario cuenta con una condición que espera que el **count** del entry call prioritario sea 0 para aceptar.