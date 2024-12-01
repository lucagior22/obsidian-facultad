# PMA
# 1

> Suponga que N clientes llegan a la cola de un banco y que serán atendidos por sus empleados. Analice el problema y defina qué procesos, recursos y canales/comunicaciones serán necesarios/convenientes para resolverlo. Luego, resuelva considerando las siguientes situaciones: 
> a. Existe un único empleado, el cual atiende por orden de llegada. 

```c
chan banco (int, texto);
chan respuesta[0..N](texto);

process clientes[id:0..N-1]{
	texto consulta, resolucion;
	send banco(consulta);
	receive respuesta[id](resolucion);
}

process empleado{
	texto consulta, resolucion;
	int id_cliente;
	while (true) {
		receive banco(id_cliente, consulta);
		resolucion = atender(consulta);
		send respuesta[id_cliente](resolucion); 
	}	
}
```

> b. Ídem a) pero considerando que hay 2 empleados para atender, ¿qué debe modificarse en la solución anterior? 

*La solución es la misma, dado que las sentencias **send y receive** son atómicas y que los receive retiran desde el otro extremo de la cola donde los send anexan sus textos.*

```c
chan banco (int,texto);
chan respuesta[0..N-1](texto)

process clientes[id:0..N-1]{
	texto consulta, resolucion;
	send banco(consulta);
	receive respuesta[id](resolucion);
}

process empleado[id:1..2]{
	texto consulta, resolucion;
	int id_cliente;
	while (true) {
		receive banco(id_cliente, consulta);
		resolucion = atender(consulta);
		send respuesta[id_cliente](resolucion); 
	}	
}
```

> c. Ídem b) pero considerando que, si no hay clientes para atender, los empleados realizan tareas administrativas durante 15 minutos. ¿Se puede resolver sin usar procesos adicionales? ¿Qué consecuencias implicaría?

*Esta solución genera demora innecesaria y podria hacer que un empleado no termine su ejecución de la siguiente manera:*
- El canal tiene un último mensaje.
- Ambos empleados evaluan la condición del ``if not empty(banco)``.
- Emplado 1 gana el mensaje mientras que empleado 2 va a esperar a que llegue otro mensaje al canal que nunca va a pasar porque los clientes terminaron su tarea.
```c
chan banco(int,texto);
chan respuesta[0..N-1](texto);
process clientes[id:0..N-1]{
	texto consulta;resolucion;
	send banco(consulta);
	recieve respuesta[id](resolucion);
}

process empleado[1..2]{
	texto consulta; resolucion;
	while true{
		if(not empty(banco)){
			recieve banco(id_cliente,consulta)
			resolucion = atender(consulta)
			send respuesta[id_cliente](resolucion) 		
		}else{
			delay(15 minutos)
		}
	}
}
```

*Esta solución con proceso coordinador es más correcta ya que los empleados no pueden quedarse trabados como en la anterior.*
```c
chan banco(int,texto);
chan respuesta[0..N-1](texto);
chan pedido(int)
chan siguiente[1..2](int,texto)
process clientes[id:1..N-1]{
	texto consulta; resolucion;
	send banco(consulta)
	recieve respuesta[id](resolucion)

}

process empleado [id:1..2]{
	texto consulta; resolucion
	int id_cli;
	while true{
		send pedido(id)
		recieve siguiente[id](id_e,consulta)
		if(id_e<>-1){
			resolucion = atender(consulta)
			send respuesta[id_E](resolucion)
		}else{
			wait(15 minutos)
		}
	}
}

process coordinador{
	texto r;
	int id_E;id_cli;
	while (true){
		recieve pedido(id_E)
		if(not empty(banco)){
			recieve banco(id_e,r)
		}else{
			id_e=-1
			r=null
		}
		send siguiente[id_e](id_e,r)
	}
}
```
---
# 2
> Se desea modelar el funcionamiento de un banco en el cual existen 5 cajas para realizar pagos. Existen P clientes que desean hacer un pago. Para esto, cada una selecciona la caja donde hay menos personas esperando; una vez seleccionada, espera a ser atendido. En cada caja, los clientes son atendidos por orden de llegada por los cajeros. Luego del pago, se les entrega un comprobante. Nota: maximizar la concurrencia.

*Notas consulta:*
- 1 Banco.
- 5 Caja.
	- Se debe conocer cuantas personas tiene cada caja.
		- **Solución:** Proceso administrador.
			- Recibe un mensaje de tipo *pedido*, que indica el tipo del mensaje enviado junto con el id del proceso.
				- Recibe un mensaje de *llegada* de los clientes.
				- Recibe un mensaje de *salida* de las cajas.
			- Lleva el contador de clientes por cola.
			- Asigna a cada cliente la caja con el menor numero de clientes.
- P Cliente.
- ¿Cómo se cuando una cola decrementó la cantidad de gente en la cola?
	- Cajas avisan a los clientes y a los administradores que se terminó la transacción.
		- Además, envian un mensaje al canal de *pedido*, con tipo de pedido *salida* y el id de la caja.

*Esta solución genera **Busy waiting** lo que, de ser evitable, debe ser evitado.*
```c
chan banco[0..4](int,text); respuesta[0..P-1](int,text)
chan llegue (int); respondi[1..4]()
chan menos_concurrido[0..P-1] (int);
process administrador{
	int cant_fila[1..4]= ([N],0)	
	int id_C
	while True{
		id_min = min(cant_fila)
		recieve llegue(id_c)
		cant_fila[id_min]++
		send menos_concurrido[id_c](id_min)
		if not respondi[0].empty -> recieve respondi[0] (id);cant_fila[id]--;
		* not respondi[1].empty -> recieve respondi[1] (id);cant_fila[id]--;
		* not respondi[2].empty -> recieve respondi[2] (id);cant_fila[id]--;
		* not respondi[3].empty -> recieve respondi[3] (id);cant_fila[id]--;
		* not respondi[4].empty -> recieve respondi[4] (id);cant_fila[id]--;
	}
}

process clientes[id:0..P-1]{
	text pago; constancia
	int id_menos_concurrido
	send llegue (id)
	recieve menos_concurrido[id](id_menos_concurrido)
	send banco[menos_concurrido](id,pago)
	recieve respuesta[id](constancia)	
}

process Caja[id:0..4]{
	text pago; constancia;
	int id_e;
	while (true){
		recieve banco[id](id_e,pago)
		constancia = realizar_pago(pago)
		send respuesta[id_e](constancia)
		send respondi[id](id)
	}						
}
```

*Esta solución es mas apropiada, ya que se previene el busy waiting, durmiendo al proceso administrador en la cola pedidos hasta que se reciba uno.*
```c
chan banco[0..4](int,text); respuesta[0..P-1](int,text)
chan llegue (int); respondi[1..4]()
chan menos_concurrido[0..P-1] (int);
chan pedido(int,int)
process administrador{
	int cant_fila[1..4]= ([N],0)	
	int id_C;pedido, id_pedido
	while True{
		recieve pedido(tipo,id_pedido)
		if (pedido == 1){
			id_min = min(cant_fila)
			recieve llegue(id_c)
			cant_fila[id_min]++
			send menos_concurrido[id_c](id_min)
		}else{
			recieve respondi[id_pedido]()
			cant_fila[id_pedido]--

		}		
	}
}
process clientes[id:0..P-1]{
	text pago; constancia
	int id_menos_concurrido
	send llegue (id)
	send pedido(1,id)
	recieve menos_concurrido[id](id_menos_concurrido)
	send banco[menos_concurrido](id,pago)
	recieve respuesta[id](constancia)	
}

process Caja[id:0..4]{
	text pago; constancia;
	int id_e;
	while (true){
		recieve banco[id](id_e,pago)
		constancia = realizar_pago(pago)
		send respuesta[id_e](constancia)
		send respondi[id](id)
		send pedido(2,id)
	}						
}

```

---
# 3

> Se debe modelar el funcionamiento de una casa de comida rápida, en la cual trabajan 2 cocineros y 3 vendedores, y que debe atender a C clientes. El modelado debe considerar que: - Cada cliente realiza un pedido y luego espera a que se lo entreguen. - Los pedidos que hacen los clientes son tomados por cualquiera de los vendedores y se lo pasan a los cocineros para que realicen el plato. Cuando no hay pedidos para atender, los vendedores aprovechan para reponer un pack de bebidas de la heladera (tardan entre 1 y 3 minutos para hacer esto). - Repetidamente cada cocinero toma un pedido pendiente dejado por los vendedores, lo cocina y se lo entrega directamente al cliente correspondiente. Nota: maximizar la concurrencia

```c
chan realizar_pedido(texto,int)
chan respuesta_pedido(texto,id);pedido_cocina(texto,id)
chan solicitar_pedido(int);
chan entrega_pedido[1..C] (comida);

process coordinador{
	int id_vend
	int id_cliente;
	texto pedido;
	while true(){
		recieve solicitar_pedido(id_vend)
		if (relizar_pedido.empty()){
			id_cliente = -1
			pedido = null
			send respuesta_pedido[id_vend](pedido,id_cliente)
		}else{
			recieve realizar_pedido(pedido,id_cliente)
			send respuesta_pedido[id_ven](pedido,id_cliente)
		}		
	
	}
}
process	cocinero [1..2]{
	int id_cliente
	while true{
		recieve pedido_cocina (pedido,id_cliente)
		comida = cocinarPedido(pedido)	
		entregar_pedido[id_cliente](comida)
	}
}

process vendedor [1..3]{
	int id_cliente;
	while true{
		send solicitar_pedido(id)
		recieve respuesta_pedido (pedido,id_cliente)
		if(id_cliente<>-1){
			send pedido_cocina(pedido,id_cliente)
		}else{
			reponerHeladera();
		}	
	}
}
process clientes [1..C]{
	texto pedido;
	comida com;
	send realizar_pedido (pedido,id)
	recive entrega_pedido[id](comida);

}

```

---
# 4
> Simular la atención en un locutorio con 10 cabinas telefónicas, el cual tiene un empleado que se encarga de atender a N clientes. Al llegar, cada cliente espera hasta que el empleado le indique a qué cabina ir, la usa y luego se dirige al empleado para pagarle. El empleado atiende a los clientes en el orden en que hacen los pedidos. A cada cliente se le entrega un ticket factura por la operación. 
> a) Implemente una solución para el problema descrito. 

*Consultas:*
- ¿Cuando hago el canal de Pedido y cuando el if de varios canales?
	- Distinta prioridad de canales.
		- Sin prioridad: Canal de pedido.
	- Busy waiting
		- If puede generar busy waiting.

*Solución con busy waiting aceptable*
```c
chan acceder_cabina(int)
chan mi_cabina[0..N-1](int)
chan pagar_cabina(int,int)
chan recibir_recibo(texto)
process cliente[id:0..N-1]{
	texto recibo
	send acceder_cabina(id)
	recieve mi_cabina[id](id_cabina)
	acceder_a_cabina(id_cabina)
	send pagar_cabina(id_cabina,id)
	recieve recibir_recibo[id](recibo)
}

process empleado{
	boolean cabina_dis[i..10] = ([n] = true);
	int id_cliente;id_cabina;
	while(true]{
		if (not acceder_cabina.empty() and buscar_cabina_dis(cabina_dis)<>-1){
			id_cabina = buscar_cabina_dis(cabina_dis)
			cabina_dis[id_cabina]=false
			recieve acceder_cabina(id_cliente,id_cabina) 
			send mi_cabina[id_cliente](buscar_cabina_dis(cabina_dis))
		}	   
		* (not pagar_cabina.empty()){
			recieve pagar_cabina(id_cabina,id_cliente)
			cabina_dis[id_cabina] = true
			texto recibo=Cobrar()
			send recibir_recibo[id_cliente](recibo)
		}
	}
}

```

*Sin busy waiting y mejor*
```c
chan acceder_cabina(int)
chan mi_cabina[0..N-1](int)
chan pagar_cabina(int,int)
chan recibir_recibo(texto)
chan pedidos(int)
process cliente[id:0..N-1]{
	texto recibo
	send acceder_cabina(id)
	send pedido(1)
	recieve mi_cabina[id](id_cabina)
	acceder_a_cabina(id_cabina)
	send pagar_cabina(id_cabina,id)
	send pedido(2)
	recieve recibir_recibo[id](recibo)
}

process empleado{
	int cabina_dis[i..10] = ([n] = -);
	int id_cliente;id_cabina;
	while(true]{
		receive pedido(tipo)
		if (tipo == 1){
			id_cabina = buscar_cabina_dis(cabina_dis)
			if(id_cabina == -1){
				recieve pagar_cabina(id_cabina,id_cliente)
				cabina_dis[id_cabina] = true
				texto recibo=Cobrar()
				send recibir_recibo[id_cliente](recibo)
			}
			cabina_dis[id_cabina]=false
			recieve acceder_cabina(id_cliente,id_cabina) 
			send mi_cabina[id_cliente](buscar_cabina_dis(cabina_dis))
		}	   
		else (tipo == 2 && !pagar_cabina.empty){
			recieve pagar_cabina(id_cabina,id_cliente)
			cabina_dis[id_cabina] = true
			texto recibo=Cobrar()
			send recibir_recibo[id_cliente](recibo)
		}
	}
}
```

> b) Modifique la solución implementada para que el empleado dé prioridad a los que terminaron de usar la cabina sobre los que están esperando para usarla. Nota: maximizar la concurrencia; suponga que hay una función Cobrar() llamada por el empleado que simula que el empleado le cobra al cliente.
```c
chan acceder_cabina(int)
chan mi_cabina[0..N-1](int)
chan pagar_cabina(int,int)
chan recibir_recibo(texto)
chan pedidos(int)
process cliente[id:0..N-1]{
	texto recibo
	send acceder_cabina(id)
	send pedido(1)
	recieve mi_cabina[id](id_cabina)
	acceder_a_cabina(id_cabina)
	send pagar_cabina(id_cabina,id)
	send pedido(2)
	recieve recibir_recibo[id](recibo)
}

process empleado{
	int cabina_dis[i..10] = ([n] = -);
	int id_cliente;id_cabina;
	while(true]{
		receive pedido()
		if (!pagar_cabina.empty){
			recieve pagar_cabina(id_cabina,id_cliente)
			cabina_dis[id_cabina] = true
			texto recibo=Cobrar()
			send recibir_recibo[id_cliente](recibo)
		}	   
		else {
			id_cabina = buscar_cabina_dis(cabina_dis)
			if(id_cabina == -1){
				recieve pagar_cabina(id_cabina,id_cliente)
				receive pedido()
				cabina_dis[id_cabina] = true
				texto recibo=Cobrar()
				send recibir_recibo[id_cliente](recibo)
			}
			cabina_dis[id_cabina]=false
			recieve acceder_cabina(id_cliente,id_cabina) 
			send mi_cabina[id_cliente](buscar_cabina_dis(cabina_dis))
			
		}
	}
}
```

*Resolución de repaso dia anterior al examen*
```c
chan usoCabina(int), finCabina(int, int), cabinaAsignada[1..N](int), ticket[1..N](Ticket), hayPedido(int)

Process Cliente::[id : 1..N] {
	Ticket ticket;
	int idCabina;
	send usoCabina(id);
	receive cabinaAsignada[idCabina];
	UsarCabina();
	send finCabina(idCabina, id);
	receive ticket[id](ticket);
}

Process Empleado {
	Cola cabinasLibres;
	Int idCabina, idCliente;
	Ticket ticket;
	for (int i = 1; i <= 10; i++) {
		cabinasLibres.push(i);
	}
	while (True) {
		receive hayPedido();
		if (!empty(finCabina)) {
			receive finCabina(idCabina, idCliente)
			cabinasLibres.push(idCabina)
			ticket = Cobrar(idCliente)
			send ticket[id](ticket)
		} else {
			receive usoCabina(idCliente)
			if (empty(cabinasLibres)) {
				receive hayPedido()
				receive finCabina(idCabina, idCliente)
				cabinasLibres.push(idCabina)
				ticket = Cobrar(idCliente)
				send ticket[id](ticket)
			}
			send cabinaAsignada[idCliente](cabinasLibres.push())	
		}
	}
}
```

---
# 5
> Resolver la administración de 3 impresoras de una oficina. Las impresoras son usadas por N administrativos, los cuales están continuamente trabajando y cada tanto envían documentos a imprimir. Cada impresora, cuando está libre, toma un documento y lo imprime, de acuerdo con el orden de llegada. 
> a) Implemente una solución para el problema descrito. 
> **Nota: ni los administrativos ni el director deben esperar a que se imprima el documento.**

*Notas de consulta:*
- Se describe que es lo que hace la impresora, es un proceso.
- Si mas de un proceso tiene que recibir de un canal entonces se podría agregar un coordinador.
- a)
	- Personas envian a un canal común.
		- Se ordenan por como funciona el canal.
- b)
	- Existe un coordinador.
		- Espera que haya una impresora libre.
		- Espera que alguien quiera imprimir, canal *pedidos*.
		- Lee un canal de pedidos.
			- Si el canal de pedidos de director no está vacio, atiende ese pedido.
			- Sino atiende a los administrativos.
		- Administrativos y director envian sus documentos a imprimir a sus colas respectivas.

```c
chan comunicación (texto)
process administrativo[id:0..N-1]{
	texto documento;
	send comunicación(documento)
}

process impresora [id:1..3]{
	texto documento;
	while true{
		recieve comunicación(documento)
		imprimir (documento)
	}	
}

```

> b) Modifique la solución implementada para que considere la presencia de un director de oficina que también usa las impresas, el cual tiene prioridad sobre los administrativos. 
```c
chan comunicación (texto); quiero_recibir(); mando_pedido()
chan comunicación_prio(texto); cola_impresion(texto)
process director{
	texto documento_prioritario
	send comunicación_prio(documento_prioritario)	
	send mando_pedido()
}
process coordinador{
	
	while true{
		recieve quiero_recibir( )
		recieve mando_pedido()
		if(not comunicacion_prio.empty() ){
			recieve comunicacion_prio(documento_prioritario)
			send cola_impresion(documento_prioritario)
		}else{
			recieve comunicacion(documento)
			send cola_impresion(documento)
		}
	}
}
process administrativo[id:0..N-1]{
	texto documento;
	send comunicación(documento)
	send mando_pedido()
}

process impresora [id:1..3]{
	texto documento;
	while true{
		send quiero_recibir(id) 
		recieve comunicación(documento)
		imprimir (documento)
	}	
}
```

> c) Modifique la solución (a) considerando que cada administrativo imprime 10 trabajos y que todos los procesos deben terminar su ejecución. 
```c
chan comunicación_a (texto)
chan comunicación_i (texto)

process coordinador{
	int cant_rest = N*10
	while Cant_rest >0 {
		recieve comunicación_a (documento)
		send comunicación_i (documento)
		cant_rest--;
	}
	
	for[1..3] -> send comunicacion_i(null)
}

process administrativo[id:0..N-1]{
	texto documento;
	for [i 1..10]{
		documento = generarDocumento()
		send comunicación_a(documento)
	}
}

process impresora [id:1..3]{
	texto documento = "";
	while documento <> null {
		recieve comunicación_(documento)
		if (documento <> null){
			imprimir (documento)
		}
	}	
}
```

> d) Modifique la solución (b) considerando que tanto el director como cada administrativo imprimen 10 trabajos y que todos los procesos deben terminar su ejecución. 
> e) Si la solución al ítem d) implica realizar Busy Waiting, modifíquela para evitarlo. 


# PMS

# 1 
> Suponga que existe un antivirus distribuido que se compone de R procesos robots Examinadores y 1 proceso Analizador. Los procesos Examinadores están buscando continuamente posibles sitios web infectados; cada vez que encuentran uno avisan la dirección y luego continúan buscando. El proceso Analizador se encarga de hacer todas las pruebas necesarias con cada uno de los sitios encontrados por los robots para determinar si están o no infectados. 
> 
> a) Analice el problema y defina qué procesos, recursos y comunicaciones serán necesarios/convenientes para resolverlo. 
> b) Implemente una solución con PMS sin tener en cuenta el orden de los pedidos. 

*Solución básica, puede ser menos concurrente que la siguiente. No respeta el orden*
```c
process Analizador {
	Direccion direccion;
	while (true) {
		Examinador[*]?direccion(direccion)
		analizar(direccion)
	}
}

process Examinador::[0..R-1] {
	Direccion direccion;
	while (true) {
		direccion = buscarSitio()
		Analizador!direccion(direccion)
	}
}
```

> c) Modifique el inciso (b) para que el Analizador resuelva los pedidos en el orden en que se hicieron.

*Solución mas concurrente y respetando el orden.*
```c
process Analizador {
    while true {
        Admin!pedido()
        Admin?respuesta(direccion)
        analizar(direccion)
    }
}

process Examinador[id:1..N] {
	Direccion direccion;
    while true {
        direccion = buscarSitio() //busca un sitio web
        Admin!resultado(direccion)
    }
}

process Admin {
    Cola direcciones;
    Direccion direccion;
    
    do Examinador[*]?resultado(direccion) -> direcciones.push(direccion)
    [] !empty(direcciones); Analizador?pedido() -> Analizador!respuesta(direcciones.pop())
    od 
}
```

# 2
> En un laboratorio de genética veterinaria hay 3 empleados. 
> 
> El primero de ellos continuamente prepara las muestras de ADN; cada vez que termina, se la envía al segundo empleado y vuelve a su trabajo. 
> 
> El segundo empleado toma cada muestra de ADN preparada, arma el set de análisis que se deben realizar con ella y espera el resultado para archivarlo. 
> 
> Por último, el tercer empleado se encarga de realizar el análisis y devolverle el resultado al segundo empleado.

```c
process EmpleadoUno {
	while (True) {
		muestra = prepararMuestra()
		Admin!envioMuestra()
	}
}

process EmpleadoDos {
	while (True) {
		Admin!esperoMuestra()
		Admin?reciboMuestra(muestra)
		set = armarSet(muestra)
		EmpleadoTres!envioSet(set)
		EmpleadoTres?reciboResultado(resultado)
		archivar(resultado)
	}
}

process EmpleadoTres {
	while(true) {
		EmpleadoDos?envioSet(set)
		resultado = hacerAnalisis(set)
		EmpleadoDos!reciboResultado(resultado)
	}
}

process Admin {
	Cola muestras
	do EmpleadoUno?envioMuestra(muestra) -> muestras.push(muestra);
	[] !empty(muestras); EmpleadoDos?esperoMuestra() -> { 
		aux = muestras.pop(); 
		EmpleadoDos!reciboMuestra(aux) 
		}
}
```

# 3
> En un examen final hay N alumnos y P profesores. Cada alumno resuelve su examen, lo entrega y espera a que alguno de los profesores lo corrija y le indique la nota. Los profesores corrigen los exámenes respetando el orden en que los alumnos van entregando. 
> a) Considerando que P=1. 
```c
process Alumno::[id: 1..N] {
	examen = realizarExamen()
	Profesor!entregaExamen(examen, id)
	Profesor?entregaCorreccion(correccion)
}

process Profesor {
	Boolean fin = False;
	for (int i = 1; i < N; i++) {
		if Alumno[*]?entregaExamen(examen, idAlumno) -> {
			correcion = corregirExamen(examen)
			Alumno[idAlumno]!entregaCorreccion(correccion)
			}
		fi
	}
}
```
*Consultas: ¿Se genera demora innecesaria teniendo en cuenta que los alumnos igualmente tienen que esperar la corrección?*

> b) Considerando que P>1. 
```c
process Alumno::[id: 1..N] {
	examen = realizarExamen()
	Admin!entregaExamen(examen, id)
	Profesor[*]?entregaCorreccion(correccion)
}

process Profesor::[id : 1..P] {
	Boolean fin = False;
	while (!fin) {
		Admin!profesorLibre(id)
		if Admin?entregaExamen(examen, idAlumno) -> {
			correccion = corregirExamen(examen)
			Alumno[idAlumno]!entregaCorreccion(correccion)
			}
		* Admin?finCorrecciones() -> fin = True
		fi
	}
}

process Admin {
	Cola entregas;
	Int entregasRestantes = N;

	while (entregasRestantes > 0) {
		if Alumno[*]?entregaExamen(examen, idAlumno) -> {
			entregas.push((examen, idAlumno))
			}
		* !entregas.empty(); Profesor[*]?profesorLibre(idProfesor) -> {
				(examen, idAlumno) = entregas.pop()
				Profesor[idProfesor]!entregaExamen(examen, idAlumno)
				entregasRestantes--
			}
		fi
	}
	
	for (int i = 1; i <= P; i++) {
		Profesor[i]!finCorrecciones()
	}
}
```
*Consultas: ¿Se puede enviar un mensaje vacio con fines de sincronización? ¿Sería mejor que el profesor avise al admin que terminó de corregir en vez de suponer que lo hizo cuando se le entregó el examen?*

> c) Ídem b) pero considerando que los alumnos no comienzan a realizar su examen hasta que todos hayan llegado al aula. Nota: maximizar la concurrencia; no generar demora innecesaria; todos los procesos deben terminar su ejecución
```c
process Alumno::[id: 1..N] {
	Admin!llegadaAlumno()
	Admin?comienzoExamen()
	examen = realizarExamen()
	Admin!entregaExamen(examen, id)
	Profesor[*]?entregaCorreccion(correccion)
}

process Profesor::[id : 1..P] {
	Boolean fin = False;
	while (!fin) {
		Admin!profesorLibre(id)
		if Admin?entregaExamen(examen, idAlumno) -> {
			correccion = corregirExamen(examen)
			Alumno[idAlumno]!entregaCorreccion(correccion)
			}
		* Admin?finCorrecciones() -> fin = True
		fi
	}
}

process Admin {
	Cola entregas;
	Int entregasRestantes = N;
	Int alumnosPresentes = 0;

	// Este do podria cambiarse por un for de 1 a N
	do !(alumnosPresentes == N); Alumno{*]?llegadaAlumno() -> {
		alumnosPresentes++
		}
	od
		
	for (int k = 1; k < N; k++) {
		Alumno[k]!comienzoExamen()
	}

	while (entregasRestantes > 0) {
		if Alumno{*]?entregaExamen (examen, idAlumno) -> {
			entregas.push((examen, idAlumno))
			}
		* !entregas.empty(); Profesor{*]?profesorLibre(idProfesor) -> {
				(examen, idAlumno) = entregas.pop()
				Profesor[idProfesor]!entregaExamen(examen, idAlumno)
				entregasRestantes--
			}
		fi
	}
	
	for (int i = 1; i <= P; i++) {
		Profesor[i]!finCorrecciones()
	}
}
```

> En un examen final hay N alumnos y P profesores. Cada alumno resuelve su examen, lo entrega y espera a que alguno de los profesores lo corrija y le indique la nota. Los profesores corrigen los exámenes respetando el orden en que los alumnos van entregando. Considerando que P>1. Los alumnos no comienzan a realizar su examen hasta que todos hayan llegado al aula. Nota: maximizar la concurrencia; no generar demora innecesaria; todos los procesos deben terminar su ejecución
```c
Process Profesor::[id : 1..P] {
	bool fin = false;
	int idAlumno, nota;
	text examen;
	
	while (!fin) {
		Admin!profesorLibre(id)
		if Admin?entregaExamen(idAlumno, examen) -> {
				nota = corregir(examen)
				Alumno[idAlumno]!entregaCorreccion(nota)
			}
		* Admin?finCorrecciones -> {
				 fin = true
			}
	}
}

Process Alumno::[id : 1..N] {
	text examen;
	int nota;
	Admin!llegadaAlumno()
	Admin!comenzar()
	examen = hacerExamen()
	Admin!entregaExamen(id, examen)
	Profesor[*]?entregaCorreccion(nota)
}

Process Admin {
	Cola entregas;
	int entregasRestantes = N;
	
	for i in 1..N {
		Alumno[*]?llegadaAlumno()
	}
	for i in 1..N {
		Alumno[*]?comenzar()
	}
	while (entregasRestantes < 0) {
		if Alumno[*]?entregaExamen(idAlumno, examen) -> {
				entregas.push((idAlumno, examen));
			}
		* (!empty(entregas)); Profesor[*]?profesorLibre(idProfesor) -> {
				 Profesor[idProfesor]!engregaExamen(entregas.pop());
				 entregasRestantes--;
			}
	}
	
	for i in 1..P {
		Profesor[i]!finCorrecciones;	
	}
}
```

# 4
> En una exposición aeronáutica hay un simulador de vuelo (que debe ser usado con exclusión mutua) y un empleado encargado de administrar su uso. Hay P personas que esperan a que el empleado lo deje acceder al simulador, lo usa por un rato y se retira. Nota: cada persona usa sólo una vez el simulador.
> a) Implemente una solución donde el empleado sólo se ocupa de garantizar la exclusión mutua. 

```c
process Empleado {
	int id
	While (true) {
		Persona[*]?pedirSimulador(id)
		Persona[id]!pasar()
		Persona[id]?dejarSimulador()
	}
}

process Persona::[id : 1..P] {
	Empleado!pedirSimulador(id)
	Empleado?pasar()
	delay(500) // Se usa el simulador
	Empleado!dejarSimulador()
}
```

> b) Modifique la solución anterior para que el empleado considere el orden de llegada para dar acceso al simulador. 
```c
process Admin {
	Cola cola;
	for (int i = 1; i <= P; i++) {
		if Persona[*]?pedirSimulador(id) -> {
			cola.push(id)
			}
		!(cola.empty()); Empleado?empleadoLibre() -> {
			id = cola.pop()
			Empleado!quienSigue(id)
			}
		fi
	}
}


process Empleado {
	int id
	for (int i = 1; i <= P; i++) {
		Admin!empleadoLibre()
		Admin?quienSigue(id)
		Persona[id]!pasar()
		Persona[id]?dejarSimulador()
	}
}

process Persona::[id : 1..P] {
	Admin!pedirSimulador(id)
	Empleado?pasar()
	delay(500) // Se usa el simulador
	Empleado!dejarSimulador()
}
```

# 5
> En un estadio de fútbol hay una máquina expendedora de gaseosas que debe ser usada por E Espectadores de acuerdo con el orden de llegada. Cuando el espectador accede a la máquina en su turno usa la máquina y luego se retira para dejar al siguiente. Nota: cada Espectador una sólo una vez la máquina.

```c
process Admin {
	Cola cola;
	Boolean libre = True;
	for (int i = 1; i <= E; i++) {
		if !libre; Espectador[*]?pedirMaquina(id) -> {
			cola.push(id)
			}
		* libre; Espectador[*]?pedirMaquina(id) -> {
			libre = false
			Espectador[id]!pasar()
			}
		* Espectador[*]?dejarMaquina() -> {
			if cola.empty() { 
				libre = True
			} else {
				id = cola.pop()
				Espectador[id]!pasar()
			}	
			}
		fi
	}
}

process Espectador::[id : 1..E] {
	Admin!pedirMaquina(id)
	Admin?pasar()
	// Se utiliza la máquina expendedora
	Admin!dejarMaquina()
}
```