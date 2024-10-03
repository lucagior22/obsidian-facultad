
El protocolo busy-waiting es ineficiente si se la utiliza en multiprogramación.

# Semáforos:
	Instancia de un tipo de datos abstracto con solo dos operaciones atómicas
Operaciones:
- V: Señala la ocurrencia de un evento, incrementando el estado interno
- P: Demora un proceso hasta que ocurra un evento, luego decrementa el estado interno 

### Declaraciones:

	sem s; **Incorrecto, se debe inicializar en la declaración**
	sem mutex = 1
	sem fork[5] = ([5] 1)

### Semáforo general

	P(s): <await (s > 0); s = s - 1>
	V(s): < s = s + 1 >

### Semáforo binario

	P(b): < await (b > 0) b = b - 1 >
	V(b): < await (b < 1) b = b + 1 >

### Semáforo de señalización
Comunicación entre varios procesos, donde cada uno avisa que se dió un evento y luego espera a que el otro haya sufrido el evento tambien. Generalmente inicializado en 0
	
		V(llega1); P(llega2)

### Semáforo binario dividido
Para una serie b1, ..., bn se cumple la invariante  `SPLIT: 0 <= b1 + ... + bn <= 1`

SBD para un problema de Productor/Consumidor

```java
	typeT buf; sem vacio = 1, lleno = 0

	process Productor [i = 1 to M] {
		while (true) {
			...
			// Producir datos
			P(vacio); buf = datos; V(lleno) // Depositar
		}
	}

	process Consumidor [i = 1 to N] {
		while (true) {
			P(lleno); resultado = buf; V(vacio); // Retirar
			// Consumir resultado
		}
	}
```

Se cumple `SPLIT: 0 <= vacio + lleno <= 1 `

### Utilizar los semáforos como contadores de recursos libres

*Ejemplo:* Un buffer es una cola de mensajes depositados y aun no buscados. Existe 1 productor y 1 consumidor que depositan y retiran los elementos del buffer.
![[Pasted image 20240907092739.png]]

```java
	typeT buf[n]; int ocupado = 0, libre = 0
	sem vacio = n, lleno = 0

	process Productor {
		while (true) {
			// Producir datos
			P(vacio); buf[libre] = datos; libre = (libre + 1) mod n; V(lleno);
			
		}
	}

	process Consumidor {
		while (true) {
			P(lleno); buf[ocupado] = resultado; ocupado = (ocupado + 1) mod n; V(vacio);
			// consumir resultado
		}
	}
	
```

Esta solución sirve para una relacion 1 a 1 entre Productores/Consumidores y un buffer te tamaño n

*En el caso de que ecistan mas de un productor y/o comprador:*
Se debe aplicar exclusión mútua para las acciones de retirar/producir

```java
type buf[n]; int ocupado = 0, libre = 0;
sem vacio = n, lleno = 0;
sem mutexD = 1, mutexR = 1; // Mutex Depositar y Retirar

	process Productor {
		while (true) {
			// Producir datos
			P(vacio);
			P(mutexD); buf[libre] = datos; libre = (libre + 1) mod n; V(mutexD)
			V(lleno);
		}
	}

	process Consumidor {
		while (true) {
			P(lleno); 
			P(mutexR); buf[ocupado] = resultado; ocupado = (ocupado + 1) mod n; V(mutexR); 
			V(vacio);
			// consumir resultado
		}
	}

```


### Varios procesos compitienod por varios recursos compartidos

n procesos p1, p2, ..., pn
m recursos r1, r2, ..., rm

 Puede darse *deadlock* cuando varios procesos compiten por conjuntos superpuestos de recursos

***Problema de los filósofos***
*5 filosofos cuentan con 4 tenedores, para comer tiene que contar con 2 tenedores.*

```java
process Filosofo [i = 0 to 4]{
	while (true) {
		// adquiere tenedores
		come;
		// libera tenedores
		piensa;
	}
}
```

Levantar un tenedor => P
Bajar un tenedor => V

*exclusión mútua selectiva*

```java
sem tenedores [5] = {1, 1, 1, 1, 1}
	process Filosofos [i = 0..3] {
		while (true) {
			P(tenedor[i]); P(tenedor[i+1]);
			Come;
			V(tenedor[i]); V(tenedor[i+1]);
		}
	}

	process Filosofos [4] {
		while (true) {
			P(tenedor[0]); P(tenedor[4]]);
			come;
			V(tenedor[0]); V(tenedor[4]]);		
		}
	}
```

## Passing the Baton

Implementado con un SBS (Semáforo Binario Selectivo)

- Técnica general para implementar sentencias *await*
- Cuando un proceso está dentro de una SC mantiene el *baton* (testimonio, token) que implica el permiso para ejecutar.
- Cuando el proceso va a salir de la SC ejecuta un *Signal*, lo que entrega el *baton* al siguiente proceso que lo requiera, de no haber ninguno, el *baton* se libera.

Componentes:
- Semaforo ***e*** binario inicialmente 1 (controla la entrada a sentencias atómicas)
- Semaforo ***bj*** para demorar procesos esperando que ***Bj*** sea true.
- Un contador ***dj*** del número de procesos demorados sobre ***bj***

Igualdades:
`< Si >`
```java
P(e); Si; SIGNAL;
```

`< await (Bj); Sj >`
```java
P(e);
if (not Bj) {dj = dj + 1; V(e); P(bj)}
Sj;
SIGNAL
```

`SIGNAL`
```java
if (B1 and d1 > 0) {d1 = d1 - 1; V(b1)}
...
if (Bn and dn > 0) {dn = dn - 1; V(bn)}
else V(e)
fi
```

El rol del *SIGNAL* es el de señalizar exactamente a uno de los semáforos para pasarle el *baton*.

## Alocación y Scheduling

### Shortest Job Next (SJN)
- Recurso de una sola unidad
- request (tiempo de trabajo, id de proceso)
- release ()
	- El recurso se libera para el proceso que tenga el menor tiempo de trabajo
	- En caso de que el minimo tiempo sea compartido entre mas de un proceso se elige al que esperó mas

```python
request(tiempo, id):
	P(e)
	if (not libre) DELAY
	libre = false
	SIGNAL

release():
	P(e)
	libre = true
	SIGNAL
```

DELAY: 
- El proceso inserta sus parametros en un conjunto, cola o lista ordenada llamada *Pares*
- Libera la SC ejecutando V(e)
- Se demora en un semáforo hasta que *request* pueda ser satisfecho

SIGNAL:
- Cuando el recurso es liberado, se analiza *Pares* y se asigna el recurso segun **SJN**

***Semáforos privados***
Si solamente un proceso ejecuta operaciones *P* sobre un determinado semáforo, este es privado.
- Resultan útiles para señalar procesos individuales.
- ***b[id]*** son de este tipo
