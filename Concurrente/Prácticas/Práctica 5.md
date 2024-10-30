# 1
> Se requiere modelar un puente de un único sentido que soporta hasta 5 unidades de peso. El peso de los vehículos depende del tipo: cada auto pesa 1 unidad, cada camioneta pesa 2 unidades y cada camión 3 unidades. Suponga que hay una cantidad innumerable de vehículos (A autos, B camionetas y C camiones). Analice el problema y defina qué tareas, recursos y sincronizaciones serán necesarios/convenientes para resolverlo. 
> a. Realice la solución suponiendo que todos los vehículos tienen la misma prioridad. 

```ada
PROCEDURE Puente IS
	
	TASK Administrador IS 
		ENTRY EntradaAuto;
		ENTRY EntradaCamioneta;
		ENTRY EntradaCamion;
		ENTRY Salida(Peso: IN Integer)
	END administrador;
	
	TASK BODY Administrador IS
		pesoActual: Integer := 0;
	BEGIN
		LOOP
			SELECT
				-- Entrada auto
				WHEN (pesoActual + 1 <= 5) =>
					ACCEPT EntradaAuto do
						pesoActual := pesoActual + 1;
					END EntradaAuto;
			OR
				-- Entrada camioneta
				WHEN (pesoActual + 2 <= 5) =>
					ACCEPT EntradaCamioneta do
						pesoActual := pesoActual + 2;
					END EntradaCamioneta;
			OR
				-- Entrada camion
				WHEN (pesoActual + 3 <= 5) =>
					ACCEPT EntradaCamion do
						pesoActual := pesoActual + 3;
					END EntradaCamion;
			OR
				-- Salida de cualquier vehiculo
				ACCEPT Salida (Peso: IN Integer) do
					pesoActual := pesoActual - Peso;
				END Salida
			END SELECT
		END LOOP
	END administrador
	
	TASK TYPE Auto IS
		ENTRY Cruzar;
	END Auto

	TASK BODY Auto IS
		Administrador.EntradaAuto;
		Cruzar()
		Delay(60000) -- Simulo la demora del paso
		Administrador.Salida(1)
	END Auto

	TASK TYPE Camioneta IS
		ENTRY Cruzar;
	END Camioneta

	TASK BODY Camioneta IS
		Administrador.EntradaCamioneta;
		Cruzar()
		Delay(60000) -- Simulo la demora del paso
		Administrador.Salida(2)
	END Camioneta
	
	TASK TYPE Camion IS
		ENTRY Cruzar;
	END Camion

	TASK BODY Camion IS
		Administrador.EntradaCamion;
		Cruzar()
		Delay(60000) -- Simulo la demora del paso
		Administrador.Salida(3)
	END Camion

-- Arreglos de TASKS
	Autos := array (1..A) of Auto;
	Camionetas := array (1..B) of Camioneta;
	Camiones := array (1..C) of Camion;

BEGIN
-- Progr. Principal
END Puente
```

> b. Modifique la solución para que tengan mayor prioridad los camiones que el resto de los vehículos.

```ada
PROCEDURE Puente IS
	
	TASK Administrador IS 
		ENTRY EntradaAuto;
		ENTRY EntradaCamioneta;
		ENTRY EntradaCamion;
		ENTRY Salida(Peso: IN Integer)
	END administrador;
	
	TASK BODY Administrador IS
		pesoActual: Integer := 0;
	BEGIN
		LOOP
			SELECT
				-- Entrada auto
				WHEN (pesoActual + 1 <= 5 and EntradaCamion'Count = 0) =>
					ACCEPT EntradaAuto DO
						pesoActual := pesoActual + 1;
					END EntradaAuto;
			OR
				-- Entrada camioneta
				WHEN (pesoActual + 2 <= 5 and EntradaCamion'Count = 0) =>
					ACCEPT EntradaCamioneta DO
						pesoActual := pesoActual + 2;
					END EntradaCamioneta;
			OR
				-- Entrada camion
				WHEN (pesoActual + 3 <= 5) =>
					ACCEPT EntradaCamion DO
						pesoActual := pesoActual + 3;
					END EntradaCamion;
			OR
				-- Salida de cualquier vehiculo
				ACCEPT Salida (Peso: IN Integer) DO
					pesoActual := pesoActual - Peso;
				END Salida
			END SELECT
		END LOOP
	END administrador
	
	TASK TYPE Auto IS
		ENTRY Cruzar;
	END Auto

	TASK BODY Auto IS
		Administrador.EntradaAuto;
		Cruzar()
		Delay(60000) -- Simulo la demora del paso
		Administrador.Salida(1)
	END Auto

	TASK TYPE Camioneta IS
		ENTRY Cruzar;
	END Camioneta

	TASK BODY Camioneta IS
		Administrador.EntradaCamioneta;
		Cruzar()
		Delay(60000) -- Simulo la demora del paso
		Administrador.Salida(2)
	END Camioneta
	
	TASK TYPE Camion IS
		ENTRY Cruzar;
	END Camion

	TASK BODY Camion IS
		Administrador.EntradaCamion;
		Cruzar()
		Delay(60000) -- Simulo la demora del paso
		Administrador.Salida(3)
	END Camion

-- Arreglos de TASKS
	Autos := array (1..A) of Auto;
	Camionetas := array (1..B) of Camioneta;
	Camiones := array (1..C) of Camion;

BEGIN
-- Progr. Principal
END Puente
```

--- 
# 2
> Se quiere modelar el funcionamiento de un banco, al cual llegan clientes que deben realizar un pago y retirar un comprobante. Existe un único empleado en el banco, el cual atiende de acuerdo con el orden de llegada. 
> a) Implemente una solución donde los clientes llegan y se retiran sólo después de haber sido atendidos. 
```ADA
PROCEDURE Banco IS
	TASK Empleado IS
		ENTRY RealizarPago (pago: IN Pago, comprobante: OUT Comprobante)
	END Empleado

	TASK BODY Empleado is
		comprobante : Comprobante;
	BEGIN
		LOOP
			SELECT 
				ACCEPT RealizarPago (pago: IN Pago, comprobante: OUT Comprobante) DO
					comprobante := ProcesarPago(Monto)
				END RealizarPago 
			END SELECT
		END LOOP
	END Empleado

	TASK TYPE Cliente;

	TASK BODY Cliente IS
		pago : Pago;
		comprobante : Comprobante;
	BEGIN
		Empleado.RealizarPago(pago, comprobante)
	END Cliente

	Clientes := array (1..n) of Cliente;
BEGIN
END Banco
```

> b) Implemente una solución donde los clientes se retiran si esperan más de 10 minutos para realizar el pago. 
```ADA
PROCEDURE Banco IS
	TASK Empleado IS
		ENTRY RealizarPago (pago: IN Pago, comprobante: OUT Comprobante)
	END Empleado

	TASK BODY Empleado is
		comprobante : Comprobante;
	BEGIN
		LOOP
			SELECT 
				ACCEPT RealizarPago (pago: IN Pago, comprobante: OUT Comprobante) DO
					comprobante := ProcesarPago(Monto)
				END RealizarPago 
			END SELECT
		END LOOP
	END Empleado

	TASK TYPE Cliente;

	TASK BODY Cliente IS
		pago : Pago;
		comprobante : Comprobante;
	BEGIN
		SELECT
			Empleado.RealizarPago(pago, comprobante)
		OR DELAY 600000 -- Se esperan 600000ms o 10min
			null
		END SELECT
	END Cliente

	Clientes := array (1..n) of Cliente;
BEGIN
END Banco
```

> c) Implemente una solución donde los clientes se retiran si no son atendidos inmediatamente.  
```ADA
PROCEDURE Banco IS
	TASK Empleado IS
		ENTRY RealizarPago (pago: IN Pago, comprobante: OUT Comprobante)
	END Empleado

	TASK BODY Empleado is
		comprobante : Comprobante;
	BEGIN
		LOOP
			SELECT 
				ACCEPT RealizarPago (pago: IN Pago, comprobante: OUT Comprobante) DO
					comprobante := ProcesarPago(Monto)
				END RealizarPago 
			END SELECT
		END LOOP
	END Empleado

	TASK TYPE Cliente;

	TASK BODY Cliente IS
		pago : Pago;
		comprobante : Comprobante;
	BEGIN
		SELECT
			Empleado.RealizarPago(pago, comprobante)
		ELSE -- Se retira inmediatemente
			null
		END SELECT
	END Cliente

	Clientes := array (1..n) of Cliente;
BEGIN
END Banco
```

> d) Implemente una solución donde los clientes esperan a lo sumo 10 minutos para ser atendidos. Si pasado ese lapso no fueron atendidos, entonces solicitan atención una vez más y se retiran si no son atendidos inmediatamente.

```ADA
PROCEDURE Banco IS
	TASK Empleado IS
		ENTRY RealizarPago (pago: IN Pago, comprobante: OUT Comprobante)
	END Empleado

	TASK BODY Empleado is
		comprobante : Comprobante;
	BEGIN
		LOOP
			SELECT 
				ACCEPT RealizarPago (pago: IN Pago, comprobante: OUT Comprobante) DO
					comprobante := ProcesarPago(Monto)
				END RealizarPago 
			END SELECT
		END LOOP
	END Empleado

	TASK TYPE Cliente;

	TASK BODY Cliente IS
		pago : Pago;
		comprobante : Comprobante;
	BEGIN
		SELECT
			Empleado.RealizarPago(pago, comprobante)
		OR DELAY 600000 -- Se esperan 600000ms o 10min
			SELECT
				Empleado.RealizarPago(pago, comprobante)
			ELSE -- Se retira inmediatemente
				null
			END SELECT
		END SELECT
	END Cliente

	Clientes := array (1..n) of Cliente;
BEGIN
END Banco
```