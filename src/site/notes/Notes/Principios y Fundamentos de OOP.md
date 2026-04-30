---
{"dg-publish":true,"permalink":"/notes/principios-y-fundamentos-de-oop/","dg-note-properties":{"subject":["[[Paradigmas de la Programación]]"],"clase":["[[Clase 4 - Intro a OOP]]"],"type":["Concept"],"topics":["[[C++]]","[[OOP]]"],"tags":null}}
---

	# Conceptos Fundamentales
## Abstracción
Acto de identificar caracteristicas principales de un objeto para resolver un problema, mientras se **ignoran** detalles irrelevantes.
	Se usa en OOP para simplificar la interacción con sistemas complejos, y permite enfocarse en lo que hace el objeto y no como lo hace
## Encapsulamiento
Agrupación de acciones y datos que operan en una clase, las cuales cumplen con ciertas restricciones de acceso (ocultación de datos o data hiding). 
Este concepto protege el estado interno de un objeto de mal o indebido uso del mismo. 
	El manejo de estados internos se realizan con funciones método denominadas “**setter**” y “**getter**”.
## [[Notes/Herencia\|Herencia]]
Mecanismo por el cual una nueva clase (clase derivada o subclase) puede heredar atributos y métodos de una clase ya existente (clase base o superclase). 
La clase derivada puede extender o modificar el comportamiento de la clase base. 
	De esta forma, se obtiene **reusabilidad de código**, creándose una jerarquía de clases.
## [[Notes/Polimorfismo\|Polimorfismo]]
Capacidad de las clases para tomar distintas formas mediante el uso de una interfaz común que permite que objetos de diferentes clases sean tratados en forma similar. 
	Esto permite que el código sea aún más **flexible** y **mantenible**.
# Implementación
Se implementan estos conceptos mediante:
- **Atributos**: estados o propiedades de un objeto
- **Metodos**: acciones o comportamientos del objeto
# Otros Conceptos
Ademas de los pilares de OOP, existen criterios que permiten evaluar si un diseño es correcto, mantenible y escalable:
- ### Alta Cohesión
	Cada clase tiene una responsabilidad clara
- ### Bajo Acoplamiento
	Las clases dependen lo menos posible entre sí
- ### Evitar duplicacion de codigo
	Una misma funcionalidad debe definirse en un único lugar 
- ### Bajo impacto ante cambios futuros
	Puede evolucionar sin reescrituras masivas
## Cohesión
Qué tan relacionadas están entre sí las responsabilidades de una clase
- **Alta cohesión:** clases simples, claras y fáciles de mantener. 
- **Baja cohesión:** clases que hacen “muchas cosas”, difíciles de entender y modificar.
## Acoplamiento
Mide el grado de dependencia entre clases:
- **Bajo acoplamiento:** una clase conoce lo mínimo necesario de otras clases. 
- **Alto acoplamiento:** cambios en una clase obligan a modificar muchas otras.
## Duplicacion de Codigo
Ocurre cuando el mismo comportamiento o lógica aparece repetida en varios lugares del programa. 
- Aumenta el tamaño del código. 
- Dificulta el mantenimiento. 
- Incrementa la probabilidad de errores inconsistentes. 
Herencia, composición y funciones bien diseñadas ayudan a evitar duplicaciones
## Impacto ante cambios futuros
Qué tan costoso es modificar el sistema cuando cambian los requisitos. Un buen diseño orientado a objetos: 
- Localiza los cambios en pocas clases, 
- Minimiza efectos colaterales, 
- Facilita extender el sistema sin reescribir código existente. 
Está directamente relacionado con: 
- Bajo acoplamiento, 
- Alta cohesión, 
- Uso adecuado de herencia y polimorfismo.
# Principios SOLID
*no aplicadas en el curso, referencias para estudios posteriores en software*
- ## (S) Responsabilidad Unica
	Una sola razón para cambiar
- ## (O) Abierto/Cerrado
	Comportamiento modificable sin necesidad modificar codigo existente
- ## (L) Sustitucion de Liskov
	Clases derivadas deben poder reemplazar la base sin alterar el comportamiento esperado
- ## (I) Segregacion de Interfaces
	Preferible utilizar interfaces **pequeñas y especificas** ante grandes y rigidas
- ## (D) Inversion de dependencias
	Clases de alto nivel no deben depender de clases concretas sino de **abstracciones**