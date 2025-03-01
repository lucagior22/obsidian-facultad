
---

**Pregunta 1**

**Usamos el patrón Template Method cuando**

Seleccione una:  
a. Queremos escribir las partes invariantes de un algoritmo una vez y diferir las variantes para cada posible sub-clase  
b. Cuando tenemos muchos métodos de nombres diferentes pero que tienen el mismo código  
c. Para mejorar la forma en la que usamos la herencia con `this` y `super`

**Respuesta correcta:** a. Queremos escribir las partes invariantes de un algoritmo una vez y diferir las variantes para cada posible sub-clase

---

**Pregunta 2**

**Un patrón de diseño de software**

Seleccione una:  
a. Describe un problema recurrente y su solución de manera de poder ser re-utilizada muchas veces  
b. Es una jerarquía de clases abstractas y concretas que deben ser instanciadas para ser usadas  
c. Es la solución a un problema muy complejo

**Respuesta correcta:** a. Describe un problema recurrente y su solución de manera de poder ser re-utilizada muchas veces

---

**Pregunta 3**

**Supongamos una clase abstracta A con subclases B, C, D y supongamos un template method t() en A con el siguiente código:**

```java
public void t() {  
  o1();  
  o2();  
  o3();  
  t1();  
}
```

**Supongamos que t1() es un método que expresa una funcionalidad que no va a variar en cada subclase (a diferencia de o1(), o2() y o3())**

Seleccione una:  
a. Las subclases tienen que incluir algún código adicional (por ejemplo usando `super`) para resolver la situación  
b. El template method está mal definido y requerirá edición  
c. El método `t1()` puede estar definido solo en A y se invocará naturalmente como `t1()` - o `this.t1()`

**Respuesta correcta:** c. El método `t1()` puede estar definido solo en A y se invocará naturalmente como `t1()` - o `this.t1()`

---

**Pregunta 4**

**Usamos el patrón Composite**

Seleccione una:  
a. Cuando queremos representar estructuras parte-todo de objetos a los cuales queremos tratar polimórficamente.  
b. Cuando tenemos relaciones jerárquicas como las que existen en una organización (director, jefe, empleado)  
c. Para representar un tipo de herencia específica entre el todo y las partes que permita que las partes hereden del todo

**Respuesta correcta:** a. Cuando queremos representar estructuras parte-todo de objetos a los cuales queremos tratar polimórficamente.

---

**Pregunta 5**

**Usando patrones conseguimos**

Seleccione una:  
a. Programas más eficientes  
b. Diseños más flexibles, extensibles y reusables  
c. Reducir el número de clases y métodos en nuestro sistema

**Respuesta correcta:** b. Diseños más flexibles, extensibles y reusables

---

**Pregunta 6**

**Supongamos que queremos crear objetos adaptadores ad (de la clase AD) para permitir que instancias a (de la clase A) puedan interactuar (enviándole mensajes) con instancias c (de la clase C) dado que el protocolo (mensajes que entienden las instancias) de C no es compatible con lo que requiere A.**

**Para conseguir el comportamiento descrito en el patrón Adapter necesitamos que...**

Seleccione una:  
a. Los objetos `ad` de `AD` puedan interactuar con objetos `a` de `A` para poder resolver los requerimientos de los objetos `c` de `C`  
b. Un objeto `a` de `A` conozca una instancia `ad` de `AD` la cual a su vez conozca la instancia `c` de `C` con la cual se comunica  
c. El objeto `c` de `C` con el cual necesitamos interactuar conozca una instancia `ad` de `AD` para realizar la adaptación

**Respuesta correcta:** b. Un objeto `a` de `A` conozca una instancia `ad` de `AD` la cual a su vez conozca la instancia `c` de `C` con la cual se comunica

---
**Pregunta 7**

**Usamos el Patrón Strategy porque**

Seleccione una:  
a. Tenemos una jerarquía de clases con múltiples implementaciones de un método en las sub-clases y queremos usar un template method  
b. Tenemos un método que puede tener múltiples implementaciones y no queremos tomar la decisión de cuál usar con una estructura IF  
c. Hay varios comportamientos diferentes en un objeto y queremos poder seleccionar correctamente cuál usar

**Respuesta correcta:** b. Tenemos un método que puede tener múltiples implementaciones y no queremos tomar la decisión de cuál usar con una estructura IF

---

**Pregunta 8**

**Elija la respuesta correcta con respecto a Proxy.**

Seleccione una:  
a. El propósito de Proxy es agregar responsabilidades a un objeto dinámicamente.  
b. El Proxy virtual se utiliza para representar objetos que se encuentran en una base de datos.  
c. El Proxy de protección le delega al objeto real la responsabilidad de chequear si el cliente tiene los permisos necesarios para accederlo.

**Respuesta correcta:** b. El Proxy virtual se utiliza para representar objetos que se encuentran en una base de datos.

---

**Pregunta 9**

**Cuando usamos el State**

Seleccione una:  
a. Utiliza un template method que en las sub-clases determina cuál comportamiento se debe ejecutar  
b. El objeto contexto (que recibe los mensajes) los delega en su estado actual que es quien tiene el comportamiento correspondiente  
c. Chequea si en el estado actual puede o no responder dicho mensaje

**Respuesta correcta:** b. El objeto contexto (que recibe los mensajes) los delega en su estado actual que es quien tiene el comportamiento correspondiente

---

**Pregunta 10**

**Elija la respuesta correcta con respecto a Decorator.**

Seleccione una:  
a. El propósito del patrón Decorator es generar una composición de objetos.  
b. Decorador y Decorado pertenecen a distintas jerarquías de clase.  
c. El patrón Decorator permite tener mayor flexibilidad en tiempo de ejecución que una solución con herencia

**Respuesta correcta:** c. El patrón Decorator permite tener mayor flexibilidad en tiempo de ejecución que una solución con herencia

---

**Pregunta 11**

**Elija la respuesta correcta con respecto a Proxy.**

Seleccione una:  
a. El Proxy remoto se utiliza para representar objetos que se encuentran en una base de datos.  
b. El Proxy virtual se utiliza para representar objetos en una arquitectura distribuida.  
c. El propósito de Proxy es proporcionar un intermediario para controlar el acceso al objeto real.

**Respuesta correcta:** c. El propósito de Proxy es proporcionar un intermediario para controlar el acceso al objeto real.

---

**Pregunta 12**

**Elija la respuesta correcta con respecto a Proxy.**

Seleccione una:  
a. El Proxy de protección le pregunta al objeto real si el cliente tiene los permisos necesarios para accederlo.  
b. El Proxy de protección se puede ver como un Decorator.  
c. El propósito de Proxy es proporcionar un intermediario que traduzca el protocolo que entiende el objeto real.

**Respuesta correcta:** b. El Proxy de protección se puede ver como un Decorator.

**Aclaración**: *En este caso, el **Proxy de protección actúa como un envoltorio** que decide si se permite o no una acción sobre el objeto real. Esto es similar a cómo un **Decorator** añade funcionalidades adicionales sin modificar la clase base, por lo que es válido decir que **se puede ver como un Decorator**.*

---

**Pregunta 13**

**El smell “Feature Envy” (Envidia de Atributo) marca un problema de:**

Seleccione una:  
a. Mala asignación de responsabilidades.  
b. Mal uso de variables de instancia y temporales.  
c. Mala jerarquía de herencia (envidia entre clase y subclase).

**Respuesta correcta:** a. Mala asignación de responsabilidades.

---

**Pregunta 14**

**Los “bad smells” son:**

Seleccione una:  
a. Bugs, es decir, errores en el código.  
b. Errores de estimación en el tiempo de finalización del desarrollo de una funcionalidad.  
c. Indicios de duplicación de código.  
d. Indicios de problemas de diseño que requieren la aplicación de refactoring.

**Respuesta correcta:** d. Indicios de problemas de diseño que requieren la aplicación de refactoring.

---

**Pregunta 15**

**El refactoring Extract Method permite:**

Seleccione una:  
a. Crear un método más reusable.  
b. Mover métodos en una jerarquía.  
c. Reasignar responsabilidades entre clases.

**Respuesta correcta:** a. Crear un método más reusable.

---

**Pregunta 16**

**Las herramientas de refactoring:**

Seleccione una:  
a. Realizan las transformaciones sin ningún tipo de chequeo previo, dejando el chequeo en manos del programador.  
b. Miden la deuda técnica que significa la cantidad de refactorings aplicados.  
c. Chequean la semántica del programa antes de aplicar refactoring, por ejemplo, haciendo análisis de posibles valores de parámetros.  
d. Chequean precondiciones con el análisis del árbol de sintaxis y luego aplican los cambios.

**Respuesta correcta:** d. Chequean precondiciones con el análisis del árbol de sintaxis y luego aplican los cambios.

---

**Pregunta 17**

**En cuál de estos casos usarías test doubles:**

Seleccione una:  
a. Para no duplicar el código que crea los objetos del fixture de un test.  
b. Para no acarrear errores del objeto que provee los datos de entrada del método a testear.  
c. Para no acarrear errores en la superclase de la clase a testear.

**Respuesta correcta:** b. Para no acarrear errores del objeto que provee los datos de entrada del método a testear.

---

**Pregunta 18**

**A qué nos referimos cuando decimos que el refactoring nos permite aprender del feedback:**

Seleccione una:  
a. Permite diseñar en forma incremental, partiendo de un diseño simple y esperando ver cómo funciona en su contexto antes de seguir avanzando.  
b. Permite diseñar el sistema completo con toda la complejidad que haga falta al principio y luego modificarlo según cómo funciona.

**Respuesta correcta:** a. Permite diseñar en forma incremental, partiendo de un diseño simple y esperando ver cómo funciona en su contexto antes de seguir avanzando.

---

**Pregunta 19**

**Elija la respuesta correcta con respecto a refactoring hacia patrones**

Seleccione una:  
a. La sobre-ingeniería es preferible y recomendable sobre la baja o poca ingeniería  
b. La sobre-ingeniería, es decir, tener un diseño sofisticado y complejo, es tan peligrosa como la baja-ingeniería, es decir, tener un diseño pobre.

**Respuesta correcta:** b. La sobre-ingeniería, es decir, tener un diseño sofisticado y complejo, es tan peligrosa como la baja-ingeniería, es decir, tener un diseño pobre.

---

**Pregunta 20**

**El smell “Shotgun Surgery” causa que:**

Seleccione una:  
a. La misma clase se modifica ante distintos cambios de requerimientos de diferentes maneras y por diferentes razones.  
b. Una clase tenga baja cohesión.  
c. Ante un cambio de requerimientos debo modificar código en muchas clases.

**Respuesta correcta:** c. Ante un cambio de requerimientos debo modificar código en muchas clases.

---

**Pregunta 21**

**Seleccione la afirmación cierta**

Seleccione una:  
a. Todas las aplicaciones construidas con un mismo framework tienen aspectos en común que no podemos cambiar (esos aspectos constituyen el Frozenspot)  
b. Cada clase en un framework resuelve un problema concreto, es independiente del contexto de uso, no espera nada de nuestro código y es generalmente independiente de otras clases en el framework.  
c. Una librería de clases es una aplicación “semicompleta”, “reusable”, que puede ser especializada para producir aplicaciones a medida.

**Respuesta correcta:** a. Todas las aplicaciones construidas con un mismo framework tienen aspectos en común que no podemos cambiar (esos aspectos constituyen el Frozenspot)

---

**Pregunta 22**

**Seleccione la afirmación cierta**

Seleccione una:  
a. Un framework es un conjunto de clases concretas y abstractas, relacionadas para proveer una arquitectura reusable para una familia de aplicaciones relacionadas.  
b. Una librería de clases es un conjunto de clases concretas y abstractas, relacionadas para proveer una arquitectura reusable para una familia de aplicaciones relacionadas.  
c. Una librería de clases es una aplicación “semicompleta”, “reusable”, que puede ser especializada para producir aplicaciones a medida.

**Respuesta correcta:** a. Un framework es un conjunto de clases concretas y abstractas, relacionadas para proveer una arquitectura reusable para una familia de aplicaciones relacionadas.

---

**Pregunta 23**

**Seleccione la afirmación cierta**

Seleccione una:  
a. Cuando utilizamos frameworks, nuestro código controla/usa a los objetos del framework (llamamos a esto flujo del control)  
b. Cada clase en una librería resuelve un problema concreto, es independiente del contexto de uso, no espera nada de nuestro código y es generalmente independiente de otras clases en la librería  
c. Los frameworks orientados a objetos resuelven problemas comunes a la mayoría de las aplicaciones (p.e., manejo de archivos, funciones aritméticas, colecciones, fechas)  
d. Cada clase en un framework resuelve un problema concreto, es independiente del contexto de uso, no espera nada de nuestro código y es generalmente independiente de otras clases en el framework.

**Respuesta correcta:** b. Cada clase en una librería resuelve un problema concreto, es independiente del contexto de uso, no espera nada de nuestro código y es generalmente independiente de otras clases en la librería

---

**Pregunta 24**

**Elija la respuesta correcta**

Seleccione una:  
a. Cuando ya tengo código existente, el problema que soluciona Template Method es el relacionado con el smell “Pull Up Method”.  
b. Cuando ya tengo código existente, el problema que soluciona Template Method es el relacionado con el smell “Data Class” o Clase de datos.  
c. Cuando ya tengo código existente, el problema que soluciona Template Method es el relacionado con el smell “Duplicate Code” o Código duplicado.

**Respuesta correcta:** c. Cuando ya tengo código existente, el problema que soluciona Template Method es el relacionado con el smell “Duplicate Code” o Código duplicado.

---

**Pregunta 25**

**Seleccione la afirmación cierta**

Seleccione una:  
a. Con herencia en la implementación de plantillas y ganchos se evita la duplicación de código y el creciente número de clases cuando existen muchas alternativas y combinaciones posibles  
b. Si utilizo herencia, al implementar los métodos gancho debo pasar como parámetro todo lo que necesiten - no tienen acceso a las variables de instancia definidas en la clase que implementa la plantilla.  
c. El patrón método plantilla (implementado como se describe en el libro de patrones) es la única forma de introducir variabilidad en un framework, y por eso decimos que es un generador de hotspots e inversión de control.  
d. Las plantillas y ganchos pueden implementarse utilizando herencia o composición como mecanismos para separar los aspectos comunes de los aspectos variables en un framework.

**Respuesta correcta:** d. Las plantillas y ganchos pueden implementarse utilizando herencia o composición como mecanismos para separar los aspectos comunes de los aspectos variables en un framework.

---

**Pregunta 26**

**Seleccione la afirmación cierta**

Seleccione una:  
a. Si uso herencia, la plantilla se implementa en una clase abstracta y los ganchos en sus subclases (como el clásico patrón Método Plantilla). Si uso composición, un objeto implementa la plantilla y delega la implementación de los ganchos en sus partes.  
b. Si se implementa plantillas y ganchos por composición, NO es posible (o es muy complejo) cambiar en tiempo de ejecución el comportamiento implementado por los ganchos. Si se implementa por herencia, SI es posible.  
c. El patrón método plantilla (implementado como se describe en el libro de patrones) es la única forma de introducir variabilidad en un framework, y por eso decimos que es un generador de hotspots e inversión de control.  
d. Cuando hablamos de implementar plantillas y ganchos por composición, nos referimos a combinar el patrón template method con el patrón composite.

**Respuesta correcta:** a. Si uso herencia, la plantilla se implementa en una clase abstracta y los ganchos en sus subclases (como el clásico patrón Método Plantilla). Si uso composición, un objeto implementa la plantilla y delega la implementación de los ganchos en sus partes.

---

**Pregunta 27**

**La integración continua implica ...**

Seleccione una:  
a. Guardar los cambios del código en un repositorio central en forma frecuente.  
b. Desplegar en producción una nueva versión de la aplicación cada vez que se modifica el código.  
c. Administrar la infraestructura utilizando técnicas de código.

**Respuesta correcta:** a. Guardar los cambios del código en un repositorio central en forma frecuente.

---

**Pregunta 28**

**El capital de la deuda técnica es ...**

Seleccione una:  
a. El costo de implementar nueva funcionalidad  
b. El costo de implementar la solución correcta  
c. El costo de mantener el sistema en el estado actual

**Respuesta correcta:** b. El costo de implementar la solución correcta

---

**Pregunta 29**

**Las actividades para administrar la deuda técnica son ...**

Seleccione una:  
a. Priorizar, Identificar, Testear, Pagar  
b. Priorizar, Identificar, Medir, Pagar  
c. Identificar, Testear, Medir, Pagar

**Respuesta correcta:** b. Priorizar, Identificar, Medir, Pagar

---

**Pregunta 30**

**Seleccione la afirmación cierta**

Seleccione una:  
a. Si se implementa plantillas y ganchos por composición, NO es posible (o es muy complejo) cambiar en tiempo de ejecución el comportamiento implementado por los ganchos. Si se implementa por herencia, SI es posible.  
b. El patrón método plantilla (implementado como se describe en el libro de patrones) es la única forma de introducir variabilidad en un framework, y por eso decimos que es un generador de hotspots e inversión de control.  
c. Con herencia en la implementación de plantillas y ganchos se evita la duplicación de código y el creciente número de clases cuando existen muchas alternativas y combinaciones posibles  
d. Si implemento plantillas y ganchos aplicando herencia, al implementar los métodos gancho puedo utilizar las variables de instancia y todo el comportamiento heredado de la clase abstracta (donde se implementa la plantilla)

**Respuesta correcta:** d. Si implemento plantillas y ganchos aplicando herencia, al implementar los métodos gancho puedo utilizar las variables de instancia y todo el comportamiento heredado de la clase abstracta (donde se implementa la plantilla)

---

**Pregunta 31**

**Nos referimos con el término "protocolo de una clase" al conjunto de mensajes que entienden sus instancias. Con eso en mente, el patrón de diseño Adapter...**

Seleccione una:  
a. Permite que enriquezcamos el protocolo de una clase con nueva funcionalidad  
b. Se enfoca en el problema de adaptación de protocolos incompatibles  
c. Nos provee objetos reusables que podemos usar para adaptar protocolos

**Respuesta correcta:** b. Se enfoca en el problema de adaptación de protocolos incompatibles

---

**Pregunta 32**

**Usamos un patrón específico del catálogo del libro de Gamma...**

Seleccione una:  
a. Cuando en nuestro problema aparece un requerimiento cuyo nombre coincide exactamente con el nombre del patrón  
b. Cuando la jerarquía de clases en nuestra aplicación se parece mucho a la estructura del patrón  
c. Cuando la naturaleza de nuestro problema de diseño coincide con la aplicabilidad del patrón

**Respuesta correcta:** c. Cuando la naturaleza de nuestro problema de diseño coincide con la aplicabilidad del patrón

---

**Pregunta 33**

**Cuando usamos Composite y un objeto compuesto c recibe un mensaje x, lo delega en sus partes (c1, c2, c3, c4). Si alguna de ellas (por ejemplo, c1) es un objeto compuesto, lo delega a su vez en sus partes c11, c12, c13... Esto es posible porque:**

Seleccione una:  
a. Porque chequeamos para cada parte c1 si es compuesto o atómico  
b. Un Composite puede estar formado por Composites u objetos atómicos indistintamente  
c. Porque escribimos código recursivo en la clase Composite

**Respuesta correcta:** b. Un Composite puede estar formado por Composites u objetos atómicos indistintamente

---

**Pregunta 34**

**Supongamos que estamos usando el patrón Composite para representar objetos f de la clase F (por ejemplo, Figura) y un cliente e (un Editor) se comunica con los f enviando mensajes f1, f2, etc.**

Seleccione una:  
a. El receptor del mensaje tiene que chequear a qué tipo de clase pertenece para saber si el mensaje debe ser delegado a sus partes o no  
b. El editor chequea si el receptor es atómico o compuesto para saber qué mensajes mandar  
c. El editor envía los mensajes f1, f2 sin preocuparse por el receptor

**Respuesta correcta:** c. El editor envía los mensajes f1, f2 sin preocuparse por el receptor

---

**Pregunta 35**

**Cuando usamos el Patrón State y debe cambiarse de estado...**

Seleccione una:  
a. El contexto determina cuál es su nuevo estado en función del método que acaba de recibir  
b. El estado "actual" le avisa al "nuevo" estado que ahora él pasa a ser el nuevo  
c. El método (en el estado actual) que "sabe" que debe cambiarse de estado le avisa al contexto cuál es su "nuevo" estado

**Respuesta correcta:** c. El método (en el estado actual) que "sabe" que debe cambiarse de estado le avisa al contexto cuál es su "nuevo" estado

---

**Pregunta 36**

**Las partes de un test de unidad son:**

Seleccione una:  
a. Ejercitar la unidad y chequear.  
b. Crear el fixture, ejercitarlo, comprobar el resultado y eliminar el fixture.  
c. Escribir el test, codificar lo necesario para que el test pase y refactorizar.  
d. Crear el fixture y comprobar el resultado de la creación.

**Respuesta correcta:** b. Crear el fixture, ejercitarlo, comprobar el resultado y eliminar el fixture.

---

**Pregunta 37**

**Elija la opción correcta con respecto a refactoring:**

Seleccione una:  
a. Permite deshacerse de una jerarquía mal diseñada.  
b. Permite eliminar bugs en el código.  
c. Permite agregar nueva funcionalidad en forma ordenada.

**Respuesta correcta:** a. Permite deshacerse de una jerarquía mal diseñada.

---

**Pregunta 38**

**Elija la opción correcta con respecto a tests de unidad:**

Seleccione una:  
a. El objetivo de testear es llegar a una cobertura suficiente que me permita encontrar bugs en los artefactos más riesgosos.  
b. El objetivo de testear es llegar a una cobertura del 100% que incluya todos los posibles caminos de ejecución.  
c. Un test de unidad debe contener un solo assert.

**Respuesta correcta:** a. El objetivo de testear es llegar a una cobertura suficiente que me permita encontrar bugs en los artefactos más riesgosos.

---

**Pregunta 39**

**Las consecuencias de aplicar el refactoring Form Template Method son:**

Seleccione una:  
a. Elimina el código duplicado pero puede causar la creación de muchos métodos vacíos o con comportamiento por defecto forzados por el Template Method  
b. Elimina el código duplicado pero puede causar Data class.  
c. Todas positivas al eliminar la duplicación; no tiene contras.

**Respuesta correcta:** a. Elimina el código duplicado pero puede causar la creación de muchos métodos vacíos o con comportamiento por defecto forzados por el Template Method

---

**Pregunta 40**

**La "Estructura" de un patrón según la forma de descripción del libro de Gamma y que habitualmente mostramos en UML...**

Seleccione una:  
a. Nos muestra los roles que cumplen las diferentes clases (y objetos) que participan en el patrón  
b. Debe ser copiada y pegada en nuestro sistema para poder usarla  
c. Contiene clases que debemos instanciar para poder usar el patrón en ejemplos concretos

**Respuesta correcta:** a. Nos muestra los roles que cumplen las diferentes clases (y objetos) que participan en el patrón