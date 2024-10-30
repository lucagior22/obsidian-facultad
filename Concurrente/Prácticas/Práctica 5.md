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
					ACCEPT EntradaAuto do
						pesoActual := pesoActual + 1;
					END EntradaAuto;
			OR
				-- Entrada camioneta
				WHEN (pesoActual + 2 <= 5 and EntradaCamion'Count = 0) =>
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