---
{"dg-publish":true,"permalink":"/notes/programacion-de-clases-c/","dg-note-properties":{"subject":["[[Paradigmas de la Programación]]"],"clase":["[[OOP]]"],"type":["Concept"],"topics":["[[OOP]]"],"tags":null}}
---

# Clases y Objetos
## Clase
Una clase es el plano de lo que se desea representar, esta incluye los **atributos** y **métodos** del objeto. 
 - Deben ser escritas 
 - no poseen tiempo de vida porque no ocupan memoria 
	 (más allá de la memoria del programa) en tiempo de ejecución. 
- Al ser tan maleable, se suele usar un conjunto de clases, separando definicion de implementacion en archivos como **header** y **source**
### Objeto
Al instanciar una clase, se crean objetos. 
- Tienen un tiempo de vida durante el cual se van modificando o no
#### Ejemplo Simple
``` cpp
#include <iostream> 
#include <string> 
class Plane { 
public: 
	float altitude; // Altitud del avion 
	std::string model; // Modelo de avion 
	int capacity; // Numero de pasajeros 
void mostrarInfo(){ 
	std::cout << "Modelo: " << model 
	<< ", Capacidad: " << capacity 
	<< ", Altitude: " << altitude << std::endl; 
	} 
void volar() const{ 
	std::cout << "El avion " << model 
	<< " esta volando a una altitud " << altitude << " mts." 
	<< std::endl; 
	} 
};
```

## Aspectos a Considerar antes de Crearlas
1. Que hace mi objeto en mi programa? Cuales son sus comportamientos? 
	Estos son usualmente implementados como métodos del tipo público (recuerde encapsulamiento y abstracción)
2. Que estados y datos se necesitan en la clase? 
	Si hay información que sólo es necesaria que la sepa la clase, estos serán datos privados. 
	Preguntesé: si los setters y getters no realizan mas que devolver o cambiar un valor, ¿es necesario que sea privado?
3. Recuerde agregar constructores, destructores y operadores sobrecargados (¿¡que esto!?) 

# Files
## header
En este archivo se realiza unicamente definiciones (no implementaciones ni asignaciones)
No debe estar incluido el archivo *source* "Ejemplo.cpp" (mala practica)
#### Ejemplo:
```cpp
#pragma once 
#include <string> 

class Plane { 
public: 
	float altitude; // Altitud del avion 
	std::string model; // Modelo de avion 
	int capacity; // Nr de pasajeros 
	
	void mostrarInfo() const; 
	void volar(); 
	bool requiereManteminento() const; 
private: 
	float oilStatus; // En porcentaje 
}; 
```
## source
En este archivo se encuentra el codigo que **implementa** las definiciones del archivo header
	Notar que se usa la sintaxis de `::` (scope resolution operator) para indicar a que clase perteneces los metodos implementados
#### Ejemplo
```cpp
#include "Plane.h" 
#include <iostream> 

void Plane::mostrarInfo() const{ 
	std::cout << "Modelo: " << model 
	<< ", Capacidad: " << capacity 
	<< ", Altitude: " << altitude 
	<< std::endl; 
} 

void Plane::volar(){ 
	std::cout << "El avion " << model 
	<< " esta volando a una altitud " << altitude << " mts." 
	<< std::endl; 
} 

bool Plane::requiereManteminento() const{
	return (oilStatus < 50.0 ? true : false); 
} // Si hay menos del 50 % requeire mantenimiento
```
## main
En este archivo se implementa toda la logica de uso el programa.
Se incluye unicamente el header file (no el source code).
#### Ejemplo
```cpp
#include "Plane.h" 
#include <iostream> 

int main(){ 
	Plane myPlane; 
	myPlane.model = "Cessna 172 Skyhawk"; 
	myPlane.capacity = 4; 
	myPlane.altitude = 0.0; 
	
	myPlane.mostrarInfo(); 
	// Imprime: Modelo: Cessna 172 Skyhawk, Capacidad: 4, Altitude: 0 
	
	if (myPlane.requiereManteminento()) 
		std::cout << "No volar, requiere mantenimiento" << std::endl; 
	else{ 
		std::cout << "Puede volar, no se requiere mantenimiento." 
		<< std::endl; 
		myPlane.volar(); 
	} // No vuela porque oilStatus no se definio 
	return 0;
}
```
# Miembros
Una clase puede asignarle permisos a sus atributos y metodos.
## *public*
Indica que los metodos y atributos son de acceso publuco una vez definidos.

NO todos los metodos pueden ser public:
- Rompe la regla de encapsulamiento, 
- Da a otros objetos acceso total, 
- Expone la complejidad de los métodos a los usuarios, 
- Otros programadores pueden hacer cambios en partes del sistema que introduzcan bugs.
## *private*
Es el calificador por default. Este indica que los metodos y atributos definidos solo pueden ser accedidos **desde el objeto**

# Constructores
Es la parte del codigo que se utiliza para **inicializar** un objeto.
- Se ejecuta cada vez que un objeto es creado (permite incializar los atributos)
- Los atributos solo se pueden iniciar mediante:
	- El constructor
	- Metodos setter
- Si uno no lo define, el compilador crea uno por default.
- Puede ser sobrecargado (se puede inicializar una clase con distintos tipos de dato)


*Al inicializar un objeto, primeramente se ejecuta la definición de las variables (en este caso, todas son private), luego se llama al constructor y se ejecuta su código*
### Ejemplo
#### main
```cpp
#include <iostream> 
#include "Plane.h" 

int main(){ 
	Plane aPlane(10, 20); 
	aPlane.setNumWheels(2); 

	std::cout << "Numero de ruedas: " << aPlane.getNumWheels() 
	<< std::endl; 
	// Imprime: Numero de ruedas: 2 
	
	return 0; 
} 
```
#### source
```cpp
#include "Plane.h"
#include <iostream> 

Plane::Plane(){
	iNumWheels = 3; 
} 

Plane::Plane(PLANE_TYPE t){ 
	if (t==UAV) 
		iNumWheels = 0; 
	else 
		iNumWheels = 3; 
} 

Plane::Plane(int x, int y){ 
	iXPos = x; 
	iYPos = y; 
} 

int Plane::getNumWheels(){ 
	return iNumWheels; 
} 

void Plane::setNumWheels(int nrW){ 
	iNumWheels = nrW; 
}
```
#### header
```cpp
enum PLANE_TYPE{ JET, PROPELER, UAV, MILITARY}; 

class Plane { 
public: 
	Plane(); // Constructor default a definir. 
	Plane(PLANE_TYPE t); // Constructor overloaded 
	Plane(int x, int y); // Constructor overloaded 
	void setNumWheels(int nrW); 
	int getNumWheels(); 
private: 
	int iNumWheels; 
	PLANE_TYPE planeType; 
	int iXPos; 
	int iYPos; 
};
```
Los distintos constructores definidos se llaman dependiendo del tipo de variable que se pasa como parametro para incializar el objeto.
## Lista de inicialización
Tambien, mas comodamente, se pueden utilizar listas de inicializacion para iniciar datos miembro
### Asignar datos default
```cpp
Plane::Plane(){ 
	iNumWheels = 3; 
}
```
a 
```cpp
Plane::Plane():iNumWheels(3){}
```
### Inicializar variables
```cpp
Plane::Plane(int x, int y){ 
	iXPos = x; 
	iYPos = y; 
}
```
a 
```cpp
Plane::Plane(int x, int y):iXPos(x), iYPos(y){}
```
==Las variables [[Notes/Calificadores const y static#const\|const]] DEBEN ser inicializadas con lista de inicializacion:==

![Pasted image 20260413103403.png](/img/user/Attachments/Pasted%20image%2020260413103403.png)
## Llamada Implicita / Explicita
Una invocacion **implicita** 
- la hace el compilador y es automatica (puede conllevar conversion de tipo)
- no utilzia parentesis 
- implica menor control

Una invocacion **explicita** 
- se hace con la instruccion del programador
- utiliza parentesis
- se tiene control de la llamada
![Pasted image 20260413104002.png](/img/user/Attachments/Pasted%20image%2020260413104002.png)
### Llamada Implicita NO deseada
```cpp
#include <iostream> 
class myClass { 
public: 
	myClass(int x) { 
		std::cout << "Se llamo al constructor con: " << x << "\n"; 
	} 
}; 

// Al llamar a la función se llama implicitamente al constructor con 1 
void print(myClass obj) { 
	std::cout << "Se llamo a print" << std::endl; 
} 

int main() { 
	print(1); // Se llama a print con un número 
}
```
Hay una función que recibe un objeto como parámetro
En main se pasa un parametro int (cuando debia recibir un obj), 
Sin embargo, como el objeto que espera recibe int para inicializarse,
se crea un objeto en el scope de la función print.
- potencial fuente de bugs y funcionalidad no deseada.
- se evita declarando al consruct como **explicit**, para que no compile el programa 
# Destructores
Permite liberar memoria que haya sido dinamcamente requerida durante la vida delobjeto.
- Se ejecuta cuando el objeto sale del scope
- Solo es necesario definirlo cuando se utilizan punteros que deben ser eliminados ([[Raw Pointers\|Raw Pointers]])
	- Si no se define, el puntero no se libera y causa un *memory leak*
- Generalmente, se hacen `free` o `delete`, o en algunos casos se asigna *nullptr* para evitar *dangling pointers*

#### Ejemplo
```cpp
#include <iostream> 
class Plane { 
public: 
	Plane(int x, int y); 
	~Plane(); //declaracion del destructor
private: 
	Engine* planeEngine; 
};
```
Como `planeEngine` es un raw pointer, debe definirse el destructor y este se debe liberar en la definicion del mismo (lo definimos en el source file)
```cpp
Plane::~Plane(){ 
	delete planeEngine; 
	planeEngine = nullptr; // Asigna nullptr al puntero 
} 
```

# Objetos Creados Dinamicamente
Pueden crearse dinámicamente al igual que una variable
- Con Smart Pointers
- Con comando *[[Notes/Programación de Clases C++#new\|#new]]*
Se utiliza el operador `->` para llamar méotdos o variables publicas
```cpp
#include <iostream> 
#include "newPlane.h" 

int main(){ 
	Plane* ptrToPlane = new Plane(); 
	ptrToPlane->setNumWheels(20); 
	
	delete ptrToPlane; 
	ptrToPlane = nullptr; 
	
	return 0; 
}
```
## *new*
- aloca memoria para el objeto
- llama al constructor del objeto
- devuelve un puntero al objeto creado
## delete
- elimina los datos de la memoria *heap*
- redirige el puntero a *nullptr*
# Puntero this
Hace referencia a la instancia actual de la clase (el objeto)
Se puede usar cuando se necesita hacer referencia:
- ambiguedad entre nombres de los atributos y parametros pasados a un metodo
- devolver el objeto (ej, encadenar metodos)
- pasar el objeto a una funcion
#### Ejemplo
```cpp
#include <iostream> 

class MyClass { 
private: 
	int x; 
public: 
	MyClass(int val) : x(val) {} 
	
	void setX(int x) { 
		this->x = x; // Usando 'this' por la ambiguedad con x 
	} 
	
	MyClass* multiplyX(int factor) { 
		this->x *= factor; 
		return this; // Devolviendo 'this' para poder encadenar 
	} 
	
	void display() const { 
		std::cout << "x = " << x << std::endl; 
	} 
};
```
```cpp
int main() { 
	MyClass obj(10); 
	obj.display(); // Imprime: 10 
	obj.setX(20); 
	obj.display(); // Imprime: 20 
	// Metodos encadendo usando 'this' 
	obj.multiplyX(2)->display(); // Imprime: 40 
	return 0;
```

# Funcion Lambda 
#completar 