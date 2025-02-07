# Adapter

## Propósito
Convierte la interfaz de una clase en otra interfaz que es la que esperan los clientes. Permite que cooperen clases que de otra forma no podrían por tener interfaces incompatibles.

## Aplicabilidad
- Se quiere usar una clase existente y su interfaz no concuerda con la que necesita.
- Se quiere crear una clase reutilizable que coopere con clases no relacionadas o que no han sido previstas, es decir, clases que no tienen por qué tener interfaces compatibles.

## Estructura
![[Pasted image 20250205154750.png]]

## Participantes
- Objetivo
	- Define la interfaz específica del dominio que usa el *Cliente*.
- Cliente
	- Colabora con objetos que se ajustan a la interfaz *Objetivo*.
- Adaptable
	- Define una interfaz existente que necesita ser adaptada.
- Adaptador
	- Adapta la interfaz de *Adaptable* a la interfaz *Objetivo*

### Colaboraciones
- Los *Clientes* llaman a operaciones de una instancia de *Adaptador*.
- El *Adaptador* llama a operaciones de *Adaptable*, que son las que satisfacen la petición del *Cliente*.

# Composite

## Propósito
Compone objetos en estructuras de arbol para representar jerarquías de parte-todo. Permite que los clientes traten de manera uniforme a los objetos individuales y a los compuestos.

## Aplicabilidad
- Quiera representar jerarquías de objetos parte-todo.
- Quiera que los clientes sean capaces de obviar las diferencias entre composiciones de objetos y los objetos individuales. Los clientes tratarán a todos los objetos de la estructura compuesta de manera uniforme.

## Estructura
![[Pasted image 20250205154555.png]]

## Participantes
- **Componente Abstracto**
	- Declara la interfaz de los objetos de la composición.
	- Implementa el comportamiento predeterminado de la interfaz que es común a todas las clases.
	- Declara una interfaz para acceder a sus componentes hijos y gestionarlos.
	- *(Opcional)* Define una interfaz para acceder al padre de un componente en la estructura recursiva y, si es necesario, la implementa.
- **Hoja**
	- Representa objetos hoja en la composición, no tiene hijos.
	- Define el comportamiento de los objetos primitivos de la composición.
- **Compuesto**
	- Define el comportamiento de los componentes que tienen hijos.
	- Almacena componentes hijos.
	- Implementa operaciones de la interfaz *Componente Abstracto* relacionadas con los hijos.
- **Cliente**
	- Manipula objetos en la composición a través de la interfaz *Componente Abstracto*

### Colaboraciones
- *Clientes* usan la interfaz de la clase *Componente Abstracto* para interactuar con los objetos de la estructura compuesta.+
- Las *Hoja* trata correctamente las peticiones de los *Clientes*
- Los *Compuesto* normalmente redirigen las peticiones a sus componentes hijos.

# Decorator

## Propósito
Asigna responsabilidades a un objeto dinámicamente, proporcionando una alternativa flexible a la herencia para extender la funcionalidad.

## Aplicabilidad
- Quiera añadir objetos individales de forma dinámica y transparente, es decir, sin afectar a otros objetos.
- Quiera responsabilidades que pueden ser retiradas.
- La extensión mediante la herencia no es viable. A veces es posible tener un gran número de extensiones independientes, produciéndose una exposión de subclases para permitir todas las combinaciones. O puede ser que una definición de una clase esté oculta o que no esté disponible para ser heredada.

## Estructura
![[Pasted image 20250205163412.png]]

## Participantes
- **Componente Abstracto**
	- Define la interfaz para objetos a los que se puede añadir responsabilidades dinámicamente.
- **Componente Concreto**
	- Define un objeto al que se pueden añadir responsabilidades adicionales.
- **Decorador Abstracto**
	- Mantiene una referencia a un objeto *Componente Abstracto* y define una interfaz qeu se ajusta a la interfaz del *Componente Abstracto*
- **Decorador Concreto**
	- Añade responsabilidades al componente.

### Colaboraciones
El decorador redirige peticiones a su objeto *Componente Abstracto*. Opcionalmente puede realizar operaciones adicionales antes y depués de reenviar la petición.

# Proxy

## Propósito
Proporciona un representante o sustituto de otro objeto para controlar el acceso a éste.

## Aplicabilidad
- **Proxy Remoto**: Proporciona un representante local de un objeto situado en otro espacio de direcciones.
- **Proxy Virtual**: Crea objetos costosos por encargo. 
- **Proxy de Protección:** Controla el acceso al objeto original.

## Estructura
![[Pasted image 20250205165040.png]]

### Participantes
- **Proxy**
	- Mantiene una referencia al *Sujeto Real*.
	- Proporciona una interfaz idéntica a la del *Sujeto abstracto*
	- Controla el acceso al *Sujeto Real*, y puede ser responsable de su creación y borrado.
- **Sujeto Abstracto**
	- Define la interfaz común para el *Sujeto Real* y el *Proxy*, de modo que pueda usarse un *Proxy* en cualquier sitio en el que se espere un *Sujeto Real*.
- **Sujeto Real**
	- Define el objeto real representado.

## Colaboraciones
El *Proxy* redirige peticiones al *Sujeto Real* cuando sea necesario, dependiendo del tipo de proxy.