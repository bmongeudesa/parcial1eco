---
{"dg-publish":true,"permalink":"/notes/sobrecarga-de-operadores/","dg-note-properties":{"subject":["[[Paradigmas de la Programación]]"],"clase":["[[Clase 5 - Sobrecarga de Operadores]]"],"type":["Concept"],"topics":["[[OOP]]"],"tags":null}}
---

Es posible redefinir el comportamientos de los operadores para que actúen sobre objetos de clases definidas por el usuario
#### Se suelen sobrecargar:
	`==, !=, <, >, =, +`
Se logra mediante la adición de una función llamada "operator + operador a sobrecargar"
``` cpp
<tipoRetorno> operator <operador> (<args>);
```

### Cuando usar cada sobrecarga
#### Funciones miembro: 
- Actúan directamente sobre la memoria del objeto. 
	Ejemplo: Operadores de Asignación (=, +=, -=, *=) o unarios (++, –).
- Ventaja: Tienen acceso directo a los atributos private. 
#### Funciones NO miembro:
- Se utilizan principalmente para operadores binarios donde se busca simetría 
	(por ejemplo, si queres permitir que tanto objeto + 5 como 5 + objeto funcionen). 
- El operando de la izquierda no es de tu clase 
	(por ejemplo << (cout) y >> (cin) )

## Operadores Lógicos
### Ejemplo 1: 
#### Operador `>`:
``` cpp
class Car { 
private: 
	std::string maker; 
	double length; // Largo del auto en centímetros 

public: 
	Car(std::string maker, double length) : maker(maker), 
									length(length) {} 
	
	// Sobrecargando el operador '>' para comparar por length 
	bool operator>(const Car& other) const { 
		return this->length > other.length; 
	} 
};
```

``` cpp
int main() { 
	Car car1("Toyota", 450.0); 
	Car car2("Ford", 400.0); 
	
	if (car1 > car2) 
		std::cout << "Car 1 es mas largo que car 2\n"; 
	else 
		std::cout << "Car 1 no es mas largo que car 2\n"; 
	// Imprime: Car 1 es mas largo que car 2 
	return 0; 
} 
```
### Ejemplo 2:
#### Operador `==`:
``` cpp
#include <iostream> 
#include <string> 

class Car { 
private: 
	std::string maker; int year; 
	
public: 
	Car(std::string maker, int year) : maker(maker), year(year) {} 

	// Sobrecarga de '==' para ver si los autos son iguales 
	bool operator==(const Car& other) const { 
		return (this->maker == other.maker) && 
			   (this->year == other.year); } 
}; 
```

``` cpp
int main() { 
	Car car1("Toyota", 2010); 
	Car car2("Toyota", 2010); 
	Car car3("Ford", 2010); 
	
	// Comparar car1 y car2 
	if (car1 == car2) // Imprime: Car1 y car 2 son iguales. 
		std::cout << "Car 1 y car 2 son iguales.\n"; 
	else 
		std::cout << "Car 1 y car 2 son diferentes.\n"; 
	// Comparar car1 y car3 
	if (car1 == car3) // Imprime: Car1 y car 3 son diferentes. 
		std::cout << "Car 1 y car 3 son iguales.\n"; 
	else 
		std::cout << "Car 1 y car 3 son diferentes.\n"; 
	return 0; 
};
```
## Operadores Aritméticos
### Ejemplo 1
#### Operador `+`
``` cpp
#include <iostream> 
#include <string> 

class Car { 
private: 
	std::string maker; 
	double length; // Largo del auto en centimetros 
public: 
	Car(std::string maker, double length) : maker(maker), length(length) {} 
	
	// Sobrecarga de'+' para sumar las longitudes de los autos 
	double operator+(const Car& other) const { 
		return this->length + other.length; 
	} // El segundo operando, car2, es pasado como parámetero 
};
```

``` cpp
int main() { 
	Car car1("Toyota", 450.0); 
	Car car2("Ford", 400.0); 
	
	// Imprime la suma de las longitudes de los autos 
	std::cout << "La longitud total de los autos es: " 
			  << car1 + car2 << " cm\n"; 
			  
	return 0; 
};
```
### Ejemplo 2
#### Operador `+=`
``` cpp
#include <iostream> 
#include <string> 
class Car { 
private: 
	std::string maker; 
	double length; // Largo del auto 
public: 
	Car(std::string maker, double length) : maker(maker), length(length) {} 

	// Sobrecarga '+=' para obtener un vehículo mas largo 
	Car& operator+=(double extraLength) { 
		this->length += extraLength; // Agrega largo extra 
		return *this; // Devuelve el objeto modificado 
	} 

	void display() const { // Muestra la información 
		std::cout << "Fabricante: " << make << ", Largo: " 
		<< length << " cm\n"; 
	} 
};
```
*Se retorno `Car&` porque se desea tener concatenacion para esta operacion

==**Las operaciones que modifican al objeto se retornan por referencia**==
	Las que no, por valor

``` cpp
int main() { 
	Car car("Toyota", 450.0); // Agrega 50 cm y luego 30 cm a la longitud original del auto 
	(car += 50) += 30; car.display(); // Muestra que la longitud del auto es 530 cm 
	return 0; 
}
```
## Funciones No Miembro
*El compilador selecciona la sobrecarga adecuada en función del nombre de la función y de los tipos de sus parámetros*
### Ejemplo
``` cpp
#include <iostream> 
using namespace std; 

class Plane { 
public: 
	double wingspan; 
	Plane(double w) : wingspan(w) {} 
};
```

``` cpp
// Sobrecarga del operador + 
Plane operator+(const Plane& a, const Plane& b) { 
	return Plane(a.wingspan + b.wingspan); 
} 

int main() { 
	Plane p1(30.0), p2(35.0); 
	Plane total = p1 + p2; // Llama a operator+(p1, p2) 
	
	return 0; 
} 
```
## Operadores con *friend*
### Ejemplo:
*Se sobrecarga el operador “<<“ que recibe una clase ostream que pertenece a la C++ Standard Library (parte de iostream), para imprimir los datos del objeto Plane en una manera particular.*

``` cpp
#include <iostream> 
#include <string> 
using namespace std; 

class Plane { 
	string model; 
	int capacity; 
	double wingspan; 
public: 
	Plane(string m, int c, double w) : 
		model(m), capacity(c), wingspan(w) {} 
		
	friend ostream& operator<<(ostream& os, const Plane& p); // Amistad con función no miembro 
};
```

``` cpp
// Esta es una función no miembro, no es parte de una clase
ostream& operator<<(ostream& os, const Plane& p) { 
	os << "Plane Model: " << p.model << "\n" 
	   << "Capacity: " << p.capacity << " passengers\n" 
	   << "Wingspan: " << p.wingspan << " meters \n"; 
	return os; 
} 

int main() { 
	Plane plane("Boeing 747", 416, 68.4); // Usa el operador “<<“ para imprimir el objeto
	cout << plane; // Idem a hacer: operator<<(cout, plane) 
	return 0; }
```

## No miembro y No friend
*Se sobrecarga el operador “<<“ que recibe una clase ostream que pertenece a la C++ Standard Library (parte de iostream), para imprimir los datos del objeto Plane, sin ser ‘friend’.* 
``` cpp
#include <iostream> 
#include <string> 
using namespace std; 

class Plane { 
	string model; 
	int capacity; 
	double wingspan; 
public: 
	Plane(string m, int c, double w) : 
		model(m), capacity(c), wingspan(w) {} 
		
	// Getters públicos (necesarios si no usamos friend) 
	string getModel() const { return model; } 
	int getCapacity() const { return capacity; } 
};
```

```cpp
// Esta es una función no miembro, no es parte de una clase 
ostream& operator<<(ostream& os, const Plane& p) { 
	os << "Plane Model: " << p.getModel() << "\n" 
	   << "Capacity: " << p.getCapacity() << " passengers\n";
	return os; 
} 

int main() { 
	Plane plane("Boeing 747", 416, 68.4); // Usa el operador “<<“ para imprimir el objeto 
	cout << plane; // Idem a hacer: operator<<(cout, plane) 
	return 0; 
}
```