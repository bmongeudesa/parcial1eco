---
{"dg-publish":true,"permalink":"/notes/introduccion-a-oop/","dg-note-properties":{"subject":["[[Paradigmas de la Programación]]"],"clase":["[[OOP]]"],"type":["Concept"],"topics":["[[C++]]","[[OOP]]"],"tags":null}}
---

# Structs en C++
En C++ las **structs** poseen mas funciones que en C:

![Pasted image 20260326081405.png](/img/user/Attachments/Pasted%20image%2020260326081405.png)
- Se usan para agrupar datos y crear estructuras abstractas
- Se recomienda el uso de **using** en vez de *typedef* para definir alias

### Ejemplos
Modela un rectangulo con atributos length y width
``` cpp
#include <iostream> 
struct Rectangle { 
	// Variables 
	int length, width; 
	// Funccion miembro que calcula el area 
	int area() { 
		return length * width;
	} 
	// Funccion miembro que calcula el perimetro 
	int perimetro() { 
		return 2 * (length + width); 
	} 
};
```
``` cpp
int main() { 
Rectangle rect; 
rect.length = 5; rect.width = 3; 
Rectangle rect2{3,4}; // Otra inicialización del rectángulo 

// Muestra el area y el perimetro del rectangulo 
std::cout << "Para el segundo rectángulo: " << std::endl; 
std::cout << "Area: " << rect.area() << std::endl; // 15 
std::cout << "Perimetro: " << rect.perimetro() << std::endl; // 16 
std::cout << "Para el segundo rectángulo: " << std::endl; 
std::cout << "Area: " << rect.area() << std::endl; // 12 
std::cout << "Perimetro: " << rect.perimetro() << std::endl; // 14 

return 0;
}
```
## *using*
- using se puede usar con [[Templates\|Templates]] y typedef no
- using es mas legible que typedef
![Pasted image 20260326082430.png](/img/user/Attachments/Pasted%20image%2020260326082430.png) ![Pasted image 20260326082449.png](/img/user/Attachments/Pasted%20image%2020260326082449.png)
# OOP vs Programación Procedural
## Procedural
Los **lenguajes procedurales** (como C), orientados a que el programa indique cada una de sus acciones
- las funciones y procedimientos son las unidades de programacion
- los datos poseen un scope global o de función

## Orientada a Objetos
En el caso de C++, lenguaje **multi-paradigma**, incluye programacion orientada a objetos
- datos e informacion encapsulados en paquetes denominados **clases**
- las clases son **unidades de programacion**, al ser nistanciadas se convierten en **objetos**
