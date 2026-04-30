---
{"dg-publish":true,"permalink":"/notes/calificadores-const-y-static/","dg-note-properties":{"subject":["[[Paradigmas de la Programación]]"],"clase":["[[Clase 5 - Sobrecarga de Operadores]]"],"type":["Concept"],"topics":["[[OOP]]"],"tags":null}}
---

# *const*
### Metodos const
No pueden realizar cambios en el objeto, esto se logra impidiendo:
- cambiar valores de atributos dentro del metodo
- llamar metodos NO *const*
### Atributos const
Deben seguir las siguientes reglas:
- Inicializados cuando el objeto es creado
	- Si se inicializan en un [[Constructor\|Constructor]] DEBEN ser inicializados con [[Lista de Inicializacion\|Lista de Inicializacion]]
- *inmutables* (no pueden volver a cambiar)
- Solo `static const` pueden ser inicializadas (NO `static`) 
### Objetos const
No pueden ser modificados:
- Atributos declarados en el constructor
- Solo puede lamar metodos tipo `const` (**forzado por compilador**)

# mutable
Permite modificar un atributo con un método constante
#### Ej:
``` cpp
int MyClass::sInt = 8; 

int main() { 
	const MyClass obj(10); // Const object 
	obj.incrementCounter(); // Permitido 
	obj.incrementCounter(); // Permitido 
	std::cout << "Counter: " << obj.getCounter() 
	          << std::endl; // Imprime: Counter: 12 
	// obj.sInt no puede ser accedido, es private 
	return 0; 
}

class MyClass { 
private: 
	mutable int counter; // Atributo mutable 
	const int cInt; // Se debe inizialiar en el constructor 
	const int cIInt = 3; // Permitido 
	static int sInt; // Se debe definir fuera de la clase 
	//static int sIInt = 5; // No está permitido 
	static const int y = 4; // Permitido 
public: 
	MyClass() : counter(0) , cInt(3) {} 
	MyClass(int val) : counter(val) , cInt(3) {} 
	
	void incrementCounter() const { 
		counter++; // Permitido porque 'counter' es mutable 
	} 
	
	int getCounter() const { return counter; } 
}; 
```

# *static*
## Atributos
Variables que pueden ser compartidos entre todas las instancias de la clase (atributos de clase)
	La inicialización se hace fuera de la clase (aunque sean declarados dentro)
`static`
- se define dentro
- se debe inicializar fuera de la clase
`static const`
- se define dentro
- se debe inicializar dentro

``` cpp
#include <iostream> 
class MyClass { 
private: 
	static int counter; // Esta variable de clase debe inicializarse. 
	static const int PI = 3.14159; // Se debe definir 
public: 
	void incrementCounter() { counter++; } 
	static int getCounter() { return counter; } 
};

// Debe hacerse la Inicializacion del atributo static fuera de la clase 
int MyClass::counter = 0; // No es necesario instanciar una clase para acceder la variable 
```
```cpp
int main() { 
	MyClass obj; // Major definer static en constructor 
	obj.incrementCounter(); 
	obj.incrementCounter(); 
	std::cout << "Counter: " << obj.getCounter() 
			  << std::endl; // Output: Counter: 2 
			
	MyClass obj2; 
	obj2.incrementCounter(); 
	std::cout << "Counter: " << obj2.getCounter() 
			  << std::endl; // Imprime: Counter: 3 
	return 0; 
}
```
## Metodos
Pertenecen a la clase y NO a instancias particulares.
**Solo pueden acceder directamente a atributos y métodos también declarados static**

#### Ejemplo:
Uso común en librerías de matemática
- Se reciben valores como entrada para realizar calculos
- Las constantes suelen ser tipo static
``` cpp
#include <iostream> 
#include <cmath> // Para calcular la exponencial 
class Math { 
public: 
	// Constante tipo static 
	static const double SQRT_2PI; 
	
	// Metodo static para calcular la distribucion Gaussiana 
	static double gaussian(double x, double mean, double stddev) { 
		double exponent = -((x - mean) * (x - mean)) / (2 * stddev * stddev); 
		return (1 / (stddev * SQRT_2PI)) * std::exp(exponent); 
	} 
};

const double Math::SQRT_2PI = 2.5066282746310002; 
int main() { 
	double x = 0.0; // Punto a evaluar 
	double mean = 0.0; // Valor medio 
	double stddev = 1.0; // Desvio standard 
	
	// Se usa el metodo estático para calcular el valor 
	std::cout << "El valor de la gaussiana de x = " << x << " es " 
	<< Math::gaussian(x, mean, stddev) 
	<< " (o 1/SQRT_2PI)" << std::endl; 
	
	// Valor de la gaussiana en x = 0 es 0.398942 (o 1/SQRT_2PI) 
	return 0; 
}
```
