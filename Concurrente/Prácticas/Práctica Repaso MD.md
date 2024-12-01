# *Pasaje de mensajes*
# 1
>En una oficina existen 100 empleados que envían documentos para imprimir en 5 impresoras compartidas. Los pedidos de impresión son procesados por orden de llegada y se asignan a la primera impresora que se encuentre libre: 
>a) Implemente un programa que permita resolver el problema anterior usando PMA. 
```c
	chan documentos (Text);

	Process Empleado::[id: 1..100] {
		Documento documento;
		while (true) {
			documento = trabajar();
			send documentos(documento);
		}
	}
	
	Process Impresora::[id: 1..5] {
		Documento documento;
		while (true) {
			receive documentos(documento);
			imprimir(documento);
		}
	}
```

>b) Resuelva el mismo problema anterior pero ahora usando PMS.

```c

Process Impresora::[id: 1..5] {
	Documento documento;
	while(true) {
		Administrador!impresoraLibre(id);
		Administrador?imprimir(documento);
		imprimir(documento);
	}
}

Process Empleado::[id: 1..100] {
	Documento documento;
	while (true) {
		documento = generarDocumento()
		Administrador!imprimir(documento)
	}
}

Process Administrador::[id: 1..100] {
	Cola impresorasLibres, documentosAImprimir;
	
	while (true) {
		if Empleado[*]?imprimir(documento) -> {
				documentosAImprimir.push(documento);
			}
		* !empty(documentosAImprimir); Impresora[*]?impresoraLibre(idImpresoraLibre) -> {
				Impresora[idImpresoraLibre]!imprimir(documentosAImprimir.pop())
			}	
		fi
	}
}
```

---
# 2
> Resolver el siguiente problema con PMS. En la estación de trenes hay una terminal de SUBE que debe ser usada por P personas de acuerdo con el orden de llegada. Cuando la persona accede a la terminal, la usa y luego se retira para dejar al siguiente. Nota: cada Persona usa sólo una vez la terminal.
```c
Process Persona::[id : 1..P] {
	Estacion!pedirEstacion(id)
	Estacion?pasar()
	usarMaquina()
	Estacion!siguiente()
}

Process Estacion {
	Integer id;
	For i in 1..P {
		Persona[*]?pedirEstacion(id);
		Persona[id]!pasar();
		Persona[id]?siguiente()
	}
}
```

---
# 3
> Resolver el siguiente problema con PMA. En un negocio de cobros digitales hay P personas que deben pasar por la única caja de cobros para realizar el pago de sus boletas. Las personas son atendidas de acuerdo con el orden de llegada, teniendo prioridad aquellos que deben pagar menos de 5 boletas de los que pagan más. Adicionalmente, las personas embarazadas tienen prioridad sobre los dos casos anteriores. Las personas entregan sus boletas al cajero y el dinero de pago; el cajero les devuelve el vuelto y los recibos de pago.
```c
chan pedidoSinPrioridad(int, Array<Boleta>, double), pedidoPrioridad(int, Array<Boleta>, double), pedidoEmbarazada(int, Array<Boleta>, double), respuesta[1..P](), hayPedido();

Process Persona::[id : 1..P] {
	bool soyEmbarazada = cargarEmbarazo()
	double monto = cargarDinero()
	double vuelto;
	Array<Boleta> boletas = cargarBoletas()
	Array<Recibo> recibos;
	
	if (soyEmbarazada) {
		send pedidoEmbarazada(id, boletas, monto)
	} else if (boletas.length < 5) {
		send pedidoPrioridad(id, boletas, monto)
	} else {
		send pedidoSinPrioridad(id, boletas, monto)
	}
	send hayPedido()
	receive respuesta[id](vuelto, recibos)	
}

Process Cajero {
	double monto;
	double vuelto;
	int id;
	Array<Boleta> boletas;
	Array<Recibo> recibos;
	
	while (true) {
		receive hayPedido()
		if (!pedidoEmbarazada.empty) {
			receive pedidoEmbarazada(id, boletas, monto)
		} else if (!pedidoPrioridad.empty) {
			receive pedidoPrioridad(id, boletas, monto)
		} else {
			receive pedidoSinPrioridad(id, boletas, monto)
		}
		(recibos, vuelto) = cobrar(boletas, monto)
		send respuesta[id](vuelto, recibos)
	}
}
```


---
# *ADA*

# 2
> Resolver el siguiente problema. En un negocio de cobros digitales hay P personas que deben pasar por la única caja de cobros para realizar el pago de sus boletas. Las personas son atendidas de acuerdo con el orden de llegada, teniendo prioridad aquellos que deben pagar menos de 5 boletas de los que pagan más. Adicionalmente, las personas ancianas tienen prioridad sobre los dos casos anteriores. Las personas entregan sus boletas al cajero y el dinero de pago; el cajero les devuelve el vuelto y los recibos de pago.

```ada
PROCEDURE Cajero IS
	TASK TYPE Persona;
	
	TASK BODY Persona IS
		boletas : list of Text;
		edad : Integer;
		dinero : Float;
	BEGIN
		boletas := cargarBoletas()
		edad := cargarEdad()
		dinero := cargarDinero()
		IF edad > 70 DO
			Caja.PagarAncianos(boletas, dinero)
		ELSE IF boletas.length() > 5 DO
			Caja.PagarMasDeCincoBoletas(boletas, dinero)
		ELSE
			Caja.PagarMenosDeCincoBoletas(boletas, dinero)	 
		END IF
	END
	
	TASK Caja IS
		ENTRY PagarMenosDeCincoBoletas (boletas : IN list of Text, dinero : INOUT Float, recibos: OUT list of Text)
		ENTRY PagarMasDeCincoBoletas (boletas : IN list of Text, dinero : INOUT Float, recibos: OUT list of Text)
		ENTRY PagarAncianos (boletas : IN list of Text, dinero : INOUT Float, recibos: OUT list of Text)
	END
	
	TASK BODY Caja IS
	BEGIN
		WHILE (True) LOOP
			SELECT 
				WHEN (PagarAncianos'Count == 0 and PagarMenosDeCincoBoletas'Count)
				ACCEPT PagarMasDeCincoBoletas(boletas : IN list of Text, dinero : INOUT Float, recibos: OUT list of Text) DO
					(dinero, recibos) = procesarPago(boletas, dinero)
				END PagarMasDeCincoBoletas
			OR
								WHEN (PagarAncianos'Count == 0)
				ACCEPT PagarMenosDeCincoBoletas(boletas : IN list of Text, dinero : INOUT Float, recibos: OUT list of Text) DO
					(dinero, recibos) = procesarPago(boletas, dinero)
				END PagarMenosDeCincoBoletas
			OR
				ACCEPT PagarAncianos(boletas : IN list of Text, dinero : INOUT Float, recibos: OUT list of Text) DO
					(dinero, recibos) = procesarPago(boletas, dinero)
				END PagarAncianos
			END SELECT
		END LOOP
	END
	
	Personas : array (1..P) of Persona;
BEGIN
	null
END
```

---
# 3
> Resolver el siguiente problema. La oficina central de una empresa de venta de indumentaria debe calcular cuántas veces fue vendido cada uno de los artículos de su catálogo. 
> La empresa se compone de 100 sucursales y cada una de ellas maneja su propia base de datos de ventas. La oficina central cuenta con una herramienta que funciona de la siguiente manera: ante la consulta realizada para un artículo determinado, la herramienta envía el identificador del artículo a las sucursales, para que cada una calcule cuántas veces fue vendido en ella. Al final del procesamiento, la herramienta debe conocer cuántas veces fue vendido en total, considerando todas las sucursales. Cuando ha terminado de procesar un artículo comienza con el siguiente (suponga que la herramienta tiene una función generarArtículo() que retorna el siguiente ID a consultar). 
> **Nota**: maximizar la concurrencia. Existe una función ObtenerVentas(ID) que retorna la cantidad de veces que fue vendido el artículo con identificador ID en la base de la sucursal que la llama.

```ada
PROCEDURE Indumentaria IS
	TASK OficinaCentral IS
		ENTRY ObtenerCantidadVentas (cantVentas : IN Integer);
		ENTRY QuieroRecibirID (id : OUT Integer);
		ENTRY Siguiente;
	END
	
	TASK BODY OficinaCentral IS
		idProducto, acumuladoVentas : Integer;
	BEGIN
		LOOP
			acumuladoVentas := 0;
			idProducto := generarArticulo();
			FOR i IN 1..200 LOOP
				SELECT
					ACCEPT QuieroRecibirID (id : OUT Integer) DO
						id := idProducto;
					END   
				OR 
					ACCEPT ObtenerCantidadVentas (cantVentas : IN Integer) DO
						acumuladoVentas := acumuladoVentas + cantVentas;
					END   
				END SELECT
			END LOOP
			FOR i in 1..100 LOOP
				ACCEPT Siguiente;
			END LOOP
		END LOOP
	END
	
	TASK TYPE Sucursal;
	
	TASK BODY Sucursal IS
		id, cantVentas : Integer;
	BEGIN 
		LOOP
			OficinaCentral.QuieroRecibirID(id)
			cantVentas := ObtenerVentas(id)
			OficinaCentral.ObtenerCantidadVentas(cantVentas)
			OficinaCentral.Siguiente
		END LOOP
	END
	
	Sucursales : array (1..100) of Sucursal;
BEGIN
END
```

