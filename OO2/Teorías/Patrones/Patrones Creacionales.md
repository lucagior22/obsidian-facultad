# Builder

## Propósito
Separa la construcción de un objeto complejo de su representación, es decir, alguien mas que el mismo objeto se encarga de su compleja construcción generalmente dividida en pasos marcados y diferentes representaciones.

## Aplicabilidad
- El algoritmo para crear un objeto complejo debiera ser independiente de las partes de que se compone dicho objeto y de como se ensamblan.
- El proceso de construcción debe permitir diferentes representaciones.

## Estructura
- **Director**
	- Construye un objeto usando la interfaz *Constructor Abstracto*
- **Constructor Abstracto**
	- Especifica una interfaz abstracta para crear las partes de un objeto Producto.
- **Constructor Concreto**
	- Implementa la interfaz *Constructor Abstracto* para construir y ensamblar las partes del producto.
	- Define la representación a crear.
	- Proporciona una interfaz para devolver el producto. (Un método "ObtenerProducto")
- **Producto**
	- Representa el objeto complejo en construcción. El ConstructorConcreto construye la representación interna del producto y define el proceso de ensamblaje.
	- Incluye las clases que definen sus partes constituyentes, incluyendo interfaces para ensamblar las partes en el resultado final.

### Cómo se relacionan los participantes (!)
- El Cliente crea el objeto Director y lo configura con el objeto Constructor Abstracto deseado.
- El Director notifica al constructor cada vez que hay que construir una parte de un producto.
- El Constructor maneja las peticiones del director y las añade al producto.
- El Cliente obtiene el producto del constructor.

![[Pasted image 20250205103509.png]]

# Factory Method

## Propósito
Define una interfaz para crear un objeto, pero deja que sean las subclases quienes decidan que clase instanciar. Permite que una clase delegue en sus subclases la creación de objetos.

## Aplicabilidad
- Una clase no puede prever la clase de objetos que debe crear.
- Una clase quiere que sean sus subclases quienes especifiquen los objetos que ésta crea. 
- Las clases delegan la responsabilidad en una de entre varias clases auxiliares, y queremos localizar qué subclase de auxiliar concreta es en la que se delega.

## Estructura
- **Producto Abstracto**
	- Define la interfaz de los objetos que crea el método de fabricación.
- **Producto Concreto**
	- Implementa la interfaz *Producto Abstracto*
- **Creador Abstracto**
	- Declara el método de fabricación, el cual devuelve un objeto de tipo *Producto Abstracto*. También puede definir una implementación predeterminada del método de fabricación que devuelva un objeto *Producto Concreto*
- **Creador Concreto**
	- Redefine el método de fabricación para devolver una instancia de un *Producto Concreto.*
### Cómo se relacionan los participantes (!)
- El *Creador Abstracto* se apoya en sus subclases para definir el método de fabricación de manera que éste devuelva una instancia del *Producto Concreto* apropiado.