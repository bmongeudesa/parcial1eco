---
{"dg-publish":true,"permalink":"/notes/herencia/","dg-note-properties":{"subject":["[[Paradigmas de la Programación]]"],"clase":["[[Clase 6 - Herencia]]"],"type":["Concept"],"topics":["[[OOP]]"],"tags":null}}
---

# Herencia
Implica la creacion de una nueva clase (derivada) a partir de una clase (base) ya existente, donde heredará sus atributos y métodos.

![Pasted image 20260331154307.png](/img/user/Attachments/Pasted%20image%2020260331154307.png)
## *protected*
- **`protected`**: Los miembros solo son accesibles **dentro de la propia clase** y por las **clases que heredan de ella** (clases derivadas). No son accesibles desde objetos externos o funciones como `main`.
### Clase Base
``` cpp
#include <iostream> 
#include <string> 

class Avion { 
protected: 
	// No es ni público, ni privado! solo para herencia 
	std::string modelo; 
public: 
	Avion(std::string m) : modelo(m) {} 
	
	void despegar() { 
		std::cout << "El avion " << modelo 
		<< " esta despegando..." << std::endl; 
	} 
};
```
### Clases Derivadas
``` cpp
class AvionJet : public Avion { 
// Se dice que “AvionJet” hereda de “Avion” 
private: 
	int numTurbinas; 
public: // El constructor de la hija DEBE llamar al de la madre 
	AvionJet(std::string m, int t) : Avion(m), numTurbinas(t) {} 
	void activarAfterburner() {
		std::cout << modelo << " activando postcombustion en sus " 
		<< numTurbinas << " turbinas!" << std::endl; 
	} 
}; 

class UAV : public Avion { 
public: 
	UAV(std::string m) : Avion(m) {} 
	void transmitirVideo() { 
		std::cout << "El UAV " << modelo 
		<< " transmitiendo video en tiempo real." << std::endl; 
	} 
};
```

### main
``` cpp 
int main() { 
	AvionJet f16("F-16", 2); 
	UAV uav("MQ-1"); 
	
	// Métodos heredados de la clase base 
	f16.despegar(); 
	uav.despegar(); 
	
	// Métodos específicos de cada clase derivada 
	f16.activarAfterburner(); 
	uav.transmitirVideo(); 
	
	return 0; 
}
```

## Propiedad
Dependiendo como se hace la herencia, el acceso a las variables/funciones miembro será:

| Encapsulamiento en la base | Herencia Publica | Herencia Protegida | Herencia Privada |
| -------------------------- | ---------------- | ------------------ | ---------------- |
| public                     | public           | protected          | private          |
| protected                  | protected        | protected          | private          |
| private (default)          | No accesible     | No accesible       | No accesible     |
	*importante recordar que los atributos y métodos privados siempre serán accesibles desde el mismo objeto y desde ningún otro lado más*
	
## Herencia Publica y Privada
### Ejemplo:
#### Clases Bases
``` cpp
// Clase Base Avion (se usará como herencia pública) 
class Avion { 
public: 
	Avion(std::string modelo) : modelo(modelo) {} 
	
	void volar() const { 
		std::cout << modelo << " esta volando!" << std::endl; 
	} 
protected: 
	std::string modelo; 
};
```

```cpp
// Clase Base Motor (se usará como herencia privada) }
class Motor { 
public: 
	Motor(float horsepower) : horsepower(horsepower) {} 
	
	void encenderMotor() const { 
		std::cout << "Encendiendo motor con " << horsepower << " HP." 
		<< std::endl; 
	} 
protected: 
	float horsepower; 
}; 
```
#### Derivadas
*Al crearse la base PropPlane, esta hereda los miembros públicos y protegidos de la clase Avion y Motor. No obstante, los miembros de Motor no pueden ser accedidos desde el objeto cessna en main*

``` cpp
// Clase deriveda PropPlane (herencia publica de Avion y privada de Motor) 
class PropPlane : public Avion, private Motor { 
public: 
	PropPlane(std::string modelo, float horsepower) : Avion(modelo), Motor(horsepower) {} 
	void encenderPropPlane() { 
	// Está permitido llamar a encenderMotor() desde 
	// PropPlane porque Motor se hereda en forma privada 
		Motor::encenderMotor(); std::cout << modelo 
		<< " esta listo para despegar!" << std::endl; 
	} 
protected: 
	int nroPasajeros = 1; 
};
```

``` cpp
int main() { 
	PropPlane cessna("Cessna 172", 160); 
	// Se llama un metodo de PropPlane 
	cessna.encenderPropPlane(); // Se llama este metodo heredado de Avion 
	cessna.volar(); 
	// cessna.encenderMotor(); // Error: encenderMotor() es 
	//privada en PropPlane porque se heredo de Motor en forma 
	//privada (ver class PropPlane) 
	
	// std::cout << cessna.nroPasajeros; // Error: nroPasajeros no 
	// se puede acceder! Por qué? 
	return 0; 
}
```
# Constructores y Destructores
Una clase derivada SIEMPRE llamará:
- Constructor de su clase base primero
- Destructor de clase derivada primero (inverso)
*Si no existe un constructor default en la clase base, el compilador creará uno vacío para poder instanciar la clase base.*
#### Ejemplo
``` cpp
#include <iostream> 
class Base { 
public: 
	Base() { std::cout << "Constructor de clase base" << std::endl; } 
	~Base() { std::cout << "Destructor de clase base" << std::endl; } }; 
	
class Intermedia : public Base { 
public: 
	Intermedia() { std::cout << "Constructor de clase intermedia" 
	<< std::endl; } 
	~Intermedia() { std::cout << "Destructor de clase intermedia" 
	<< std::endl; } 
}; 

class Derivada : public Intermedia { 
public: 
	Derivada() { std::cout << "Constructor de clase derivada" 
	<< std::endl; } 
	~Derivada() { std::cout << "Destructor de clase derivada" 
	<< std::endl; } 
}; 

int main() { 
	std::cout << "Creando objeto de clase Derivada\n"; 
	Derivada obj; 
	std::cout << "Fin del main.\n"; 
	return 0;
};
```
1. Creando objeto de clase Derivada 
2. Constructor de clase base 
3. Constructor de clase intermedia 
4. Constructor de clase derivada 
5. Fin del main. 
6. Destructor de clase derivada 
7. Destructor de clase intermedia 
8. Destructor de clase base 
## Explicitos e Implícitos
- El constructor de la base puede ser llamado desde la clase derivada para evitar codigo repetido
	Los llamados pueden ser **implicitos o explicitos**
#### Explicitos
El llamado se realiza al llamar el constructor de la clase desde el constructor de la clase derivada (debe escribirse)
``` cpp
#include <iostream> 
class Base { 
public: 
	Base(int x, int y) { // Constructor de la clase Base 
		std::cout << "El constructor base es llamado con x=" 
		<< x << ", y=" << y << std::endl; 
	} 
}; 

class Derived : public Base { 
public: // Se llama el constructor de Base explícitamente 
	Derived(int xx, int yy) : Base(xx, yy) { 
		"Se llama al constructor derivado" << std::endl; 
	} 
}; 

int main() { // Se llama Base(10,20) con Derived(10,20) 
	Derived obj(10, 20); 
	return 0; 
}
```
#### Implicitos
No es necesario escribir el constructor que llama al constructor de la clase base
``` cpp
#include <iostream> 
class Base { 
public: 
	Base(int x, int y) { // Constructor de la clase Base 
		std::cout << "El constructor base es llamado con x=" 
		<< x << ", y=" << y << std::endl; 
	} 
}; 
class Derived : public Base { 
public: // Se heredan el/los constructor/es de Base 
	using Base::Base; 
	// Se usa cuando los constructores necesarios ya existen en Base 
}; 
// Si se escribiera un constructor igual a alguno de Base, se reemplazá. Si se pone uno nuevo, se agrega. 

int main() { 
	Derived obj(10, 20); // No hay constructor en Derived 
	return 0; 
}
```
#### Implicitas multiples
Se pueden heredar multiples constructores de la clase base (los cuales pueden ser sobrescritos)
``` cpp
#include <iostream> 
class Base { 
public: 
	Base() { // Constructor default de Base 
		std::cout << "Constructor Default de Base\n"; 
	} 
	
	Base(int x) { // Constructor de Base con 1 param 
		std::cout << "Constructor de Base con x=" << x << std::endl; 
	} 
	
	Base(int x, int y) {// Constructor de Base con 2 params 
		std::cout << "Constructor de Base con x=" << x << " e y = " 
		<< y << std::endl; 
	} 
};
```

``` cpp
class Derivada : public Base { 
public: // Se heredan todos los constructores de Base 
	using Base::Base; 
	
	Derivada(int x, int y) { 
		std::cout << "Constructor de Derivada con x=" << x 
		<< " e y=" << y << std::endl;
	} // Se sobreescribe el const de Base de 2 params 
}; 

int main() { 
	// Constructor default de Base 
	Derivada obj1; 
	// Constructor de 1 param de Base 
	Derivada obj2(10); 
	// Constructor sin parametros de Base 'y'
	
	// constructor con 2 params de Derivada 
	Derivada obj3(10, 20); 
	return 0; 
} // :Base(x,y) para el contructor de Base de 2 params.
```
# Herencia Múltiple
Una clase puede tener varias clases base (visto antes), y asi heredar multiples comportamientos y datos.
	Si 2 metodos tienen mismo nombre, conviene usar referencias a su base
	`Base1::comer()` o `Base2::comer()`

Este tipo de herencia puede conducir a ciclos cerrados de herencia (ilegales en C++)
![Pasted image 20260410120504.png\|267](/img/user/Attachments/Pasted%20image%2020260410120504.png)
## Problema del Diamante
Ocurre cuando hay varias clases base (clases B y C) que tienen la misma clase derivada (clase D) y una misma clase ancestro (clase A).
![Pasted image 20260410123730.png](/img/user/Attachments/Pasted%20image%2020260410123730.png)
El problema aparece cuando se desea hacer uso de un método de la clase ancestro, para la cual existen dos posibles caminos (a través de la clase B y la C), generándose una ambigüedad.
#### Ejemplo 1:
``` cpp
#include <iostream> 
using namespace std; 

// Clase Ancestro
class A { 
public: 
	A() { cout << "Constructor de A" << endl; } 
	void show() { cout << "Showing..." << endl; } 
}; 

// Heredan de A
class B : public A { 
public: 
	B() { cout << "Constructor de B" << endl; } 
};
class C : public A { 
public: 
	C() { cout << "Constructor de C" << endl; } 
}; 

// Clase Problematica:
class D : public B, public C { \
public: 
	D() { cout << "Constructor de D" << endl; } 
}; 

int main() { 
	D d; 
	// Hay dos caminos para llegar a show() 
	d.show(); //Error: "D::show" is ambiguous 
	return 0; 
} 
```

#### Ejemplo 2:
``` cpp
#include <iostream> 
using namespace std; 
class A { public: int x; }; 
class B : public A { }; 
class C : public A { }; 
class D : public B, public C { }; 

int main() { 
	D objeto; 
	// objeto.x = 10; // Error por Ambigüedad. ¿B::x o C::x? 
	// Para desambiguar: objeto.[ClasePadre]::[Atributo]
	objeto.B::x = 10; // Acceso manual a la copia de B 
	objeto.C::x = 20; // Acceso manual a la copia de C 
	
	std::cout << objeto.B::x; // Imprime 10 
	std::cout << objeto.C::x; // Imprime 20 (variables distintas)
```
### Como Evitarlo
``` cpp
#include <iostream> 
using namespace std; 
class A { 
public: 
	A() { cout << "Constructor de A" << endl; } 
	void show() { cout << "Showing..." << endl; } 
}; 

class B : virtual public A { 
public: 
	B() { cout << "Constructor de B" << endl; } 
}; 
class C : virtual public A { 
public: 
	C() { cout << "Constructor de C" << endl; } 
};

class D : public B, public C { 
public: 
	D() { cout << "Constructor de D" << endl; } 
}; 

int main() { 
	D d; // Hay dos caminos para llegar a show() 
	d.show(); // No hay error 
	return 0; 
}
```
```
Imprime:
Constructor de A 
Constructor de B 
Constructor de C 
Constructor de D 
Showing...
```
## Object Slicing
Ocurre cuando un objeto de una clase derivada se copia en una variable de la clase base por valor.
Se hace cuando solo se quiere un objeto con la info de la clase base
- Se pierde la parte de la derivada y queda solo la de la base
- También se puede perder la posibilidad de invocar los métodos definidos en la clase derivada

``` cpp
#include <iostream> 
class Base { 
public: 
	int x; 
}; 

class Derivada : public Base { 
public: 
	int y; 
}; 

int main() { 
	Derivada d; 
	d.x = 1; d.y = 2; 
	
	// Slicing, se pierde y. 
	Base b = d; 
	
	return 0;
}
```

