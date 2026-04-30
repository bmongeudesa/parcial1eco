---
{"dg-publish":true,"permalink":"/notes/copia-de-objetos/","dg-note-properties":{"subject":["[[Paradigmas de la Programación]]"],"clase":["[[Clase 5 - Sobrecarga de Operadores]]"],"type":["Concept"],"topics":["Memoria","[[OOP]]"],"tags":null}}
---

# Shallow Copy
Si el objeto solo contiene tipos primitivos, podemos copiar simplemente con `=`. A este tipo de copia se la denomina **shallow copy**
``` cpp
myClass c1(3,5);
myClass c2;
c2 = c1
```
Solo funciona si no se tiene memoria alocada dinamicamente dentro del objeto. 
	Si no, se copia el puntero y genera problemas de doble borrado o a [[dangling pointers\|dangling pointers]]. 
	Para estos casos de memoria alocada dinamicamente se debe realizar una [[Notes/Copia de Objetos#Deep Copy\|#Deep Copy]]
# Deep Copy
Crea un nuevo objeto con su propia memoria, copiando el contenido en lugar de la dirección de los punteros.

Permiten copiar objetos con memoria alocada dinamicamente. Esto da como resultado un nuevo objeto con memoria alocada en una posicion distinta.

1. Crear una Shallow Copy
2. Crear una Deep Copy con [[Raw Pointers\|Raw Pointers]]
3. Crear una Deep Copy con [[Notes/Punteros y Referencias\|Punteros y Referencias]]

*el uso de smart pointers permite gestionar la copia de manera más segura*


# Copy Constructors
Es un constructor especial de C++ que crea un objeto como copia de un objeto original, de la siguiente manera:
``` cpp
MyClass obj1(10);
MyClass obj2 = obj1;
```
En este caso no es necesario sobrecargar el operador `=`.
## Shallow Copy
![Pasted image 20260331145703.png](/img/user/Attachments/Pasted%20image%2020260331145703.png)
### Ejemplo
Este ejemplo ilustra la necesidad de un Deep Copy y demuestra que no es necesario usar un constructor para Shallow Copy.
	Al salir del scope los 2 punteros se hace un doble`delete`, lo cual ==causa un error a nivel ejecución==
``` cpp
class Engine { 
public: 
	Engine(int power) : power(power) {} 
	int getPower() const {return power; } 
private: 
	int power; }; 


class Plane { 
public: 
	// Constructor tipo Shallow Copy 
	Plane(int power) : engine(new Engine(power)) {} 
	// Constructor que copia el puntero 
	Plane(const Plane& rht) : engine(rht.engine) {} 
	
	void display() const { 
		std::cout << "Engine Power: " 
		<< engine->getPower() 
		<< "\n"; 
	}

	~Plane() { 
		delete engine; 
		// Se borraran p1 y p2. Al borrar p2 se producirá 
		//un error porque el puntero a Engine ya se liberó 
		//al llamar al destructor de p1. 
	} 
private: 
	Engine* engine; 
}; 
	
int main() { 
	Plane p1(200); 
	Plane p2 = p1; // Shallow Copy 
	
	p1.display(); // Ambos comparten el 
	p2.display(); // mismo puntero a Engine 
	
	return 0; 
} 
```
## Deep Copy
![Pasted image 20260331150123.png](/img/user/Attachments/Pasted%20image%2020260331150123.png)
### Ejemplo
``` cpp
class Engine { 
public: 
	Engine(int power) : power(power) {} 
	int getPower() const {return power; } 
private: 
	int power; 
};
	 
class Plane { 
public: 
	Plane(int power) : engine(new Engine(power)) {}
	// Constructor tipo Deep Copy 
	Plane(const Plane& rht) : engine(new Engine( (rht.engine)->getPower() ) ){} 
	// En la linea anterior se creo un nuevo puntero a Engine 
	void display() const { 
		std::cout << "Engine Power: " 
		<< engine->getPower()
		<< "\n";
	}

	~Plane() { 
		delete engine; 
	} 
private: 
	Engine* engine; 
}; 


int main() { 
	Plane p1(200); 
	Plane p2 = p1; // Al Deep Copy Constructor 
	p1.display(); 
	p2.display(); // Los objetos Engine son independientes 
	
	return 0; 
} 
// Como los objetos Engine son distintos objetos en distintas posiciones de memoria, son independientes
```

``` cpp
#include <iostream> 
#include <memory> // Para los smart pointers 

class Engine { 
public: 
	Engine(int power) : power(power) {} 
	int getPower() const { return power; } 
private: 
	int power; 
}; 

class Plane { 
public: 
	Plane(int serial, int length, int power) : 
		iSerialNumber(serial), iLength(length), 
		engine(std::make_unique<Engine>(power)) {} 
	
	// Deep Copy con Copy Constructor usando shared_ptr
	Plane(const Plane& rht) :// Deep copy de engine
		iSerialNumber(rht.iSerialNumber),
		iLength(rht.iLength),
		engine(std::make_unique<Engine>(rht.engine->getPower()) ) {}
							
	void display() const { 
		std::cout << "Serial Number: " << iSerialNumber 
		<< "\n"; 
		std::cout << "Length: " << iLength 
		<< "\n"; 
		std::cout << "Engine Power: " << engine->getPower() 
		<< "\n"; } 
private: 
	int iSerialNumber; 
	int iLength; 
	std::unique_ptr<Engine> engine; // Shared pointer para Engine 
}; 

int main() { 
	Plane p1(1234, 50, 200); 
	Plane p2 = p1; // Deep copy usando copy constructor 

	std::cout << "Plane 1 Details:\n";
	p1.display(); 
	std::cout << "\nPlane 2 Details (after deep copy):\n";
	p2.display();
	
	return 0; 
} // No hay necesidad de escribir los destroyers 
```

# Copia con Sobrecarga de Operador `=`
Se sobrecarga el operador `=` para realizar una deep copy al asignar un objeto a otro. Antes de copiar, se libera la memoria existente para evitar memory leaks, y luego se reserva nueva memoria copiando el contenido.

``` cpp
#include <iostream> 
using namespace std; 

class A { 
public: 
	int* data; 
	A(int value) { data = new int(value); } 
	
	// Operador de asignación (deep copy) 
	A& operator=(const A& other) { 
		if (this == &other) return *this; // Auto-asignación 
		
		delete data; // Liberar memoria actual 
		data = new int(*other.data); // Copiar contenido 
		
		return *this; 
	} 
};

void imprimirData(A a, A b){ 
	cout << "a.x: " << *(a.data) << ", " 
		<< "b.x: " << *(b.data) << endl; 
} 

int main(){ 
	int x = 23; 
	A a(x), b(5); 
	
	imprimirData(a, b); // a.x: 23, a.y: 5 
	b = a; 
	imprimirData(a, b); // a.x: 23, a.y: 23 
	
	return 0; 
} 
```