
# Ejercicio 1

>Existen N personas que deben ser chequeadas por un detector de metales antes de poder ingresar al avión. 
>   
>   a. Analice el problema y defina qué procesos, recursos y semáforos/sincronizaciones serán necesarios/convenientes para resolverlo. 
>   
>   b. Implemente una solución que modele el acceso de las personas a un detector (es decir, si el detector está libre la persona lo puede utilizar; en caso contrario, debe esperar). 
>   
>   c. Modifique su solución para el caso que haya tres detectores. 
>   
>   d. Modifique la solución anterior para el caso en que cada persona pueda pasar más de una vez, siendo aleatoria esa cantidad de veces.
### a)
 Procesos: Persona.
 Recursos: Detector de metales, simulado con la función de detectar().
 Semáforos: El detector de metales es una SC dado que solo puede ser utilizada de a una persona, por lo que se requiere un semáforo para el acceso a este recurso.

### b)

```c
sem mutexDetector = 1

// Las N personas
Process Persona::[id : 1..N] {
	P(mutexDetector)
	detectar()
	V(mutexDetector)
}

```

### c)


```c
sem mutexDetector = 3

Process Persona::[id : 1..N] {
	P(mutexDetector)
	detectar()
	V(mutexDetector)
}

```

### d)

```c
	sem mutexDetector = 3

	Process Persona::[id : 1..N] {
		int iteraciones = random()
		while (iteraciones > 0) {
			P(mutexDetector)
			detectar()
			V(mutexDetector)
			iteraciones--
		}
	}
```

---

# Ejercicio 2

> Un sistema de control cuenta con 4 procesos que realizan chequeos en forma colaborativa. Para ello, reciben el historial de fallos del día anterior (por simplicidad, de tamaño N). De cada fallo, se conoce su número de identificación (ID) y su nivel de gravedad (0=bajo, 1=intermedio, 2=alto, 3=crítico). Resuelva considerando las siguientes situaciones: 
>  
>    a) Se debe imprimir en pantalla los ID de todos los errores críticos (no importa el orden). 
>   
>    b) Se debe calcular la cantidad de fallos por nivel de gravedad, debiendo quedar los resultados en un vector global. 
>   
>    c) Ídem b) pero cada proceso debe ocuparse de contar los fallos de un nivel de gravedad determinado.

### a)

```c
Cola fallos[N] = // Se importan los datos
int restantes = N
sem mutex = 1

Process proceso::[id : 0..3] {
	Fallo fallo
	P(mutex)
	while (restantes > 0) {
		fallo = fallos.pop()
		restantes--
		V(mutex)
		if (fallo.getNivel() == 3) {
			print(fallo.getId())
		}
		P(mutex)
	}
	V(mutex) // Corrección hecha por Beiser correctamente, sino el programa nunca termina
}
```

### b)

```c
Cola fallos[N] = // Se importan los datos
sem mutexFallos = 1
int restantes = N

int fallosPorNivel[4] = ([4] 0)
sem mutexFallosPorNivel[4] = ([4] 1)

Process proceso::[id : 0..3] {
	Fallo fallo
	int nivel
	P(mutexFallos)
	while (restantes > 0) {
		fallo = fallos.pop()
		restantes--
		V(mutexFallos)
		nivel = fallo.getNivel()
		P[mutexFallosPorNivel[nivel]]
		fallosPorNivel[nivel]++
		V[mutexFallosPorNivel[nivel]]
		P(mutexFallos)
	}
}
```

### c)

```c title:Mi-Solución-con-array
array fallos[N] = // Se importan los datos
sem mutexFallos = 1
int restantes = N

int fallosPorNivel[4] = ([4] 0)

Process proceso::[id : 0..3] {
	Fallo fallo
	int ultimoFalloProcesado = -1
	bool listo = false
	
	P(mutexFallos)
	while (restantes > 0 && !listo) {
		// Recorro el vector de fallos hasta que se encuentra uno del nivel correspondiente
		for i = ultimoFalloProcesado+1..N-1 {
			if (fallos[i].getNivel() == id) {
				// Se procesa el fallo
				fallo = fallos[i]
				ultimoFalloProcesado = i
				restantes--
				V(mutexFallos)
				fallosPorNivel[i]++
				break;
			}
			// Si i es igual al ultimo indice y no se ejecutó el break entonces no hay mas fallos para este proceso
			if (i == N-1) {
				listo = true
			}
		}
		if (!listo) {
			P(mutexFallos)
		}
	}
}
```

```c title:Solución-Agusrnfr-con-cola
sem mutex = 1; int cant = 0; colaFallos c[N]; array contFallos[4] = ([4] 0);

process controlador[id:0..3]{
    Fallo fallo;
    int nivel;
    P(mutex)
    while (cant < N){
        fallo = c.pop();
        cant++;
        V(mutex);
        nivel = fallo.getNivel();
        if (nivel == id){
            if (nivel == 3){
                print(fallo.getID());
            }
            cantFallos[id]++;
        }
        else{
            P(mutex); cant--;
            c.push(fallo);
            V(mutex);
        }
        P(mutex);
    }
    V(mutex);
}
```

---

# Ejercicio 3

> Un sistema operativo mantiene 5 instancias de un recurso almacenadas en una cola. Además, existen P procesos que necesitan usar una instancia del recurso. Para eso, deben sacar la instancia de la cola antes de usarla. Una vez usada, la instancia debe ser encolada nuevamente para su reúso.

```c
Cola recursos[5]
sem mutexR = 5
sem mutexC = 1

Process proceso::[id : 1..P] {
	Recurso recurso
	while (true) {
		P(mutexR)
		P(mutexC)
		recurso = recursos.pop()
		V(mutexC)
		// Utilización del recurso
		P(mutexC)
		recursos.push(recurso)
		V(mutexC)
		V(mutexR)
	}
}

```

---
# Ejercicio 4
>  Suponga que existe una BD que puede ser accedida por 6 usuarios como máximo al mismo tiempo. Además, los usuarios se clasifican como usuarios de prioridad alta y usuarios de prioridad baja. Por último, la BD tiene la siguiente restricción: 
>    - no puede haber más de 4 usuarios con prioridad alta al mismo tiempo usando la BD. 
>    - no puede haber más de 5 usuarios con prioridad baja al mismo tiempo usando la BD. 
>    Indique si la solución presentada es la más adecuada. Justifique la respuesta.
>    ![[Pasted image 20240919162656.png]]

La solución presentada no es **la más** adecuada dado que al pasar primero por el semáforo "total" y luego por el "baja" o "alta" lo que se logra es que se bloqueen espacios de utilización de la base de datos sin certeza de que se pueda utlizar la misma inmediatamente, dado que puedo ocupar un cupo total y quedarme esperando a que se libere un cupo de mi prioridad demorando a un proceso de la otra prioridad que si pueda acceder inmediatamente a la BD porque sus cupos son suficientes.

---

# Ejercicio 5
>En una empresa de logística de paquetes existe una sala de contenedores donde se preparan las entregas. Cada contenedor puede almacenar un paquete y la sala cuenta con capacidad para N contenedores. Resuelva considerando las siguientes situaciones: 
>
>a) La empresa cuenta con 2 empleados: un empleado Preparador que se ocupa de preparar los paquetes y dejarlos en los contenedores; un empelado Entregador que se ocupa de tomar los paquetes de los contenedores y realizar la entregas. Tanto el Preparador como el Entregador trabajan de a un paquete por vez. 
>
>b) Modifique la solución a) para el caso en que haya P empleados Preparadores. 
>
>c) Modifique la solución a) para el caso en que haya E empleados Entregadores. 
>
>d) Modifique la solución a) para el caso en que haya P empleados Preparadores y E empleadores Entregadores.

### a)

```c
Contenedor contenedores[0..N-1]
int libre = 0, ocupado = 0
Sem vacios = N, llenos = 0

Process Preparador {
	Paquete paquete;
	while(true) {
		paquete = // Preparar paquete
		P(vacios)
		contenedores[libre] = paquete
		libre = (libre + 1) mod N
		V(llenos)
	}
}

Process Entregador {
	Paquete paquete;
	while (true) {
		P(llenos)
		paquete = contenedores[ocupado]
		ocupado = (ocupado + 1) mod n 
		V(vacios)
		// Entregar paquete
	}
}
```

### b)
```c
Contenedor contenedores[0..N-1]
int libre = 0, ocupado = 0
Sem vacios = N, llenos = 0, mutexP = 1

Process Preparador::[ id : 0..P-1] {
	Paquete paquete;
	while(true) {
		paquete = // Preparar paquete
		P(vacios)
		P(mutexP)
		contenedores[libre] = paquete
		libre = (libre + 1) mod N
		V(mutexP)
		V(llenos)
	}
}

Process Entregador {
	Paquete paquete;
	while (true) {
		P(llenos)
		paquete = contenedores[ocupado]
		ocupado = (ocupado + 1) mod n 
		V(vacios)
		// Entregar paquete
	}
}
```

c)
```c
Contenedor contenedores[0..N-1]
int libre = 0, ocupado = 0
Sem vacios = N, llenos = 0, mutexE = 1

Process Preparador {
	Paquete paquete;
	while(true) {
		paquete = // Preparar paquete
		P(vacios)
		contenedores[libre] = paquete
		libre = (libre + 1) mod N
		V(llenos)
	}
}

Process Entregador::[ id : 0..E-1] {
	Paquete paquete;
	while (true) {
		P(llenos)
		P(mutexE)
		paquete = contenedores[ocupado]
		ocupado = (ocupado + 1) mod n 
		V(mutexE)
		V(vacios)
		// Entregar paquete
	}
}
```

d)

```c
Contenedor contenedores[0..N-1]
int libre = 0, ocupado = 0
Sem vacios = N, llenos = 0, mutexE = 1, mutexP = 1

Process Preparador::[ id : 0..P-1] {
	Paquete paquete;
	while(true) {
		paquete = // Preparar paquete
		P(vacios)
		P(mutexP)
		contenedores[libre] = paquete
		libre = (libre + 1) mod N
		V(mutexP)
		V(llenos)
	}
}

Process Entregador::[ id : 0..E-1] {
	Paquete paquete;
	while (true) {
		P(llenos)
		P(mutexE)
		paquete = contenedores[ocupado]
		ocupado = (ocupado + 1) mod n 
		V(mutexE)
		V(vacios)
		// Entregar paquete
	}
}
```

---

# Ejercicio 6

>Existen N personas que deben imprimir un trabajo cada una. Resolver cada ítem usando semáforos: 

a)
> Implemente una solución suponiendo que existe una única impresora compartida por todas las personas, y las mismas la deben usar de a una persona a la vez, sin importar el orden. Existe una función Imprimir(documento) llamada por la persona que simula el uso de la impresora. Sólo se deben usar los procesos que representan a las Personas. 

```c
Sem mutex = 1;

Process persona::[0..N-1] {
	Documento documento = 
	while (true) {
		P(mutex)
		Imprimir(documento)
		V(mutex)
	}
}
```

b)
>Modifique la solución de (a) para el caso en que se deba respetar el orden de llegada. 
```c
Sem turnoPersona[0..N-1] = ([N] = 0), turnoPersona[0] = 1, mutex = 1
int turnoActual = 0, siguienteTurno = 0;


Process persona::[id : 0..N-1] {
	Documento documento;
	int miTurno;
	P(mutex)
	miTurno = siguienteTurno;
	siguienteTurno = (siguienteTurno + 1) Mod N;
	V(mutex)

	P(turnoPersona[miTurno]) // Espera a ser señalado para entrar

	Imprimir(documento)
	
	turnoActual = (turnoActual + 1) Mod N;
	V(turnoPersona[turnoActual])
}
```

c)
> Modifique la solución de (a) para el caso en que se deba respetar estrictamente el orden dado por el identificador del proceso (la persona X no puede usar la impresora hasta que no haya terminado de usarla la persona X-1). 
```c
Sem turnoPersona[0..N-1] = 0, turnoPersona[0] = 1

Process persona::[id : 0..N-1] {
	Documento documento = 
	P(turnoPersona[id])
	Imprimir(documento)
	V(turnoPersona[(id + 1) MOD N])
}
```

d)
> Modifique la solución de (b) para el caso en que además hay un proceso Coordinador que le indica a cada persona que es su turno de usar la impresora. 
```c
Sem turnoPersona[0..N-1] = ([N] = 0), mutex = 1, quierenImprimir = 0, termine = 0;
Cola c;

Process persona::[id : 0..N-1] {
	Documento documento;
	int miTurno;
	V(quierenImprimir) // Señaliza al coordinador que hay gente
	
	P(mutex)
	c.push(id) // Se agrega en la cola 
	V(mutex)
	P(turnoPersona[id]) // Espera a ser llamado
	Imprimir(documento) 
	V(termine) // Señaliza al coordinador que terminó
}

Process coordinador {
	int id;
	for (int i = 0; i < N; N++) {
		P(quierenImprimir)
		P(mutex)
		id = c.pop() 
		V(mutex)
		V(turnoPersona[id])
		P(termine)
	}
}
```

e)
> Modificar la solución (d) para el caso en que sean 5 impresoras. El coordinador le indica a la persona cuando puede usar una impresora, y cual debe usar.
```c
Sem turnoPersona[0..N-1] = ([N] = 0), mutex = 1, quierenImprimir = 0, termine = 0, 
Array impresoras[5] = {}
Cola c;

Process persona::[id : 0..N-1] {
	Documento documento;
	int miTurno;
	V(quierenImprimir) // Señaliza al coordinador que hay gente
	P(mutex)
	c.push(id) // Se agrega en la cola 
	V(mutex)
	P(turnoPersona[id]) // Espera a ser llamado
	Imprimir(documento) 
	V(termine) // Señaliza al coordinador que terminó
}

Process coordinador {
	int id;
	for (int i = 0; i < N; N++) {
		P(quierenImprimir)
		P(mutex)
		id = c.pop() 
		V(mutex)
		V(turnoPersona[id])
		P(termine)
	}
}
```

---
# Ejercicio 7
>Suponga que se tiene un curso con 50 alumnos. Cada alumno debe realizar una tarea y existen 10 enunciados posibles. Una vez que todos los alumnos eligieron su tarea, comienzan a realizarla. Cada vez que un alumno termina su tarea, le avisa al profesor y se queda esperando el puntaje del grupo (depende de todos aquellos que comparten el mismo enunciado). Cuando un grupo terminó, el profesor les otorga un puntaje que representa el orden en que se terminó esa tarea de las 10 posibles.
>
>Nota: Para elegir la tarea suponga que existe una función elegir que le asigna una tarea a un alumno (esta función asignará 10 tareas diferentes entre 50 alumnos, es decir, que 5 alumnos tendrán la tarea 1, otros 5 la tarea 2 y así sucesivamente para las 10 tareas).

50 Alumnos
10 Enunciados posibles
Se espera a que todos los alumnos elijan su tarea
Cuando un alumno termina su tarea le avisa al profesor
Cuando todos los alumnos de un mismo enunciado terminan, el profesor le asigna la nota correspondiente al orden en que realizaron la misma
Existe un método para elegir la tarea

```c
Sem todosConEnunciado = 0, mutex = 1, alumnoFinalizo = 0, enunciadoConPuntaje[10] = ([10] = 0);
Queue finalizaron;
int puntajePorEnunciado[10] = ([10] = 0), finalizadosPorEnunciado[10] = ([10] = 0);
int alumnosConEnunciado = 0;

Process Alumno::[id : 0..49] {
	enunciado = elegir();
	
	P(mutex); 
	alumnosConEnunciado += 1; 
	if (alumnosConEnunciado == 50) { V(todosConEnunciado) }
	V(mutex);
	
	P(todosConEnunciado)
	// Resolver enunciado

	P(mutex)
	finalizaron.push(enunciado)
	V(mutex)
	V(alumnoFinalizo)
	P(enunciadoConPuntaje[enunciado])
}

Process Profesor {
	int puntaje = 10;
	for (int i = 0; i < 50; i++) {
		P(alumnoFinalizo)
		
		P(mutex)
		enunciado = finalizaron.pop()
		V(mutex)

		finalizadosPorEnunciado[enunciado] -= 1

		if (finalizadosPorEnunciado[enunciado] == 0) {
			puntajePorEnunciado[enunciado] = puntaje;
			puntaje -= 1;
			for (int i = 0; i < 5; i++) {
				V(enunciadoConPuntaje[enunciado])
			}
		} 
	}
}


```

---
# Ejercicio 8
>Una fábrica de piezas metálicas debe producir T piezas por día. Para eso, cuenta con E empleados que se ocupan de producir las piezas de a una por vez. La fábrica empieza a producir una vez que todos los empleados llegaron. Mientras haya piezas por fabricar, los empleados tomarán una y la realizarán. Cada empleado puede tardar distinto tiempo en fabricar una pieza. Al finalizar el día, se debe conocer cual es el empleado que más piezas fabricó.
	 **a)** Implemente una solución asumiendo que T > E.
	 **b)** Implemente una solución que contemple cualquier valor de T y E.

```c
Sem mutexPiezasRestantes = 1, mutexContadorPorEmpleado[E] = ([E] = 0), llegaronTodos = 0, mutexEmpleadosQueLlegaron = 1, mutexListo = 1; mutexMaximaProduccion = 1;
int empleadosQueLlegaron = 0, piezasRestantes = T, maximaProduccion = 0, empleadoMaximaProduccion = -1;
bool listo = False;

Process empleado::[id : 0..E-1] {
	int piezasFabricadas = 0
	P(mutexEmpleadosQueLlegaron);
	empleadosQueLlegaron += 1;
	if (empleadosQueLlegaron == E) {
		for (int i = 0; i > E; i++) {
			V(llegaronTodos)
		}
	}
	V(mutexEmpleadosQueLlegaron)

	P(llegaronTodos)
	
	P(mutexListo);
	while (!listo) {
		// Producir pieza
		piezasFabricadas += 1
		P(mutexPiezasRestantes)
		piezasRestantes -= 1
		if (piezasRestantes == 0) {
				listo = True
		}
		V(mutexPiezasRestantes)
		V(mutexListo)
		P(mutexListo)
	}
	V(mutexListo)

	P(mutexMaximaProduccion)
		if (piezasFabricadas > maximoProducido) {
			maximaProduccion = piezasFabricadas;
			empleadoMaximaProduccion = id;
		}
	V(mutexMaximaProduccion)
}
```

---
# Ejercicio 9
> Resolver el funcionamiento en una fábrica de ventanas con 7 empleados (4 carpinteros, 1 vidriero y 2 armadores) que trabajan de la siguiente manera: 
> 
> • Los carpinteros continuamente hacen marcos (cada marco es armando por un único carpintero) y los deja en un depósito con capacidad de almacenar 30 marcos. 
> 
> • El vidriero continuamente hace vidrios y los deja en otro depósito con capacidad para 50 vidrios. 
> 
> • Los armadores continuamente toman un marco y un vidrio (en ese orden) de los depósitos correspondientes y arman la ventana (cada ventana es armada por un único armador).

```c
Marco marcos[30]; Vidrio vidrios[50];
sem mutexMarcos = 1, mutexVidrios = 1, llenoVidrios = 0, llenoMarcos = 0; vacioVidrios = 50, vacioMarcos = 30; 

Process carpintero::[id : 0..3] {
	while (true) {
		Marco marco = producirMarco();
		P(vacioMarcos)
		P(mutexMarcos)
		marcos.push(marco)
		V(mutexMarcos)
		V(llenoVidrios)
	}
}

Process vidriero {
	while (true) {
		Vidrio vidrio = producirVidrio();
		P(vacioVidrios)
		P(mutexVidrios)
		vidrios.push(vidrio)
		V(mutexVidrio)
		V(llenoVidrios)
	}
}

Process armador::[ id : 0..1] {
	Vidrio vidrio;
	Marco marco;
	while (true) {
		P(llenoMarcos)
		P(mutexMarcos)
		marco = marcos.pop()
		V(mutexMarcos)
		V(vacioMarcos)
		
		P(llenoVidrios)
		P(mutexVidrios)
		vidrio = vidrios.pop()
		V(mutexVidrios)
		V(vacioMarcos)

		// Arma la ventana
	}
}

```


---
# Ejercicio 10
> A una cerealera van T camiones a descargarse trigo y M camiones a descargar maíz. Sólo hay lugar para que 7 camiones a la vez descarguen, pero no pueden ser más de 5 del mismo tipo de cereal. 

> a) Implemente una solución que use un proceso extra que actúe como coordinador entre los camiones. El coordinador debe atender a los camiones según el orden de llegada. Además, debe retirarse cuando todos los camiones han descargado. 

```c
Sem cuposCamiones = 7, cuposTrigo = 5, cuposMaiz = 5, mutexCola = 1, mutexCamionesDescargados = 1 turnos[0..T+M-1] = 0, camionesLlegaron = 0;
Queue llegada;
int camionesDescargados = 0;

Process camionTrigo::[ id : 0..T-1 ] {
	P(mutexCola); 
	llegada.push(id);
	V(camionesLlegaron) 
	V(mutexCola);

	P(cuposTrigo)
	P(cuposCamiones)
	P(turnos[id])

	// Descarga cereal

	V(cuposCamiones)
	V(cuposTrigo)

	P(mutexCamionesDescargados)
	camionesDescargados += 1
	V(mutexCamionesDescargados)
}

Process camionMaiz::[ id : T..T+M-1 ] {
	P(mutexCola); 
	llegada.push(id); 
	V(camionesLlegaron) 
	V(mutexCola);

	P(cuposMaiz)
	P(cuposCamiones)
	P(turnos[id])

	// Descarga cereal

	V(cuposCamiones)
	V(cuposMaiz)

	P(mutexCamionesDescargados)
	camionesDescargados += 1
	V(mutexCamionesDescargados)
}

Process coordinador {
	int id;
	for (int i = 0; i < T + M; i++) {
		P(camionesLlegaron)
		P(mutexCola)
		id = llegada.pop()
		V(mutexCola)
		V(turnos[id]) 
	}
}

```

> b) Implemente una solución que no use procesos adicionales (sólo camiones). No importa el orden de llegada para descargar. Nota: maximice la concurrencia.

```c
Sem cuposCamiones = 7, cuposTrigo = 5, cuposMaiz = 5;

Process camionTrigo::[ id : 0..T-1 ] {
	P(cuposTrigo)
	P(cuposCamiones)
	// Descarga cereal
	V(cuposCamiones)
	V(cuposTrigo)
}

Process camionMaiz::[ id : 0..M-1 ] {
	P(cuposMaiz)
	P(cuposCamiones)
	// Descarga cereal
	V(cuposCamiones)
	V(cuposMaiz)
}
```

---
# Ejercicio 11
> En un vacunatorio hay un empleado de salud para vacunar a 50 personas. El empleado de salud atiende a las personas de acuerdo con el orden de llegada y de a 5 personas a la vez. Es decir, que cuando está libre debe esperar a que haya al menos 5 personas esperando, luego vacuna a las 5 primeras personas, y al terminar las deja ir para esperar por otras 5. Cuando ha atendido a las 50 personas el empleado de salud se retira. Nota: todos los procesos deben terminar su ejecución; suponga que el empleado tienen una función VacunarPersona() que simula que el empleado está vacunando a UNA persona.

```c
Cola cola;
Sem mutexCola = 1, mutexContador = 1, hayCinco = 0, vacunado[50] = ([50] = 0);
int contadorTirada = 0;

Process empleado {
	Array grupoActual[5];
	for (int i = 0; i < 10, i++) {
		P(hayCinco)
		for (int j = 0; i < 5; i++) { // Entran todos juntos
			P(mutexCola)
			id = cola.pop()
			grupoActual[j] = id
			V(mutexCola)
		}
		for (int k = 0; i < 5; i++) { // Se vacunan todos juntos
			vacunarPersona()
		}
		for (int l = 0; i < 5; i++) { // Se van todos juntos
			V(vacunado[l])
		}
	}
}

// El enunciado no aclara de que manera tratar al grupo de 5 personas asi que yo las trato como grupo, hacen todo juntos.

Process persona::[id : 0..49] {
	P(mutexCola)
	cola.push(id)
	V(mutexCola)
	P(mutexContador)
	contadorTirada += 1;
	if (contadorTirada == 5) {
		V(hayCinco)
		contadorTirada = 0
	}
	V(mutexContador)
	P(vacunado[id])

}
```

---
# Ejercicio 12
> Simular la atención en una Terminal de Micros que posee 3 puestos para hisopar a 150 pasajeros. En cada puesto hay una Enfermera que atiende a los pasajeros de acuerdo con el orden de llegada al mismo. Cuando llega un pasajero se dirige al Recepcionista, quien le indica qué puesto es el que tiene menos gente esperando. Luego se dirige al puesto y espera a que la enfermera correspondiente lo llame para hisoparlo. Finalmente, se retira. 
> b) Modifique la solución anterior para que sólo haya procesos Pasajeros y Enfermera, siendo los pasajeros quienes determinan por su cuenta qué puesto tiene menos personas esperando. Nota: suponga que existe una función Hisopar() que simula la atención del pasajero por parte de la enfermera correspondiente.

> a) Implemente una solución considerando los procesos Pasajeros, Enfermera y Recepcionista.
```c
Queue puestoUno, puestoDos, puestoTres, colaRecepcionista;
Sem mutexUno = 1, mutexDos = 1, mutexTres = 1, mutexRecepcionista = 1, hayGenteRecepcionista = 0;
int cantUno = 0, cantDos = 0, cantTres = 0; 

Process pasajero::[ id : 0..149 ] {
	P(mutexRecepcionista)
	colaRecepcionista.push(id)
	V(hayGenteRecepcionista)
	V(mutexRecepcionista)

	
}

Process enfermera::[ id : 1..3 ] {
	
}

Process recepcionista {
	int id;
	while (true) {
		P(hayGenteRecepcionista)
		P(mutexRecepcionista)
		id = colaRecepcionista.pop()
		V(mutexRecepcionista)
		if (cantUno <= cantDos && cantUno <= cantTres) {
			P(mutexUno)
			puestoUno.push(id)
			V(mutexUno)
		} else if (cantDos <= cantUno && cantDos <= cantTres){
			P(mutexDos)
			puestoDos.push(id)
			V(mutexDos)
		} else {
			P(mutexTres)
			puestoTres.push(id)
			V(mutexTres)
		}	
	}
}
```