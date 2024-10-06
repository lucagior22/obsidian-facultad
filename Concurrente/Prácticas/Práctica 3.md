# 1
> ![[Pasted image 20241003103858.png]]
> a. ¿El código funciona correctamente? Justifique su respuesta. 

Si, funciona correctamente. El programa no necesita de la sentencia while, dado que podria ser un if y cumpliría exactamente la misma función.

> b. ¿Se podría simplificar el programa? ¿Sin monitor? ¿Menos procedimientos? ¿Sin variable condition? En caso afirmativo, rescriba el código. 

Si, con un semáforo para exclusión mutua.
```c
Sem mutex = 1;

Process auto::[1..M] {
	P(mutex)
	// Pasa por el puente.
	V(mutex)
} 
```

---
# 2

>Existen N procesos que deben leer información de una base de datos, la cual es administrada por un motor que admite una cantidad limitada de consultas simultáneas. 
>
>a) Analice el problema y defina qué procesos, recursos y monitores/sincronizaciones serán necesarios/convenientes para resolverlo.
>b) Implemente el acceso a la base por parte de los procesos, sabiendo que el motor de base de datos puede atender a lo sumo 5 consultas de lectura simultáneas.

```c

Process proceso::[1..N] {
	MotorBD.iniciarLectura();
	Leer();
	MotorBD.finalizarLectura();
}

Monitor MotorBD {
	int limite = 5, lectoresActual = 0;
	cond cola;

	procedure iniciarLectura() {
		if (lectoresActual >= 5) {
			wait(cola);
		}

		lectoresActual += 1;
	}

	procedure finalizarLectura() {
		lectoresActual -= 1;
	}

}
```

---
# 3

> Existen N personas que deben fotocopiar un documento. La fotocopiadora sólo puede ser usada por una persona a la vez. Analice el problema y defina qué procesos, recursos y monitores serán necesarios/convenientes, además de las posibles sincronizaciones requeridas para resolver el problema. Luego, resuelva considerando las siguientes situaciones: 
> 
> a) Implemente una solución suponiendo no importa el orden de uso. Existe una función Fotocopiar() que simula el uso de la fotocopiadora. 

```c
Process Persona::[1..N] {
	Fotocopiadora.Usar();
	// Usa fotocopiadora
	Fotocopiadora.Liberar();
}

Monitor Fotocopiadora {
	cond cola;
	bool libre = True;

	procedure Usar() {
		if ( !libre ) {
			wait(cola);
		}

		libre = False;
	}

	// Dado que no importa el orden, pongo la variable que maneja el flujo en true y levanto al que sea. Un proceso puede adelantarse al que acabo de levantar y entrar dado que libre == true.
	procedure Liberar() {
		libre = True;
		signal(cola);
	}
}

```

> b) Modifique la solución de (a) para el caso en que se deba respetar el orden de llegada. 

```c
Process Persona::[1..N] {
	Fotocopiadora.Usar();
	Fotocopiar();
	Fotocopiadora.Liberar();
}

Monitor Fotocopiadora {
	cond cola;
	bool enUso = false;
	int esperando = 0;

	Procedure Usar() {
		if (enUso) { esperando++; wait(cola) }
		enUso = true;		
	}

	// Como en este caso SI importa el orden, me aseguro de que solo el proceso que despierto pueda avanzar. De haber un proceso que le gane al despertado en el uso del monitor, nunca va a pasar al uso de la fotocopiadora, solo el que yo desperté lo hará. 
	Procedure Liberar() {
		if (esperando > 0) {
			esperando--;
			signal(cola);
		} else {
			enUso = false;
		}
	}
// La regla: Si hay alguien esperando, lo despierto SIN MODIFICAR LA VARIABLE que controla el flujo, si nadie espera, habilito la entrada directa MODIFICANDO LA VARIABLE de control de flujo.
}
```

> c) Modifique la solución de (b) para el caso en que se deba dar prioridad de acuerdo con la edad de cada persona (cuando la fotocopiadora está libre la debe usar la persona de mayor edad entre las que estén esperando para usarla). 

```c
Process Persona::[id: 1..N] {
	int edad = random(1..100)
	Fotocopiadora.Usar(edad, id);
	Fotocopiar();
	Fotocopiadora.Liberar();
}

Monitor Fotocopiadora {
	cond esperando[N];
	bool enUso = false;
	int esperando = 0;

	Procedure Usar(int edad, int id) {
		if (enUso) { 
			esperando++; 
			insertarOrdenado(cola, edad, id);
			wait(esperando[id]);	
		}
		enUso = true;		
	}

	Procedure Liberar() {
		if (esperando > 0) {
			esperando--;
			id = sacar(cola)
			signal(esperando[id]);
		} else {
			enUso = false;
		}
	}
}
```

> d) Modifique la solución de (a) para el caso en que se deba respetar estrictamente el orden dado por el identificador del proceso (la persona X no puede usar la fotocopiadora hasta que no haya terminado de usarla la persona X-1). 

```c
Process Persona::[id: 1..N] {
	Fotocopiadora.Usar(id);
	Fotocopiar();
	Fotocopiadora.Liberar();
}

Monitor Fotocopiadora {
	cond esperando[N]; // Un array de VC donde cada proceso se duerme segun su id.
	int proximo = 1;

	Procedure Usar(int id) {
		if (proximo != id) { 
			esperando++; 
			wait(esperando[id]);	
		}		
	}

	Procedure Liberar() {
		proximo =+ 1
		signal(esperando[proximo])
	}
}
```

> e) Modifique la solución de (b) para el caso en que además haya un Empleado que le indica a cada persona cuando debe usar la fotocopiadora. 

```c
Process Persona::[1..N] {
	Fotocopiadora.Usar();
	Fotocopiar();
	Fotocopiadora.liberar();
}

Process Empleado {
	while (true) {
		Fotocopiadora.Habilitar()
	}
}

Monitor Fotocopiadora {
	cond cola, empleado, impresoraLibre;
	int esperando = 0;

	Procedure Usar() {
		esperando++;
		signal(empleado); // Aviso que llegué.
		wait(cola); // Espero a que me llamen.
	}

	Procedure Liberar() {
		signal(impresoraLibre) // Aviso que terminé.
	}

	Procedure Habilitar() {
		if (esperando == 0) {
			wait(empleado) // Espero a que alguien me despierte, lo que indica que alguien quiere usar la fotocopiadora.
		}
		esperando-- 
		signal(cola) // Le aviso al próximo que avance.
		wait(impresoraLibre) // Espero a que me avisen que se terminó el uso de la impresora.
	}

}
```

> f) Modificar la solución (e) para el caso en que sean 10 fotocopiadoras. El empleado le indica a la persona cuál fotocopiadora usar y cuándo hacerlo.

```c
Process Persona::[1..N] {
	Fotocopiadora.Usar();
	Fotocopiar();
	Fotocopiadora.liberar();
}

Process Empleado {
	while (true) {
		Fotocopiadora.Habilitar()
	}
}

Monitor Fotocopiadora {
	cond esperoAsignacion[N], empleado, impresoraLibre;
	Cola esperandoUso, impresoras[10] = ( 1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
	Array fotocopiadoraAsignada[N] = ([N] 0)
	int esperando = 0, impresorasLibres = 0;

	Procedure Usar(int id, OUT int fotocopiadora) {
		esperando++;
		esperandoUso.push(id);
		signal(empleado);
		wait(esperoAsignacion[id]);
		fotocopiadora = fotocopiadoraAsignada[id];
	}

	Procedure Liberar(int fotocopiadora) {
		impresorasLibres++
		impresoras.push(fotocopiadora)
		signal(impresoraLibre)
	}

	Procedure Habilitar() {
		if (esperando == 0) {
			wait(empleado);
		}
		esperando--; 
		id = esperandoUso.pop();
		if (impresorasLibres == 0) {
			wait(impresoraLibre)
		}
		impresorasLibres--
		fotocopiadoraAsignada[id] = impresoras.pop()
		signal(esperandoAsignacion[id])
	}

}
```

---
# 4
> Existen N vehículos que deben pasar por un puente de acuerdo con el orden de llegada. Considere que el puente no soporta más de 50000kg y que cada vehículo cuenta con su propio peso (ningún vehículo supera el peso soportado por el puente).

```c
Process vehiculo::[1..N] {
	int peso = miPeso() // Obtengo el peso del vehiculo
	Puente.entrar();
	// Paso por el puente
	Puente.salir();
}

Monitor Puente {
	cond cola, colaAdecuados;
	int pesoActual = 0, esperando = 0;
	bool libre = True;

	procedure entrar(int peso) {
		if ( !libre ) {
			esperando += 1;
			wait(cola)
		} else {
			libre = false;
		}

		while ( pesoActual + peso > 50000 ) { 
			wait(colaAdecuado) // Acá siempre hay un solo proceso
		}

		pesoActual += peso;
		
		esperando -= 1;
		if (esperando > 0) {
			signal(cola)
		} else {
			libre = true
		}
	}

	procedure salir(int peso) {
		pesoActual -= peso;
		signal(colaAdecuado)
	}
}
```


---
# 5
> En un corralón de materiales se deben atender a N clientes de acuerdo con el orden de llegada. Cuando un cliente es llamado para ser atendido, entrega una lista con los productos que comprará, y espera a que alguno de los empleados le entregue el comprobante de la compra realizada. 
> a) Resuelva considerando que el corralón tiene un único empleado.

```c
Monitor Corralon{
    String lista
    String comprobante
    cond empleado, termino, agarro
    boolean listo
    
    procedure iniciarAtencion(IN l, OUT c) {
        lista = l
        listo = true
        signal(empleado)
        wait(termino)
        c = comprobante
        signal(agarro)
    }
    
    procedure esperarAtencion(OUT l) {
        if (not listo)
            wait(empleado)
        l = lista
    }
    
    procedure entregarComprobante(IN c) {
        comprobante = c
        signal(termino)
        listo = false
        wait(agarro)
    }
    
}

Monitor Corralon_Cola{
    boolean libre = true
    cond espera
    int esperando = 0
    
    procedure llegada() {
        if not libre {
            esperando++
            wait(espera)
        }
        else {
            libre = false
        }
    }

    procedure salida() {
        if not libre {
            esperando--
            signal(espera)
        }
        else {
            libre = true
        }
    } 
}

Process cliente::[1..N] {
    string lista;
    string comprobante;
    corralon_cola.llegada()
    corralon.iniciarAtencion(lista,comprobante)
    corralon_cola.salida()
}

Process Empleado {
    while(true) {
		Corralon.esperarAtencion(lista)
        comprobante = //crear comprobante
        delay(2min)
        Corralon.entregarComprobante(comprobante)
    }   
}
```

>  b) Resuelva considerando que el corralón tiene E empleados (E > 1). Los empleados no deben terminar su ejecución. 

```c

```

> c) Modifique la solución (b) considerando que los empleados deben terminar su ejecución cuando se hayan atendido todos los clientes.


---
# 6
>Existe una comisión de 50 alumnos que deben realizar tareas de a pares, las cuales son corregidas por un JTP. Cuando los alumnos llegan, forman una fila. Una vez que están todos en fila, el JTP les asigna un número de grupo a cada uno. Para ello, suponga que existe una función AsignarNroGrupo() que retorna un número “aleatorio” del 1 al 25. Cuando un alumno ha recibido su número de grupo, comienza a realizar su tarea. Al terminarla, el alumno le avisa al JTP y espera por su nota. Cuando los dos alumnos del grupo completaron la tarea, el JTP les asigna un puntaje (el primer grupo en terminar tendrá como nota 25, el segundo 24, y así sucesivamente hasta el último que tendrá nota 1). Nota: el JTP no guarda el número de grupo que le asigna a cada alumno.

- 25 pares de alumnos
- 1 JTP
- Los alumnos llegan y forman una fila.
- Hasta que no estan todos en la fila, no se les asigna un grupo a cada uno.
- Una vez con número de grupo, el alumno comienza su tarea.
- Cuando el alumno termina, le avisa al JTP y espera su nota.
- Cuando un par terminó su tarea, el JTP les asigna un puntaje segun el orden de finalización.


```c
Process alumno::[id : 1..50] {
	Escritorio.llegue(id)
	delay(50000) // Realiza la tarea
	Escritorio.termine(id)
}

Process JTP {
	Escritorio.esperarLlegada()
	Escritorio.asignarGrupos()
	for (int i = 0; i < 50; i++) {
		Escritorio.esperarEntrega()
	}
}

Monitor Escritorio {
	cond esperoLlegada, esperamosGrupo, esperoEntrega, esperoJTPLibre, esperoPuntaje[25];
	int alumnosPresentes = 0, puntajeProximo = 25, cantEsperandoCorreccion = 0;
	Cola alumnos, colaEsperandoCorreccion;
	Array grupo[50] = ([50] 0), terminaronPorGrupo[25] = ([25] 0), puntajePorGrupo[25] = ([25] 0);
	bool corrigiendo = false;

	procedure esperarLlegada() {
		wait(esperoLlegada)
	}

	procedure asignarGrupos() {
		for ( int i = 1; i < 50; i++) {
			id = alumnos.pop()
			grupo[id] = AsignarNroGrupo()
		}
		signal_all(esperamosGrupo)
	}

	procedure esperarEntrega() {
		if (cantEsperandoCorreccion == 0) {
			wait(esperoEntrega)
		}

		id = colaEsperandoCorreccion.pop()
		terminaronPorGrupo[grupo[id]] += 1
		if (terminaronPorGrupo[grupo[id]] == 2) {
			puntajePorGrupo[grupo[id]] = puntajeProximo
			puntajeProximo -= 1
			signal_all(esperandoPuntaje[grupo[id]])
		}
	}

	procedure llegue(int IN id) {
		alumnosPresentes += 1
		alumnos.push(id)
		if (alumnosPresentes == 50) {
			signal(esperoLlegada)
		}
		wait(esperamosGrupo)
	}

	procedure termine(int IN id) {
		cantEsperandoCorreccion += 1
		colaEsperandoCorreccion.push(id)
		signal(esperoEntrega)
		wait(esperoPuntaje[grupo[id]])
	}

}
```

---
# 10
> En un parque hay un juego para ser usada por N personas de a una a la vez y de acuerdo al orden en que llegan para solicitar su uso. Además, hay un empleado encargado de desinfectar el juego durante 10 minutos antes de que una persona lo use. Cada persona al llegar espera hasta que el empleado le avisa que puede usar el juego, lo usa por un tiempo y luego lo devuelve. Nota: suponga que la persona tiene una función Usar_juego que simula el uso del juego; y el empleado una función Desinfectar_Juego que simula su trabajo. Todos los procesos deben terminar su ejecución.


```c
process persona::[id : 1..N] {
	Juego.llegue()
	Usar_juego()
	JuegoSalida.salir()
}

process empleado {
	for (int i = 0; i < N; i++) {
		Desinfectar_juego()
		Juego.siguiente()
		JuegoSalida.seFue()
	}
}

Monitor Juego {
	int cantEsperando = 0;
	cond colaEspera, empleado;

	procedure llegue() {
		cantEsperando += 1
		signal(empleado)
		wait(colaEspera)
	}

	
	procedure siguiente() {
		if (cantEsperando == 0) {
			wait(empleado)
		}
		cantEsperando--
		signal(colaEspera)
	}
	
}

Monitor JuegoSalida {
	cond limpiar
	Boolean seFue = False

	procedure salir(){
		seFue = True
		signal(limpiar)
	}

	procedure seFue(){
		if(not seFue) {
			wait(limpiar)
		}
		seFue = False
	}
}
```