
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

