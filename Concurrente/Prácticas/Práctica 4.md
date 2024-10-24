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