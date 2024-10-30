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
					comprobante := ProcesarPago(pago)
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
					comprobante := ProcesarPago(pago)
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
					comprobante := ProcesarPago(pago)
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
					comprobante := ProcesarPago(pago)
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

---
# 3
> Se dispone de un sistema compuesto por 1 central y 2 procesos periféricos, que se comunican continuamente. Se requiere modelar su funcionamiento considerando las siguientes condiciones: 
> - La central siempre comienza su ejecución tomando una señal del proceso 1; luego toma aleatoriamente señales de cualquiera de los dos indefinidamente. Al recibir una señal de proceso 2, recibe señales del mismo proceso durante 3 minutos. 
> - Los procesos periféricos envían señales continuamente a la central. La señal del proceso 1 será considerada vieja (se deshecha) si en 2 minutos no fue recibida. Si la señal del proceso 2 no puede ser recibida inmediatamente, entonces espera 1 minuto y vuelve a mandarla (no se deshecha).
```ada
PROCEDURE Sistema IS
	TASK Timer IS
		ENTRY Comienzo (milisegundos : IN Integer)
	END Timer
	
	TASK BODY Timer IS
	BEGIN
		LOOP
			ACCEPT Comienzo (milisegundos : IN Integer) DO
				null
			END Comienzo
			Delay(milisegundos)
			Central.fin
		END LOOP
	END Timer

	TASK Central IS
		ENTRY SeñalUno;
		ENTRY SeñalDos;
		ENTRY Fin;
	END Central
	
	TASK BODY Central IS
		seguir : Boolean;
	BEGIN
		ACCEPT SeñalUno;
		
		LOOP
			SELECT
				ACCEPT SeñalUno DO
					null
				END SeñalUno
			OR
				ACCEPT SeñalDos DO
					null
				END SeñalDos
				Timer.Comienzo(180000)
				seguir := True;
				WHILE (seguir) LOOP
					SELECT
						WHEN (Fin'Count == 0) =>
							ACCEPT SeñalDos DO
								null
							END SeñalDos
					OR
						ACCEPT Fin DO
							seguir := False;
						END Fin
					END SELECT
				END WHILE	
		END LOOP
	END Central

	TASK PerifericoUno;
	
	TASK BODY PerifericoUno IS
	BEGIN	
		LOOP
			SELECT
				Central.SeñalUno
			OR DELAY (120000)
				null
			END SELECT
		END LOOP
	END PerifericoUno
	
	TASK PerifericoDos;
	
	TASK BODY PerifericoDos IS
	BEGIN
		LOOP
			SELECT
				Central.SeñalDos 
			ELSE
				Delay (60000)
				Central.SeñalDos
			END SELECT
		END LOOP
	END PerifericoDos

BEGIN
	null
END Sistema
```

---
# 4a (Primer enunciado)
> En una clínica existe un médico de guardia que recibe continuamente peticiones de atención de las E enfermeras que trabajan en su piso y de las P personas que llegan a la clínica ser atendidos. 
> - Cuando una **persona** necesita que la atiendan espera a lo sumo 5 minutos a que el médico lo haga, si pasado ese tiempo no lo hace, espera 10 minutos y vuelve a requerir la atención del médico. Si no es atendida tres veces, se enoja y se retira de la clínica. 
> - Cuando una **enfermera** requiere la atención del médico, si este no lo atiende inmediatamente le hace una nota y se la deja en el consultorio para que esta resuelva su pedido en el momento que pueda (el pedido puede ser que el médico le firme algún papel). Cuando la petición ha sido recibida por el médico o la nota ha sido dejada en el escritorio, continúa trabajando y haciendo más peticiones.
> - El **médico** atiende los pedidos dándole prioridad a los enfermos que llegan para ser atendidos. Cuando atiende un pedido, recibe la solicitud y la procesa durante un cierto tiempo. Cuando está libre aprovecha a procesar las notas dejadas por las enfermeras. 
```ada
PROCEDURE Clinica IS
	TASK Medico IS
		ENTRY AtenderPersona (solicitud : IN Text);
		ENTRY AtenderEnfermera (solicitud : IN Text);
	END Medico

	TASK BODY Medico IS
		nota, solicitud : Text;
	BEGIN
		LOOP
			SELECT
				ACCEPT AtenderPersona (solicitud : IN Text) DO
					trabajarSolicitud(solicitud)
				END AtenderPersona
			OR
				WHEN (AtenderPersona'Count == 0) =>
					ACCEPT AtenderEnfermera (solicitud : IN Text) DO
						trabajarSolicitud(solicitud)
					END AtenderEnfermera
			OR
				WHEN (AtenderPersona'Count == 0 and AtenderEnfermera'Count == 0) =>
					SELECT
						Escritorio.TomarNota(nota)
						trabajarNota(nota)
					ELSE
						null
					END SELECT
			END SELECT
		END LOOP
	END Medico
	
	-----------------------------------------------
	
	TASK TYPE Enfermera;
	
	TASK BODY Enfermera IS
		nota : Text;
	BEGIN
		LOOP
			Delay(100000) -- Simulo trabajo
			SELECT
				Medico.AtenderEnfermera
			ELSE
				nota = hacerNota()
				Escritorio.DejarNota(nota)
			END SELECT
		END LOOP
	END Enfermera
	
	-----------------------------------------------
	
	TASK TYPE Persona;
	
	TASK BODY Persona IS
		intentos : Integer;
		fuiAtendido : Boolean;
	BEGIN
		intentos := 0;
		fuiAtendido := False;
		WHILE (intentos < 3 or !fuiAtendido) LOOP
			SELECT
				Medico.AtenderPersona
			OR DELAY 300000
				Delay(600000);
				intentos := intentos + 1;
			END SELECT
		END LOOP
		-- Se retira
	END Persona
	
	-----------------------------------------------
	
	TASK Escritorio IS
		ENTRY DejarNota (nota : IN Text);
		ENTRY TomarNota (nota : OUT Text)
	END Escritorio
	
	TASK BODY Escritorio IS
		notas : queue of Text;
	BEGIN
		LOOP
			SELECT
				ACCEPT DejarNota (nota : IN Text) DO
					notas.push(nota)
				END DejarNota
			OR
				WHEN (!notas.empty()) =>
					ACCEPT TomarNota (nota : OUT Text) DO
						nota := notas.pop()
					END TomarNota
			END SELECT
		END LOOP
	END Escritorio
	
	-----------------------------------------------
	
	Enfermeras := array (1..E) of Enfermera;
	Personas := array (1..P) of Persona;
BEGIN
	null
END Clinica
```

Consultas MIAS: 
	- ¿Que diferencia hay entre los siguientes SELECT?
		- Analisis con amigos:
			- El primer *SELECT* **SIEMPRE** va a responder a un *ENTRY CALL* o en todo caso se queda dormido indefinidamente en el *SELECT*.
			- El segundo *SELECT* puede no responder a un *ENTRY CALL* dado el caso de que se ejecuten los dos *ELSE*. Entendemos que esto puede derivar en *BUSY WAITING* ya que la tarea loopearia en un un select del que sale sin responder un *ENTRY CALL*.
 
```ada
-- Primer SELECT
SELECT
	ACCEPT AtenderPersona (solicitud : IN Text) DO
		trabajarSolicitud(solicitud)
	END AtenderPersona
OR
	WHEN (AtenderPersona'Count == 0) =>
		ACCEPT AtenderEnfermera (solicitud : IN Text) DO
			trabajarSolicitud(solicitud)
		END AtenderEnfermera
OR
	WHEN (AtenderPersona'Count == 0 and AtenderEnfermera'Count == 0) =>
		SELECT
			Escritorio.TomarNota(nota)
			trabajarNota(nota)
		ELSE
			null
		END SELECT
END SELECT

-- Segundo SELECT
SELECT
	ACCEPT AtenderPersona (solicitud : IN Text) DO
		trabajarSolicitud(solicitud)
	END AtenderPersona
OR
	WHEN (AtenderPersona'Count == 0) =>
		ACCEPT AtenderEnfermera (solicitud : IN Text) DO
			trabajarSolicitud(solicitud)
		END AtenderEnfermera
ELSE
	SELECT
		Escritorio.TomarNota(nota)
		trabajarNota(nota)
	ELSE
		null
	END SELECT
END SELECT
```

---
# 4b (Segundo enunciado)
> En un sistema para acreditar carreras universitarias, hay UN Servidor que atiende pedidos de U Usuarios de a uno a la vez y de acuerdo con el orden en que se hacen los pedidos. Cada usuario trabaja en el documento a presentar, y luego lo envía al servidor; espera la respuesta de este que le indica si está todo bien o hay algún error. Mientras haya algún error, vuelve a trabajar con el documento y a enviarlo al servidor. Cuando el servidor le responde que está todo bien, el usuario se retira. Cuando un usuario envía un pedido espera a lo sumo 2 minutos a que sea recibido por el servidor, pasado ese tiempo espera un minuto y vuelve a intentarlo (usando el mismo documento).