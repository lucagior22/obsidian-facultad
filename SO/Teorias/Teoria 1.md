# ¿Qué es un SO?
- **Software** intermediario entre el usuario de una computadora y su software.
	- Al ser software, necesita procesador y memoria.

# Diferentes perspectivas
### Desde el usuario
- Abstracción con respecto a la arquitectura u organización del HW.
- Comodidad, friendliness

### Desde la administración de recursos
- Administra los recursos de HW de uno o mas procesos.
- Maneja la memoria secundaria e IO.
- Multiplexación en tiempo y espacio.

# Objetivos de los SO
- Comodidad
	- HW más facil de usar.
- Eficiencia
	- Hacer mas eficiente el uso de los recursos.
- Evolución
	- Permitir la introducción de nuevas funciones sin interferir con funciones anteriores.

# Componentes de un SO
- Kernel
- Shell
- Herramientas
# Kernel
- Es código.
	- Se carga en la memoria ppal.
- Administra recursos.
- Implementa servicios esenciales:
	- Manejo de memoria y CPU.
	- Administración de procesos.
	- Comunicación y concurrencia.
	- Gestion de IO.
	- Acceso **seguro** e igualitario al HW.
- Kernel Linux:
	- Programa que ejecuta más programas y gestiona HW.
	- Es de código abierto.
- Se ejecuta en modo **supervisor**.
	- De esta manera tiene acceso al conjunto de configuraciones necesarias para llevar a cabo sus funciones.
### Apoyo del HW:
- El kernel se apoya en ciertos dispositivos o mecanismos del HW para cumplir sus funciones.
- Modos de ejecución:
	- Define limitaciones en el conjunto de instrucciones que se puede ejecutar en cada modo.
	- Un bit en la CPU indica el modo actual.
		- Cuando hay un trap o una interrupción, el bit de modo se pone en modo Kernel.
			- Única forma de pasar a modo Kernel.
			- El proceso de usuario no hace el cambio explícitamente.
	- Las instrucciones privilegiadas deben ejecutarse en modo **Supervisor o Kernel**
	- En modo **Usuario** el proceso puede acceder solo a su espacio de direcciones.
- Interrupción de clock:
	- Evita que un proceso tome el control del CPU.
- Protección de memoria:
	- Se definen espacios limitados a cada proceso para su acceso a la memoria (registros base y límite).

### Caracterizaciones
### Monolítico
- Todos los servicios del SO están en un solo bloque de código que se ejecuta en modo **Supervisor**.
- Acceso completo a los recursos de HW.
- Mayor dificultad de mantención, administración y manejo.
- Linux, FreeBSD, Unix.

### Microkernel
- Minimiza la cantidad de código que se ejecuta en modo supervisor.
	- Busca ser mas liviano que un monolítico.
- Altamente modular.
- La mayoria de servicios se ejecutan en modo **Usuario**.
	- Mayor seguridad.
	- Rendimiento inferior debido a los constantes cambios de modo.
- Minix.

### Hibrido
- Combina características de Kernels monolíticos y microkernels.
- Son modulares.
	- Equilibrio entre rendimiento y modularidad.
	- Mayor flexibilidad en el desarrollo.
- Mejor rendimiento que microkernel.
- Mayor modularidad/seguridad que monolítico.
- Windows, MacOs.

### ExoKernel
- Provee servicios muy básicos al SO.
	- Evita alto nivel de abstracción.
- Ofrece acceso directo al HW.
- Provee primitivas básicas para la gestión de memoria.
- Se busca que las aplicaciones agreguen la funcionalidad que necesiten.

### Real-Time Kernel
- Algoritmos de scheduling CPU avanzados.
	- Busca asegurar orden correcto de ejecución y cumplir con los plazos establecidos.
- Altamente centrado en el tiempo y que todo se realice como previsto.
- Provee mecanismos de seguridad robustos.

| **Tipo de Kernel**      | **Características** |
|------------------------|--------------------|
| **Monolítico**         | - Todos los servicios del SO están en un solo bloque de código que se ejecuta en modo **Supervisor**. <br> - Acceso completo a los recursos de HW. <br> - Mayor dificultad de mantención, administración y manejo. <br> - Ejemplos: Linux, FreeBSD, Unix. |
| **Microkernel**        | - Minimiza la cantidad de código que se ejecuta en modo supervisor. <br> - Busca ser más liviano que un monolítico. <br> - Altamente modular. <br> - La mayoría de servicios se ejecutan en modo **Usuario**. <br> - Mayor seguridad, pero rendimiento inferior debido a los constantes cambios de modo. <br> - Ejemplo: Minix. |
| **Híbrido**            | - Combina características de Kernels monolíticos y microkernels. <br> - Son modulares. <br> - Equilibrio entre rendimiento y modularidad. <br> - Mayor flexibilidad en el desarrollo. <br> - Mejor rendimiento que microkernel. <br> - Mayor modularidad/seguridad que monolítico. <br> - Ejemplos: Windows, MacOS. |
| **ExoKernel**         | - Provee servicios muy básicos al SO. <br> - Evita alto nivel de abstracción. <br> - Ofrece acceso directo al HW. <br> - Provee primitivas básicas para la gestión de memoria. <br> - Se busca que las aplicaciones agreguen la funcionalidad que necesiten. |
| **Real-Time Kernel**   | - Algoritmos de scheduling CPU avanzados. <br> - Busca asegurar orden correcto de ejecución y cumplir con los plazos establecidos. <br> - Altamente centrado en el tiempo y que todo se realice como previsto. <br> - Provee mecanismos de seguridad robustos. |
