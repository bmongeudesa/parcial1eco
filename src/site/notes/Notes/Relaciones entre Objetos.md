---
{"dg-publish":true,"permalink":"/notes/relaciones-entre-objetos/","tags":["resumir"],"dg-note-properties":{"subject":["[[Paradigmas de la Programación]]"],"clase":["[[Clase 6 - Herencia]]"],"type":["Concept"],"topics":["[[OOP]]"],"tags":["resumir"]}}
---

Los tipos de relación son las formas en que las clases interactúan y se conectan entre sí para modelar relaciones. 
Hay 6 tipos: 
1. [[Notes/Relaciones entre Objetos#Asociación\|#Asociación]] 
2. [[Notes/Relaciones entre Objetos#Agregación\|#Agregación]] 
3. [[Notes/Relaciones entre Objetos#Composición\|#Composición]] 
4. [[Notes/Relaciones entre Objetos#Generalización\|#Generalización]] 
5. [[Notes/Relaciones entre Objetos#Realización\|#Realización]] 
6. [[Notes/Relaciones entre Objetos#Dependencia\|#Dependencia]] 
Estas relaciones representan conceptos léxicos como “is-a”, “has-a”, “uses-a” y “is-like-a”. Por ejemplo, “Un perro is-a mamífero”, “Un auto has-a motor”, etc. Estas relaciones no tienen que ser uno a uno
![Pasted image 20260331165500.png](/img/user/Attachments/Pasted%20image%2020260331165500.png)
# Asociación 
#resumir
Representa una relación estructural entre dos o más clases cuyos objetos están vinculados entre sí de manera persistente. 
	*"Relacionados pero no dependientes, si se borra uno el otro persiste"*
Esto implica que la relación forma parte del estado de los objetos (por ejemplo, mediante atributos), a diferencia de una dependencia, que es típicamente temporal y se da durante la ejecución de una operación. Esta relación no implica, por sí misma, ownership entre los objetos ni dependencia en sus ciclos de vida. 
Ejemplo: 
	*en un sistema de biblioteca, un Libro y un Autor están asociados. Un libro puede tener uno o más autores (por ejemplo, mediante referencias), y un autor puede haber escrito varios libros. Esta relación se mantiene en el tiempo como parte del modelo del sistema, independientemente de operaciones puntuales. Nótese que aquí el libro no está compuesto por autores, sino que mantiene una relación con ellos para acceder a su información. En esta relación, los objetos pueden interactuar entre sí, pero sus ciclos de vida son independientes.*
# Agregación
#resumir 
Representa una relación del tipo “has-a” entre un objeto que actúa como un todo (whole) y sus partes (parts). En este caso, el todo mantiene una relación con las partes, pero sin una dependencia total: el ciclo de vida de un objeto no depende del otro. 
Es decir, aunque los objetos están más estrechamente relacionados que en una simple asociación, las partes pueden existir de manera independiente del todo. 
**Ejemplo:** 
	*profesores (parts) y un departamento de una universidad (whole). Los profesores pueden ser contratados, administrados económicamente, evaluados, organizados en roles, etc. En cuanto a su lifetime, en el caso de que departamento se cierre, los profesores pueden cambiar de departamento sin dejar de ser profesores. O bien, si renuncian todos los profesores, el departamento deberá de contratar más profesores, pero no dejar de existir. En este caso hay una relación más intima entre los objetos dado que existe una acción entre el departamento y los profesores.* 

# Composición
Representa una relación del tipo “has-a” que es más fuerte que agregación. En este caso, un objeto (whole) tiene ownership sobre otros (parts) y controla sus lifetimes (ciclos de vida). Esto significa que, cuando el objeto contenedor es destruido, sus partes (objetos contenidos) también son eliminadas. 
**Ejemplo:** 
	*una casa (whole) y sus habitaciones (parts). Los cuartos por si solos no tienen sentidos fuera de una casa. Al mismo tiempo, si una casa es destruida, las habitaciones también son destruidas porque son una parte integral de la misma*

# Generalización
Es una relación “is-a” que permite crear, a partir de una clase más abstracta (llamada superclase o clase base), una clase más específica (llamada subclase o clase derivada). La subclase hereda propiedades y métodos de la superclase, los cuales pueden ser modificados y o bien pueden agregar nuevos. 
**Ejemplo:** 
	*tener una clase mamífero que tenga atributos como cantidad de patas, desplazarse, etc. Clases derivadas de estas pueden ser un gato (“is-a” mamífero) y una ballena (“is-a” mamífero). Un objeto gato tiene 4 patas y se desplaza corriendo y el objeto ballena no tiene patas y se desplaza nadando.* 
Herencia y potencialmente polimorfismo permiten la implementación de esta relación.
# Realización
Se refiere a la relación mediante la cual una clase concreta implementa el contrato definido por una interfaz (y, en muchos casos, por una clase abstracta). Es decir, la clase concreta proporciona la implementación de los métodos declarados en dicha abstracción. 
**Ejemplo:** 
	*en un sistema se utilizan clases círculo y rectángulo que requieren la implementación de calcularArea() y calcularPerimetro(). Para asegurar su implementación se utiliza una interfaz o una clase abstracta. Como estas figuras tienen fórmulas distintas para estos métodos, ambas clases deberán proporcionar sus propias implementaciones de estos métodos.* 
Realización está fuertemente relacionada con polimorfismo (tema de la próxima presentación).

# Dependencia
Es una relación en la cual una clase utiliza a otra para cumplir una función específica. Esta relación es generalmente temporal, ya que la clase dependiente no mantiene una referencia persistente al objeto utilizado, sino que lo emplea de forma puntual (por ejemplo, como parámetro o variable local). A diferencia de una asociación, esta relación no forma parte del estado del objeto, sino que se establece únicamente durante la ejecución de una operación.
**Ejemplo:** 
	*en un servicio de correo electrónico, una clase Usuario puede depender de una clase ServicioEmail para enviar un mensaje. En este caso, Usuario utiliza ServicioEmail sin necesidad de conocer su implementación interna. Además, ServicioEmail no forma parte del estado de Usuario, sino que es utilizado únicamente para ejecutar la operación de envío.*