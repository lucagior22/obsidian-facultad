## ¿Que son los patrones de diseño?

Cada patrón describe un problema que ocurre una y otra vez en nuestro entorno, así como la solución a ese problema, de tal modo que se pueda aplicar esta solución un millón de veces, sin hacer lo mismo dos veces.

## Descripción de los patrones de diseño (componentes)
- **Nombre del patrón y clasificación**
- **Propósito**:
	- Frase breve que responde a:
		- ¿Qué hace?
		- ¿En qué se basa?
		- Cuál es el problema concreto de diseño que resuelve?
- **Motivación**:
	- Un escenario donde se describe el problema y como el patrón aporta una solución.
- **Aplicabilidad**:
	- Situaciones en las cuales es favorable utilizar un patrón de diseño y como reconocerlas.
- **Participantes**

## Patrones y cómo se relacionan

|Patrón|Creacional|Estructural|Comportamental|Relación|
|---|---|---|---|---|
|**Factory Method**|✅|❌|❌|Relacionado con Builder, Adapter y Composite en la creación de objetos|
|**Builder**|✅|❌|❌|Complementa a Factory Method en la construcción de objetos complejos|
|**Adapter**|❌|✅|❌|Puede ser combinado con Factory Method, Composite y Proxy|
|**Composite**|❌|✅|❌|Relacionado con Factory Method, Adapter y Proxy para manejar estructuras jerárquicas|
|**Proxy**|❌|✅|❌|Se relaciona con Composite, Adapter y Factory Method para controlar el acceso a objetos|
|**State**|❌|❌|✅|Relacionado con Strategy y Template Method en la gestión de comportamientos dinámicos|
|**Strategy**|❌|❌|✅|Similar a State, pero enfocado en la variación de algoritmos en lugar de estados|
|**Template Method**|❌|❌|✅|Puede usar Strategy o State para implementar pasos flexibles en algoritmos|