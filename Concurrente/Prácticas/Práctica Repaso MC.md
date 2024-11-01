# Semaforos
---
# 3
> Implemente una solución para el siguiente problema. Se debe simular el uso de una máquina expendedora de gaseosas con capacidad para 100 latas por parte de U usuarios. Además, existe un repositor encargado de reponer las latas de la máquina. Los usuarios usan la máquina según el orden de llegada. Cuando les toca usarla, sacan una lata y luego se retiran. En el caso de que la máquina se quede sin latas, entonces le debe avisar al repositor para que cargue nuevamente la máquina en forma completa. Luego de la recarga, saca una botella y se retira. Nota: maximizar la concurrencia; mientras se reponen las latas se debe permitir que otros usuarios puedan agregarse a la fila.

```c
Cola colaEspera;
Sem mutexCola = 1, mutexLibre = 1, esperarRecarga = 0, hayQueRecargar = 0, esperandoEntrar[1..U-1] = ([U] 0);
bool libre = true;
int cantLatas = 100;

Process persona::[id: 0..U-1] {
	P(mutexCola)
	if (!libre) {
		colaEspera.push(id)
		V(mutexCola)
		P(esperandoEntrar[id])
	} else {
		libre = false
		V(mutexCola)
	}

	if (cantLatas == 0) {
		V(hayQueRecargar)
		P(esperarRecarga)
	} 

	// Toma la lata
	cantLatas -= 1

	P(mutexCola)
	if (empty(colaEspera)) {
		libre = true
	} else {
		int idSiguiente = colaEspera.pop()
		V(esperandoEntrar[idSiguiente])
	}
	V(mutexCola)
}

Process repartidor {
	while (true) {
		P(hayQueRecargar)
		for (int i = 0; i < 100; i++) {
			// Recarga las latas
			cantLatas += 1
		}
		V(esperarRecarga)
	}
}


```

# Monitores
---
## 1
> Resolver el siguiente problema. En una elección estudiantil, se utiliza una máquina para voto electrónico. Existen N Personas que votan y una Autoridad de Mesa que les da acceso a la máquina de acuerdo con el orden de llegada, aunque ancianos y embarazadas tienen prioridad sobre el resto. La máquina de voto sólo puede ser usada por una persona a la vez. Nota: la función Votar() permite usar la máquina.

```c
Process persona::[0..N-1] {
	bool prioridad = // Tiene o no prioridad
	Maquina.llegue(id, prioridad);
	// Voto UNIVERSAL, SECRETO y OBLIGATORIO
	Votar();
	Maquina.meVoy(id)
}

Process autoridad {
	for (int i = 0; i < N; i++) {
		Maquina.darAcceso()
	}
}

Monitor Maquina {
	Cola colaOrdenada, espero[0..N-1], autoridad;
	int cantEsperando = 0;
	bool libre = true;

	procedure llegue(int id, bool prioridad) {
		insertarOrdenado(colaOrdenada, id, prioridad) // Se ordena por orden de llegada y prioridad
		cantEsperando += 1;
		wait(espero[id]);
	}

	procedure darAcceso() {
		int id;
		if (cantEsperando == 0) {
			wait(autoridad)
		}
		id = colaOrdenada.pop()
		signal[espero[id]]
		wait(autoridad)
	}

	procedure meVoy() {
		cantEsperando -= 1;
		signal(autoridad)
	}
}
```
---
## 3
> Resolver el siguiente problema. En una montaña hay 30 escaladores que en una parte de la subida deben utilizar un único paso de a uno a la vez y de acuerdo con el orden de llegada al mismo. Nota: sólo se pueden utilizar procesos que representen a los escaladores; cada escalador usa sólo una vez el paso.

```c

Process Escalador::[1..30] {
	Montaña.entrar();
	delay(5.0) // Pasa por la escalada
	Montaña.salir();
}

Monitor Montaña {
	bool pasando = false;
	int cantEsperando = 0;
	cond espera;

	Procedure entrar() {
		if (pasando) {
			cantEsperando += 1;
			wait(espera);
		} else {
			pasando = true;
		}
	}

	Procedure salir() {
		if (cantEsperando > 0) {
			esperando -= 1;
			signal(espera);
		} else {
			pasando = false;
		}
	}

}
```