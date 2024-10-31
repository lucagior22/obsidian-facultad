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

```ada
PROCEDURE Sistema IS
	TASK Servidor IS
		ENTRY EntregaDocumento (documento : IN OUT Text, resultado : OUT Text) -- Parámetro documento INOUT simulando la entrega y corrección, y parámetro error 
	END Servidor
	
	TASK BODY Servidor IS
		LOOP
			ACCEPT EntregaDocumento (documento : IN OUT Text, resultado : OUT Text) DO
				resultado := corregirDocumento(documento)
			END EntregaDocumento
		END LOOP
	END Servidor
	
	TASK TYPE Usuario;
	
	TASK BODY Usuario IS
		documento, resultado : Text;
	BEGIN
		documento := trabajarDocumento()
		WHILE (resultado != "Todo bien") LOOP
			SELECT
				Servidor.EntregarDocumento(documento, resultado)
			OR DELAY 120000
				Delay(60000)
			END SELECT
		END LOOP
	END Usuario
	
	Usuarios := array (1..U) of Usuario;
BEGIN
END Sistema
```

---
# 5
> En una playa hay 5 equipos de 4 personas cada uno (en total son 20 personas donde cada una conoce previamente a que equipo pertenece). Cuando las personas van llegando esperan con los de su equipo hasta que el mismo esté completo (hayan llegado los 4 integrantes), a partir de ese momento el equipo comienza a jugar. 
> El juego consiste en que cada integrante del grupo junta 15 monedas de a una en una playa (las monedas pueden ser de 1, 2 o 5 pesos) y se suman los montos de las 60 monedas conseguidas en el grupo. 
> Al finalizar cada persona debe conocer el grupo que más dinero junto. 
> Nota: maximizar la concurrencia. Suponga que para simular la búsqueda de una moneda por parte de una persona existe una función Moneda() que retorna el valor de la moneda encontrada.

```ada
PROCEDURE Playa IS
	TASK TYPE Persona IS
		ENTRY ObtenerIdentificadores (idPersona : IN Integer, idGrupo : IN Integer);
		ENTRY Comenzar;
	END Persona
	
	TASK BODY Persona IS
		idGrupo, id, moneda, acumuladoMonedas, grupoGanador : Integer;
	BEGIN
		-- Se asignan los identificadores
		ACCEPT ObtenerIdentificadores (idPersona : IN Integer, idGrupo : IN Integer) IS
			id := idPersona;
			idGrupo := idGrupo
		END ObtenerIdentificadores
		
		-- Barrera hasta que lleguen todos los del grupo
		Grupos(idGrupo).LlegadaPersona(id)
		ACCEPT Comenzar;
		
		-- Juego
		acumuladoMonedas := 0;
		FOR i IN 1..15 LOOP
			moneda := Moneda()
			acumuladoMonedas := acumuladoMonedas + moneda;
		END LOOP
		
		-- Recuento y resultado final
		Grupos(idGrupo).AgregarAcumulado(acumuladoMonedas)
		ACCEPT ObtenerResultadoFinal(grupoGanador : IN Integer) DO
			grupoGanador := grupoGanador
		END ObtenerResultadoFinal
		
	END Persona
	
	TASK Grupo IS
		ENTRY LlegadaPersona (idPersona : IN Integer)
		ENTRY AgregarAcumulado (acumulado : IN Integer)
		ENTRY ObtenerIdentificador (id : IN Integer)
	END Grupo
	
	TASK BODY Grupo IS
		idGrupo, acumuladoTotal, personasPresentes : Integer;
		integrantes : array (1..5) of Integer;
	BEGIN
		-- Obtención del identificador
		ACCEPT ObtenerIdentificador (id : IN Integer) DO
			idGrupo := id
		END ObtenerIdentificador
		
		-- Barrera
		personasPresentes := 0
		FOR i IN 1..5 LOOP
			ACCEPT LlegadaPersona (idPersona : IN Integer) DO
				personasPresentes := personasPresentes + 1
				integrantes(i) := idPersona
			END LlegadaPersona
		END LOOP
		FOR i IN 1..5 LOOP
			Personas(integrantes(i)).comenzar
		END LOOP
		
		-- Suma de acumulados
		acumuladoTotal := 0;
		FOR i IN 1..5 LOOP
			ACCEPT AgregarAcumulado (acumulado : IN Integer) DO
				acumuladoTotal := acumuladoTotal + acumulado;
			END AgregarAcumulado
		END LOOP
		
		-- Entrega de resultados
		Coordinador.EntregarAcumuladoGrupo(idGrupo)
	END Grupo
	
	TASK Coordinador IS
		ENTRY EntregarAcumuladoGrupo (idGrupo : IN Integer, acumulado : IN Integer)
	END Coordinador

	TASK BODY Coordinador IS
		maximoAcumulado, grupoGanador : Integer;
	BEGIN
		FOR i IN 1..4 LOOP
			ACCEPT EntregarAcumuladoGrupo (idGrupo : IN Integer, acumulado : IN Integer)
				IF maximoAcumulado < acumulado THEN 
					acumulado := maximoAcumulado;
					grupoGanador := idGrupo;
				 END IF
			END EntregarAcumuladoGrupo
		END LOOP
		
		FOR i IN 1..20 LOOP
			Personas(i).ObtenerResultadoFinal(grupoGanador)
		END LOOP	
	END Coordinador

	Personas := array (1..20) of Persona;
	Grupos := array (1..4) of Grupo;

	i, idGrupo : Integer;
BEGIN
	FOR i IN 1..20 LOOP
		idGrupo := (i MOD 5) + 1
		Personas(i).ObtenerIdentificadores(i, idGrupo)
	END LOOP
	
	FOR i IN 1..4 LOOP
		Grupos.ObtenerIdentificador(i)
	END LOOP
END Playa
```

---
# 6
> Se debe calcular el valor promedio de un vector de 1 millón de números enteros que se encuentra distribuido entre 10 procesos Worker (es decir, cada Worker tiene un vector de 100 mil números). 
> Para ello, existe un Coordinador que determina el momento en que se debe realizar el cálculo de este promedio y que, además, se queda con el resultado. 
> **Nota**: maximizar la concurrencia; este cálculo se hace una sola vez.
```ada
PROCEDURE Promedio IS
	TASK TYPE Worker IS
		ENTRY Comenzar;
	END Worker
	
	TASK BODY Worker IS
		numeros : array (1..100000) of Integer;
		acumulado, promedio : Integer;
	BEGIN
		ACCEPT Comenzar;
		acumulado := 0;
		FOR i IN 1..100000 LOOP
			acumulado := acumulado + numeros(i)
		END LOOP
		promedio := acumulado / 100000
		Coordinador.EnviarPromedio(promedio)
	END Worker
	
	TASK Coordinador IS
		ENTRY EnviarPromedio (promedio : IN Integer)
	END Coordinador
	
	TASK BODY Coordinador IS
		acumuladoTotal, promedioTotal : Integer;
	BEGIN
		FOR i IN 1..10 LOOP
			Workers(i).Comenzar
		END LOOP
		
		acumuladoTotal := 0;
		FOR i IN 1..10 LOOP
			ACCEPT EnviarPromedio (promedio : IN Integer) DO
				acumuladoTotal := acumuladoTotal + promedio
			END EnviarPromedio
		END LOOP
		
		promedioTotal := acumuladoTotal / 10
	END Coordinador
	
	Workers := array (1..10) of Worker;
BEGIN
END Promedio
```
*Consulta: ¿Cómo determina el Coordinador el comienzo de los Workers?*