---
{"dg-publish":true,"permalink":"/notes/interfaces-y-metodos-virtuales/","dg-note-properties":{"subject":["[[Paradigmas de la Programación]]"],"clase":["[[Clase 7 - Polimorfismo]]"],"type":["Concept"],"topics":["[[OOP]]"],"tags":null}}
---

*continuacion del concepto [[Notes/Polimorfismo\|Polimorfismo]]*
## Destructor Virtual
### Problematica
Si NO se utiliza [[Notes/Polimorfismo#Dynamic Binding\|dynamic binding]], como es un objeto puntero a base:
- Se verá la leyenda de que se ha ejecutado el destructor de Base
- Pero no el de Derived
Si se borrara *der* saldria correcto
``` cpp
#include <iostream> 
class Base { 
public: 
	Base() { std::cout << "Constructing base\n"; } 
	~Base() {std:: cout<< "Destructing base\n"; } 
}; 

class Derived: public Base { 
public: 
	Derived() {std:: cout << "Constructing derived\n"; } 
	~Derived() {std:: cout << "Destructing derived\n"; } 
};
 
int main() { 
	Derived *der = new Derived(); 
	Base *base = der; 
	delete base; 
	return 0;
}
```
##### prints
```
Constructing base 
Constructing derived 
Destructing base
```

### Solucion
Se agrega la palabra `virtual` al destructor, y al borrar el objeto:
- se llama primero al destructor de *Derivada*
- luego se llama al destructor *Base*
- Ocurre si se borra *der* o *base*
``` cpp
#include <iostream> 
class Base { 
public: 
	Base() { std::cout << "Constructing base\n"; } 
	virtual ~Base() {std:: cout<< "Destructing base\n"; } 
}; 

class Derived: public Base { 
public: 
	Derived() {std:: cout << "Constructing derived\n"; } 
	~Derived() {std:: cout << "Destructing derived\n"; } 
}; 

int main() { 
	Derived *der = new Derived(); 
	Base *base = der; 
	delete base; 
	return 0; 
}
```
##### prints
```
Constructing base 
Constructing derived 
Destructing derived 
Destructing base
```

## Metodo Virtual Puro
Se definen asi:
```cpp
virtual void doSomething() = 0;
```
Al definir un metodo de esta manera, la clase se convierte en una [[Notes/Interfaces y Metodos Virtuales#Clase Abstracta\|#Clase Abstracta]] o [[Notes/Interfaces y Metodos Virtuales#Interfaces\|Interfaz]], por lo que no puede ser instanciada.
Solo podran ser definidos en las clases derivadas y no en las bases.
#### Virtual vs Virtual Puro
- `metodo virtual`: implementacion que lo define y le da a las derivadas la opcion de sobreescribirlo (`override`)
- `metodo virtual puro`: no tiene implementacion en la base (donde se define) y DEBE ser sobreescrito (`override`) en la clase derivada
```cpp
class Interfaz { 
public: 
	virtual ~Interfaz() = default; 
	virtual void metodo() = 0; 
};
```

# Interfaces y Clases Abstractas
Generalizar clases es permitido por la herencia y el polimorfismo, y las interfaces y clases abstractas son las que nos permiten asegurar que ciertos metodos siempre sean implementados.![Pasted image 20260411124917.png](/img/user/Attachments/Pasted%20image%2020260411124917.png)
## Interfaces
Son una clase en la que: 
- todos los metodos son [[Notes/Interfaces y Metodos Virtuales#Metodo Virtual Puro\|virtuales puros]]
- no puede instanciarse directamente
- no poseen atributos
*son como un contrato que establece normas para sus derivadas*

Al ser heredadas, sus metodos virtuales **deben** ser implementados (pq son virtuales puros). El tener que implementar si o si los metodos agrega complejidad y requiere precaucion al diseñar jerarquia de clases.
## Clases Abstractas
Son una clase en la que:
- algunos de los metodos son [[Notes/Interfaces y Metodos Virtuales#Metodo Virtual Puro\|virtuales puros]] (no necesariamente todos)
- no pueden instanciarse directamente
	pero al ser heredadas esas clases deben implmentar los metodos virtuales
- puede poseer atribtos

Se utiliza cuando hay datos o comportamientos comunes entre todas las clases derivadas.

Una clase abstracta puede ser una clase derivada de una interfaz sin implementar todos sus metodos. Incluso puede agregar mas funciones virtuales puras.
	Si esto sucede, y otra clase (no abstracta) hereda de la clase abstracta como base, esta debera de implementar todos los metodos virtuales puros de la interfaz y de la abstracta.

