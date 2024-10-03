## Monitores
*Modulos de programa con mas estructura, y que pueden ser implementados tan eficientemente como los semáforos*

***Mecanismo de abstracción de datos***
- Encapsulan las representaciones de recursos
- Brindan un conjunto de operaciones como únicos medios para manipular esos recursos

*Exclusión mutua*: Asegurada implicitamente dado que los procedures en el mismo monitor no ejecutan concurrentemente.

*Sincronización por Condición*: Explicita con variables condición.

## Estructura de un programa concurrente con monitores

- Procesos activos
- Monitores pasivos

*Los procesos se ejecutan siempre y son estos los que activan a los monitores*

**Ventajas** 
- Los procesos se abstraen de la implementación de los *procedures*
- El programador del monitor ignora donde y como se usan los *procedures*

Un monitor tiene:
- Interfaz: Especifica las operaciones, solo mediante su nombre (firma, parámetros)
- Cuerpo: Tiene las variables que representan el estado del recurso y las implementaciones de los procedures.

El llamado a un procedure se ve asi:
`NombreMonitor.operacion(args)`

Los procedures pueden acceder solo a:
- Variables permanentes
- Variables locales
- Parámetros pasados en la invocación

## Notación de un monitor

```
monitor NombreMonitor {
	declaraciones de variables permanentes;
	código de inicialización;

	procedure op1 (parámetros) {
		// Cuerpo de op1
	}

	procedure opn (parámetros) {
		cuerpo de opn
	}
}

```

## Variables de condición
*Se utiliza para programar la sincronización por condición*

`condición -> cond cv;

cv tiene un valor asociado que es una cola de los procesos demorados.

Operaciones:
*Solo se pueden llamar dentro del monitor que las contenga*
- wait(cv): Encolar el proceso al final de la cola.
- signal(cv): Despertar al proceso que está delante de la cola.
- signal_all(cv): Despierta todos los procesos demorados en cv, vaciando la cola asociada.

*Operaciones que no se utilizan en la práctica:`
- empty(cv): retorna true si la cola controlada por cv está vacia.
- wait(cv, rank): el proceso demorado obtiene su lugar segun el parámetro rank
- minrank(cv): retorna el rango minimo

### Disciplinas de señalización
![[Pasted image 20240912152730.png]]

### Diferencia entre wait/signal vs P/V
![[Pasted image 20240912153636.png]]