---
{"dg-publish":true,"permalink":"/notes/calificadores-y-operadores/","dg-note-properties":{"subject":["[[Paradigmas de la Programación]]"],"clase":["[[Clase 7 - Polimorfismo]]"],"type":["Concept"],"topics":["[[OOP]]"],"tags":null}}
---

# Calificadores
## *default*
Se utiliza para definir explicitamente el constructor y/o destructor default. No hace mas que pedir que se cree el destructor/constructor vacío
Si la clase tiene miembros que son objetos o una base, se llamaran automaticamente a los contructores/destructores por defecto.
``` cpp
#include <iostream> 
using namespace std; 

class MyClassAttr { 
public: 
	MyClassAttr() { cout << "Constructor de MyClassAttr\n"; } 
	~MyClassAttr() { cout << "Destructor de MyClassAttr\n"; } 
}

class MyClass { 
	MyClassAttr m; 
public: 
	MyClass() = default; // Constructor default 
	~MyClass() = default; // Destructor default 
}; 

int main() { 
	MyClass c; 
}
```
```
Constructor de MyClassAttr
Destructor de MyClassAttr
```
## *delete*
Deshabilita los métodos.
- usualmente se hace para evitar copias o asignaciones
Al definir un destructor como default, automaticamente se llamaran los destructores de los objetos miembros de esa clase.
``` cpp
class Drone { 
public: 
	// Constructor default generado explícitamente por el 
	//compilador 
	Drone() = default; 

	// Deshabilita el copy constructor evitando copias 
	Drone(const Drone&) = delete; 

	// Deshabilita el operador de asignación de copia para 
	//prevenir asignaciones 
	Drone& operator=(const Drone&) = delete; 

	// Indica explícitamente al compilador que cree el 
	//destructor 
	~Drone() = default; 
	
	void show() { 
		std::cout << "Objeto Drone creado!" << std::endl; 
	} 
}; 		
	
int main() { 
	Drone d1; 
	
	// Drone d2 = d1; // ERROR: No hay un Copy constructor 
	// Drone d2(d1); // ERROR: No hay un Copy constructor 
	// d1 = d2; // ERROR: No hay un operador de asignación 
	d1.show(); 
	}
```

### Ejemplo Interfaz
- Una interfaz no define un constructor por defecto
- Tampoco es necesario definir un destructor (se puede hacer en la clase)
- Es posible hacerlo en la interfaz, pero como un metodo virtual puro y luego implementarlo para la interfaz
``` cpp
class IEnemy { 
public: 
	virtual int getHP() = 0; 
	virtual bool isDead() = 0; 
	virtual int attack() = 0; 
	virtual ~IEnemy() {} // Destructor virtual 
};
```
```cpp
class IEnemy { 
public: 
	virtual int attack() = 0; 
	virtual ~IEnemy() = 0; // Destructor virtual puro 
}; 
IEnemy::~IEnemy(){ // El destructor debe ser implementado 
	std::cout << "El destructor de IEnemy" << std::endl;\
}; 
class A : public IEnemy{ 
public: 
	A(){std::cout << "El constructor de A" << std::endl;} 
	int attack() {return 1;} 
	~A() {std::cout << "El destructor de A" << std::endl;} 
}; // ~A() no necesita override, no se sobreescribe ~A() 

int main(){ 
	A a; // Imprime: “El constructor de A” 
	return 0; 
}// Imprime: “El destructor de A”, “El destructor de IEnemy” 
```
## *final*
### Aplicado a Clases
Es utilizado en una clase, dicha clase no podrá ser heredada por otra.
Suele utilizarse para:
- evitar modificaciones en el arbol de herencia de clases
- forzar un diseño determinado
- por seguridad
NO se podrá usar como una clase base. (Derivada como final)
	==De hacerlo, se tendrá un error de compilación==
``` cpp
#include <iostream> 

class Base { 
public: 
	virtual void show() { std::cout << "Clase Base\n"; } 
}; 

class Derivada final : public Base { 
public: // 'final’ no permite producir más clases por herencia 
	void show() override { std::cout << "Clase Derivada\n"; } 
}; 

class ProxClase : public Derivada { 
public: //Error: a final class type cannot be used as a base class. 
	void show() override { std::cout << "Proxima clase\n"; } 
}; 

int main() { 
	Derivada d; 
	d.show(); 
} 
```
### Aplicado a Metodos
Evitan que el metodo sea sobreecrito o redefinido en clases derivadas (solo tiene sentido en metodos virtuales).
``` cpp
#include <iostream> 
class Base { 
public: // 'final’ previene que la clase sea sobreescrita 
	virtual void show() final { std::cout << "Clase Base\n"; } 
}; 

class Derivada : public Base { 
public: // Error: Cannot override 'show' because it's final in Base 
	void show() override { std::cout << "Clase Derivada\n"; }
}; 

int main() { 
	Base b; 
	b.show(); 
	return 0; 
}
```
El error de compilación ocurre porque el método show() de la clase Base está marcado como final.
	final protege la implementación de show() y evita generar código nuevo para ese método en clases derivadas
# Operadores
## *static_cast*
- Utilizado para castear en forma explícita entre tipos en tiempo de compilación. 
- Realiza el cast en forma segura.
- No es seguro entre punteros a clases bases y punteros a clases derivada (no es *type safe downcasting* con polimorfismo)
```cpp
static_cast<Tipo>(expresion)
```
- Tipo: nuevo tipo al que se quiere castear
- expresion: variable y objeto a convertir
==Si no hay polimorfismo se puede usar static_cast para downcasting, es mas rapido que dynamic_cast==

Se puede utilizar de distintas formas:
#### Entre Variables Numericas
```cpp
int i=1; double d = static_cast<double>(i);
```
#### Entre objetos base y derivado
#revisar 
```cpp
Base* base = new Derived(); // Upcasting implícito, siempre seguro. 
Derived* derived = new Derived(); 
Derived* der = static_cast<Derived*>(base); 
// Downcasting, peligroso si base no apunta a Derived, aca es seguro. Si fuera Base* base = new Base(); daría comportamiento indefinido. 
Base* bs = static_cast<Base*>(derived); // Upcasting explícito, siempre seguro.
```
#### Entre `*void` y un puntero especifico 
```cpp
void* ptr = malloc(sizeof(int)); 
int* intPtr = static_cast<int*>(ptr); 
```
## *dynamic_cast*
- Utilizado para castear de forma segura objetos polimorficos (objetos con al menos un metodo virtual)
- Principalmente utilizado para hacer *downcasting* (de obj base a derivado)
```cpp
dynamic_cast<Tipo>(expresion)
```
- Tipo: nuevo tipo a castear
- Expresion: variable u obj a convertir

==Es mas lento que static_cast==, utiliza RTTI (Run-Time Type Information) para conocer los tipos en tiempo de ejecucion y verificar si es posible la conversion

**Se puede utilizar cuando:**
- se tiene herencia publica 
- existe al menos un metodo virtual
- se debe hacer uso de punteros o referencias

**Si el cast falla:**
- si puntero: se devuelve *null_ptr*
- si referencia: se lanza *std::bad_cast*
### Downcasting
  
``` cpp
#include <iostream> 

class Base { 
public: // Método virtual 
	virtual void show() { std::cout << "Clase Base\n"; } 
}; 
class Derivada : public Base { 
public: 
	void show() override { std::cout << "Clase Derivada\n"; } 
}; 

int main() { 
	// Puntero a clase Base a objeto Derivada 
	Base* b = new Derivada(); 
	// Downcast seguro 
	Derivada* d = dynamic_cast<Derivada*>(b); 
	if (d != nullptr) { 
		d->show(); // Imprime: Clase Derivada 
	} 
	else { 
		std::cout << "Cast failed!\n"; 
	}
	delete b;
}
```
### Referencias
Como en referencia no se devuelve null_ptr sino que un error, lo podemos capturar.
Este devuelve  `std::bad_cast`, puede ser capturado coin *try/catch*
Mecanismo muy poderoso para manejar un downcasting en tiempo de ejecucion.
```cpp
#include <iostream> 
#include <typeinfo> 

class Base { 
public: // Como método virtual requerido se usa el Destructor 
	virtual ~Base() {}
}; 

class Derived : public Base {}; 

int main() { 
	Derived d; 
	Base& b = d; // Referencia de clase Base a objeto Derivada 
	
	try { // Downcast seguro 
		Derived& d_ref = dynamic_cast<Derived&>(b); 
		std::cout << "Se logro hacer el Cast!\n"; 
	} catch (const std::bad_cast& e) { 
		std::cout << "El cast fallo: " << e.what() << "\n"; 
	} 
	return 0; 
} 
```