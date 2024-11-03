
> Resuelva con **ADA** el siguiente problema: la página web del Banco Central exhibe las diferentes cotizaciones del dólar oficial de 20 bancos del país, tanto para la compra como para la venta. Existe una tarea programada que se ocupa de actualizar la página en forma periódica y para ello consulta la cotización de cada uno de los 20 bancos. Cada banco dispone de una API, cuya única función es procesar las solicitudes de aplicaciones externas. La tarea programada consulta de a una API por vez, esperando a lo sumo 5 segundos por su respuesta. Si pasado ese tiempo no respondió, entonces se mostrará vacía la información de ese banco.

```ada
PROCEDURE BancoCentral IS
	TASK TYPE APIBanco IS
		ENTRY ConsultarCotizacion (cotizacionCompra : OUT Float, cotizacionCompra : OUT Float)
	END APIBanco
	
	TASK BODY APIBanco IS
		cotizacionCompraActual, cotizacionVentaActual : Float
	BEGIN
		LOOP
			actualizarCotizacion(cotizacionCompraActual, cotizacionVentaActual)
			ACCEPT ConsultarCotizacion (cotizacionCompra : OUT Float, cotizacionCompra : OUT Float) DO
				cotizacionCompra := cotizacionCompraActual
				cotizacionVenta := cotizacionVentaActual
			END ConsultarCotizacion
		END LOOP
	END APIBanco
	
	TASK TareaProgramada;
	
	TASK BODY TareaProgramada IS
		cotizacionesCompra : array (1..20) of Float;
		cotizacionesVenta : array (1..20) of Float;
	BEGIN
		LOOP
			FOR i IN 1..20 LOOP
				SELECT
					APIBanco(i).ConsultarCotizacion(cotizacionesCompra(i), cotizacionesVenta(i))
				OR DELAY 5.0
					cotizacionesCompra(i) := null
					cotizacionesVenta(i) := null
				END SELECT
			END LOOP
		END LOOP
	END TareaProgramada
	
	APIS : array (1..20) of APIBanco;
BEGIN
	null
END BancoCentral
```

---
> Resolver el siguiente problema con Pasaje de Mensajes Sincrónicos (PMS). En una excursión hay una tirolesa que debe ser usada por 20 turistas. Para esto hay un guía y un empleado. El empelado espera a que todos los turistas hayan llegado para darles una charla explicando las medidas de seguridad. Cuando termina la charla los turistas piden usar la tirolesa y esperan a que el guía les vaya dando el permiso de tirarse. El guía deja usar la tirolesa a un cliente a la vez y de acuerdo al orden en que lo van solicitando.  Nota: todos los procesos deben terminar;  suponga que el empleado tienen una función DarCharla() que simula que el empleado está dando la charla, y los turistas tienen una función UsarTitolesa() que simula que está usando la tirolesa.

```c
Process Turista::[1..20] {
	Empleado!llegadaTurista
	Empleado?finCharla
	Guia!pedidoTirolesa(id)
	Guia?pasoATirolesa
	UsarTirolesa()
	Guia!finTirolesa
}

Process Empleado {
	for (int i = 1; i <= 20; i++) {
		Turista[*]?llegadaTurista
	}
	DarCharla()
	for (int j = 1; j <= 20; j++) {
		Turista[j]!finCharla
	}
}

Process Guia {
	Int idAct;
	Cola cola;
	Bool libre;
	libre = true;
	for (int i = 1; i <= 40; i++) {
		if Turista[*]?pedidoTirolesa(id) -> {
				if (not libre) {
					cola.push(id)
				} else {
					libre = false
					idAct = id
					Turista[id]!pasoATirolesa
				}
			} 
		* Turista[idAct]?finTirolesa -> {
				if (empty(cola)) {
					libre = true
				} else {
					idAct = cola.pop()
					Turista[idAct]!pasoATirolesa
				}	
			}
	    fi
	}
}
```