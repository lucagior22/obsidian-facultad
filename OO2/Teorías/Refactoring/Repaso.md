# Proceso de refactoring
Se toma un código con "bad smells" producto de mal diseño y se lo trabaja para obtener un buen diseño. Algunas maneras de trabajar son, moviendo atributos o métodos de una clase a otra, extrayendo código de un método a otro, etc.

# Code Smells
- Duplicate code
	- El mismo o código muy similar, aparece en muchos lugares distintos.
	- Problemas:
		- Código innecesariamente más largo.
		- Es dificil de cambiar, dificil de mantener.
		- Un bug fix en un clón no es fácilmente propagado a los demás duplicados. 
- Large class
	- Mucho trabajo para una sola clase.
	- Muchas variables de instancia.
	- Muchos métodos.
	- Problemas:
		- Baja cohesión (problema de diseño)
- Long method
	- Muchas líneas de código en un método.
	- Problemas:
		- Más líneas implican mayor complejidad en el entendimiento, mantenimiento y reutilización.
- Data class
	- Una clase únicamente tiene variables, getters y setters.
	- Su única función es contener datos.
	- Problemas:
		- Puede implicar que otras clases tienen "Feature envy".
			- Deberían distribuirse correctamente los métodos entre estas clases.
		- Suele indicar que el diseño es procedural.
- Feature envy
	- Un método de una clase usa **principalmente** los datos y métodos de **otra clase** para realizar su trabajo.
	- Problemas:
		- El método "envidioso" fue ubicado en la clase incorrecta. 
- Long parameter list
	- Un método con una lista larga de parámetros.
	- Problemas:
		- Dificil reutilización debido a la complejidad de obtener todos los parámetros para pasarlos en la llamada.
		- Dificil de entender.
- Switch statements
	- Sentencias condicionales contienen lógica para diferentes tipos de objetos.
	- Todos los objetos son instancias de la misma clase, eso indica que se necesitan crear subclases.
	- Problemas:
		- La misma estructura condicional aparece en muchos lugares.

# Organización del catálogo
- **Composición de métodos**
	- Permiten distribuir el código adecuadamente.
	- Métodos largos son problemáticos.
	- Refactorings:
		- *Extract Method* *
		- *Replace Temp With Query* *
		- *Inline Method*
		- *Split Temporary Variable*
		- *Replace Method With Method Object*
		- *Substitute Algorithm*
- **Mover aspectos entre objetos**
	- Ayudan a mejorar la asignación de responsabilidades.
	- Refactorings:
		- *Move Method* *
		- *Move Field*
		- *Extract Class*
		- *Inline Class*
		- *Remove Middle Man*
		- *Hide Delegate*
- **Organización de datos**
	- Facilitan la organización de atributos.
	- Refactorings:
		- *Self Encapsulate Field*
		- *Encapsulate Field/Collection*
		- *Replace Data Value With Object*
		- *Replace Array With Object*
		- *Replace Magic Number With Symbolic Constant*
- **Simplificación de expresiones condicionales**
	- Ayudan a simplificar los condicionales.
	- Refactorings:
		- *Replace Conditional With Polimorfism* *
		- *Decompose Conditional*
		- Consolidate Conditional Expression
		- *Consolidate Duplicate Conditional Fragments*
- **Simplificación en la invocación de métodos**
	- Sirven para mejorar la interfaz de una clase.
	- Refactorings:
		- *Rename Method* *
		- *Preserve Whole Object*
		- *Introduce Parameter Object*
		- *Parametrize Method*
- **Manipulación de la generalización**
	- Ayudan a mejorar las jerarquías de clases.
	- Refactorings:
		- *Pull Up / Push Down Field*
		- *Pull Up / Push Down Method*
		- *Pull Up Constructr Body*
		- *Extract Subclass / Superclass*
		- *Collapse Hierarchy*
		- *Replace Inheritance With Delegation*
- **Big refactorings**

# Catálogo <-> Code Smells
### Código Duplicado
- Extract Method
- Pull Up Method
- Form Template Method
### Métodos Largos
- Extract Method
- Decompose Conditional
- Replace Temp With Query
### Clases Grandes
- Extract Class
- Extract Subclass
### Muchos parámetros
- Replace Parameter with Method
- Preserve Whole Object
- Introduce Parameter Object
### Cambios Divergentes
- Extract Class
### Shotgun Surgery
- Move Method / Field
### Envidia de Atributo
- Move Method
### Data Class
- Move Method
### Sentencias Switch
- Replace Conditional With Polymorphism
### Generalidad Especulativa
- Collapse Hierarchy
- Inline Class
- Remove Parameter
### Cadena de Mensajes
- Hide Delegate
- Extract Method / Move Method
### Middle Man
- Remove Middle Man
### Inappropiate Intimacy
- Move Method / Field
### Legado Rechazado
- Push Down Method / Field
### Comentarios
- Extract Method
- Rename Method


# Catálogo

## Pull Up Method
1. Asegurarse que los métodos sean idénticos, si no, parametrizar.
2. Si el selector del método es diferente en cada subclase, renombrar.
3. Si el método llama a otro que no está en la superclase, declararlo como abstracto en la superclase.
4. Si el método llama a un atributo declarado en las subclases, utilizar "Pull Up Field" o "Self Encapsulate Field" y declarar los getters abstractos en la superclase.
5. Crear un nuevo método en la superclase, copiar el cuerpo de uno de los métodos a él, ajustar, compilar.
6. Borrar el método de una de las subclases.
7. Compilar y testear.
8. Repetir desde el **6** hasta que no quede en ninguna subclase.