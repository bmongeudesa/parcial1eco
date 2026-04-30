---
{"dg-publish":true,"permalink":"/notes/polimorfismo/","tags":["completar"],"dg-note-properties":{"subject":["[[Paradigmas de la Programación]]"],"clase":["[[Clase 7 - Polimorfismo]]"],"type":["Concept"],"topics":["[[OOP]]"],"tags":["completar"]}}
---

## Definición
Significa que el objeto puede tomar distintas formas, en particular, permite que objetos de diferentes clases sean tratados como objetos de la clase base que tienen en común a través de punteros o referencias.
#### Ventajas
##### **Generalización de comportamiento**
La superclase tendrá definidos comportamientos genérico, y cada subclase deberá reemplazar este comportamiento con formas particulares.
##### **Reducción de dependencias entre componentes**
Permite tener código que sea independiente de implementaciones específicas.
##### **Flexibilidad en tiempo de ejecución**
Como todas las subclases pueden ser tratadas como la base, un contenedor tipado como la clase base puede contener las distintas subclases.

#### Ejemplo:
	
``` cpp
// Mismo mensaje, distintos comportamientos
Animal* a = new Perro(); 
a->hacerSonido(); // "Guau" 

// Codigo mas genérico
void hacerSonar(Animal* a) { 
	a->hacerSonido(); 
} 

// Extensibilidad / Reducir Dependencias 
class Gato : public Animal { 
public: 
	void hacerSonido() override { 
		std::cout << "Miau\n"; 
	} 
};
```
##### Evitar:
``` cpp
if (tipo == PERRO) ... 
else if (tipo == GATO) ...
```
##### Preferible
Permite mayor flexibilidad:
``` cpp
std::vector<Animal*> animales;
```

## Upcasting
Mediante el uso de un puntero o referencia podemos castear un objeto a su clase base, a esto lo llamamos `static upcasting`
``` cpp
A* aPtr = new C(1, 2, 4); 
B* bPtr = new C(7, 2, 1); 
C c(1,2,3); 
A& aRef = c;
```
![Pasted image 20260410194842.png\|271](/img/user/Attachments/Pasted%20image%2020260410194842.png)Se observa que la clase A tiene solamente la variable x, la clase B las x e y, y la C tiene x,y,z. 
En tiempo de compilación los objetos creados fueron limitados a la clase base (la variable tiene el tipo de la clase base, mientras que el objeto sigue siendo derivado)
NO hubo object slicing, solo fue limitada la vista (no implica copia)

#### Uso de Upcasting Estático
##### **Compatibilidad de tipos**:
Pasar objetos derivados como parametros de funciones que esperan la clase base
```cpp
void hacerSonar(Animal* a) {
	a->hacerSonido();
}
```
##### Almacenamiento de Objetos Heterogéneos
Amacenar objetos heterogéneos en contenedores de punteros o referencias a la clase base
```cpp
std::vector<Animal*> animales;
```

## Static Binding
Tambien llamado, `Metodo no Polimorfico` Es la forma por defecto de invocar métodos.
- Se usa cuando NO requiere comportamiento polimórfico
- El metodo a ejecutar es determinado en tiempo de compilacion
#### Ejemplo
*AnimalPtr es un puntero a Animal. Aunque apunte a un objeto Dog, al ejecutar speak() se llamará a Animal::speak(), mostrando:*`Soy un Animal`
``` cpp
#include <iostream> 
using namespace std; 

class Animal { 
public: 
	void speak() { cout << "Soy un Animal!" << endl;} 
}; 

class Dog : public Animal { 
public: 
	void speak() { cout << "Guau, guau!" << endl; } 
}; 
int main() { 
	Animal* animalPtr; Dog dog; 
	// Resuelve el tipo en tiempo de compilación 
	animalPtr = &dog; 
	// El animal sigue hablando como animal 
	animalPtr->speak(); 
	return 0; 
} // Acá no hay slicing, no hay copia por valor
```
## Dynamic Binding
Permite resolver que metodo ejecutar en **tiempo de ejecucion** utilizando el tipo del objeto y NO el de la declaracion.
- Los metodos de la base **deben** declarse como `virtual` 
	*Para usar dynamic binding y para permitir sobreescribir sus metodos por los de la derivada*
	Se los denomina **metodos polimórficos**
- En la clase derivada, la implementación de los métodos virtuales lleva la palabra `override`
	*la accesibilidad del métoo depende de la definicion en la clase derivada y de la clase base*
#### Ejemplo:
*La declaración de animalPtr se realiza con la clase Animal pero, a diferencia de static cast, los métodos que se ejecutan son los de los objetos que se pasan: dog y cat.*
```cpp
#include <iostream> 
using namespace std; 

class Animal { 
public: // Metodo virtual 
	virtual void speak() { cout << "Soy un Animal" << endl; } 
}; 

class Dog : public Animal { 
public: // Sobreescribe speak de animal con el de perro. 
	void speak() override { cout << "Guau y Bau" << endl; } 
}; 

class Cat : public Animal { 
public: // Sobreescribe speak de animal con el de gato. 
	void speak() override { cout << "Miau y Mow!" << endl; } 
};
```
``` cpp
int main() { 
	Animal* animalPtr; 
	Dog dog; 
	Cat cat; 
	// Resuelve en tiempo de ejecución usando RTTI 
	// (Run-Time Type Information) 
	animalPtr = &dog; // Dynamic binding => ladra, 
	animalPtr->speak(); // Guau y Bau 
	animalPtr = &cat; // Dynamic binding => maulla, 
	animalPtr->speak(); // “Miau y Mow!” 
	return 0; 
} 
```

## Implementación
Mediante [[Notes/Herencia\|Herencia]] y [[Notes/Polimorfismo#Dynamic Binding\|#Dynamic Binding]] se obtiene la implementación de polimorfismo.
- Una vez declarado un metodo como `virtual`, este lo será durante toda la cadena de herencia.
- Si una clase no sobreescribe un metodo virtual de su funcion base, la clase derivada hereda la implementacion de la base
- Permite escribir codigo general que el [[dynamic dispatch\|dynamic dispatch]] decida que metodo ejecutar segun el tipo real delobjeto
- Ayuda a la extensibilidad de software, nuevas subclases pueden ser creadas para usar el mismo software que usan otras subclases
- Al usar correctamente el polimorfismo NO se produce **object slicing**, dado que no se hace una copia del objeto

#### Ejemplo
```cpp
#include <iostream> 
using namespace std; 

class Enemy { 
public: 
	virtual void attack(){} 
};
class Goblin : public Enemy { 
public: 
	void attack() override { 
		cout << "Goblin atacan con mazo! -5 HP" << endl; 
	} 
}; 

class Skeleton : public Enemy { 
public: 
	void attack() override { 
		cout << "Esqueleto ataca con espada! -10 HP" << endl; 
	} 
};

class Dragon : public Enemy { 
public: 
	void attack() override { 
		cout << "Dragon ataca con fuego! -50 HP" << endl; 
	} 
}; 
```
``` cpp
int main() { 
	Enemy* enemies[] = { new Goblin(), 
						new Skeleton(), 
						new Dragon() };
	for (int i = 0; i < 3; i++) // Comienza ataque enemigo 
		enemies[i]->attack(); 
	for (int i = 0; i < 3; i++) // Se eliminan los punteros 
		delete enemies[i]; 
	return 0; 
} // Note que acá se ejecutan los métodos de las clases 
// derivadas y no el de la clase Enemy. 

```

