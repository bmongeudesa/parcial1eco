---
{"dg-publish":true,"permalink":"/notes/funciones-y-archivos/","dg-note-properties":{"subject":["[[Paradigmas de la Programación]]"],"clase":["[[Clase 2 - Conceptos Basicos C++]]"],"type":["Concept"],"topics":["[[C++]]"],"tags":null}}
---

# Funciones
Permiten
- legibilidad y seguimiento
- reutilizacion de codigo
- reduccion de mantenimiento
Se definen igual que en C
```cpp
tipo funcion(tipo arg, ...){
	return var;
}
```
Poseen 3 partes:

![Pasted image 20260412115402.png\|383](/img/user/Attachments/Pasted%20image%2020260412115402.png)
1. Prototipo
2. Llamada 
3. Definicion
## Recursion
- Funciones que se llaman a si mismas. 
- Reduce problemas a mas pequeños y repetitivos al original.
- Pueden generar problemas de performance
- Pueden ser optimizadas mediante [[memoization\|memoization]]
### Ejemplo
```cpp
#include <iostream> 
int nPower(int base, int exponent); 

int main() { 
	int x = 3, exp = 4; 
	std::cout << nPower(x, exp) << std::endl; 
	return 0; 
} 

int nPower(int val, int exp) { 
	if (exp == 0) { 
		return 1; // Case base: x^0 = 1 
	} 
	else { 
		return val * nPower(val, exp - 1); 
	} 
} 
```

## *inline*
Hace el prototipo y la definicion de una manera particular
```cpp
inline float cube(float val) {return val*val*val;}
```
- Avisa al compilador que las llamadas a la funcion **puede** ser reemplazada por el contenido de la funcion.
- **Ventaja**: es mas rapido, ya que evita muchas tareas
- **Desventaja**: el programa puede ser mas grande  (codigo repetido)
- Recomendable usar con funciones cortas usadas frecuentemente
- **Ventaja Macros:** type-safe, mas facil de debuggear y mantener

## Sobrecarga de Funciones
Es tener funciones con los mismos nombres pero distintos tipos de parámetros (funcionalidad de C++)
- El compilador se encarga de llamar a la funcion correspondiente
- Hace mas legible el codigo, mismo nombre para funciones con la misma tarea
```cpp
#include <iostream> 
int cube(int x); 
double cube(double y); 

int main() { 
	int x = 3 ; 
	double y = 5.5; 
	
	std::cout << cube(x) << std::endl; 
	std::cout << cube(y) << std::endl; 
	
	return 0; 
} 
	
int cube(int x) { return x*x*x ; } 
double cube(double y) { return y*y*y; }
// Imprime: 
// 27
// 166.375
```

# Calificador *const*
## *const*
Usado para obtener valores constantes
```cpp
const int x = 1;
```
Se puede definir por ejemplo el largo de un string:
```cpp
const int N = 5;
std::array<int, N> arr = {1, 2, 3, 4, 5};
```

### Como falla
Si se intenta modificar una constante o no inicializarla, resulta en un ==**error en tiempo de compilacion**==
``` cpp
const int N1 = 5; 
const int N2; // error en tiempo de compilación 
N1++; // error en tiempo de compilación 
```
### const en Funciones
Puede ser usado en la lista de parámetros de una función.
- Si el valor no se altera nunca, es buena practica hacerlo constante (tamb funciona con punteros y referencias)
- Se puede usar const en el valor a retornar (no hay diferencia)

```cpp 
const float cube(const float x) {return x*x*x;} 

int main(){ 
	float x = 3.3; 
	const float f1 = cube(x); // Misma funcionalidad
	float f2 = cube(x);  // Misma funcionalidad
	… 
	return 0; 
}
```

## *constexpr*
Se utiliza para indicar que una constante definida **puede ser** evaluada en **tiempo de compilacion**
- Permite mejorar la performance de algunos programas ("preprocesado" en compilacion)
Es el compilador el que decide realizar la evaluacion en tiempo de ejecucion o compilacion.

![Pasted image 20260412123840.png\|334](/img/user/Attachments/Pasted%20image%2020260412123840.png) Arriba, la dimension de arr se obtiene en tiempo de compilacion, por lo tanto se evalua en tiempo de compilacion. Abajo, la dimension del arr es evaluada recien en tiempo de evaluacion, por lo tanto se realiza en ese momento.
### Ejemplo Fibonacci
``` cpp
#include <iostream> 
constexpr int fib(int n) { 
	if (n == 0) 
		return 0; 
	if (n == 1) 
		return 1; 
	return fib(n - 1) + fib(n - 2); 
} 

constexpr int fibonacci[10] = { fib(0), fib(1), fib(2), fib(3), fib(4), fib(5), fib(6), fib(7), fib(8), fib(9) }; 

int main() { 
	for (int i = 0; i < 10; ++i) { 
		std::cout << fibonacci[i] << " "; 
	} 
	std::cout << std::endl; 
}
```

## *consteval*
Hace que la funcion **deba** ser evaluada en tiempo de compilacion (expansion de la funcionalidad de constexp)
``` cpp
constexpr int sumar(int x, int y) { return x + y; } 
consteval int multiplicar(int x, int y) { return x * y; } 

int main() { 
	int n = 3; 
	// Evaluado en tiempo de compilación o ejecución. 
	int a = sumar(2, 3); 
	
	// Debe ser evaluado en tiempo de compilación. 
	int b = multiplicar(2, 3); 
} 
```
*Si multiplicar recibe un parámetro definido como variable (la variable entera n), se tendrá un ==error de compilación== debido a que no es constante. Se arregla con `const int n = 3`*


# Archivos
(info basica a desarrollar en [[Archivos C++\|Archivos C++]])
Se hace uso de 2 librerias:
- una para leer: `ifstream`
- otra para escribir: `ofstream`
Se pueden escrbir en forma de texto o binario de acuerdos a los flags indicados

| Indicacion         | Descripcion                                                                |
| ------------------ | -------------------------------------------------------------------------- |
| `std::ios::in`     | Lectura (el default de ifstream)                                           |
| `std::ios::out`    | Escritura (el default de ofstream)                                         |
| `std::ios::binary` | Modo binario (el modo default es texto)                                    |
| `std::ios::app`    | Modo Append (agrega datos al final del archivo)                            |
| `std::ios::ate`    | Abre el archivo e ubica el cursor al final del mismo (lectura y escritura) |
| `std::ios::trunc`  | Borra el contenido del archivo a escribir.                                 |
### Ejemplos
#### Leer Archivo de texto
```cpp
#include <iostream> 
#include <fstream> 
#include <string> 

int main() { 
	// Abre el archivo en modo texto para leer 
	std::ifstream inFile("ArchivoTest.txt"); 
	if (inFile.is_open()) { 
		std::string line; 
		while (getline(inFile, line)) // Lee línea por línea 
			std::cout << line << '\n'; // Texto en pantalla 
		inFile.close(); // Cierra el archivo 
	} else 
		std::cerr << "Error el archivo no esta abierto!\n"; 
	// No es necesario cerrar el archivo manualmente, 
	//pero esto hace que se guarden los datos. 
	return 0; 
} 
```
#### Escribir Archivo de texto
```cpp
#include <iostream> 
#include <fstream> 
using namespace std; 

int main() { 
	ofstream outFile("ArchivoTest.txt"); 
	if (outFile.is_open()) { // Escribe por línea de texto 
		outFile << "Hola, bienvenidos a I102 PdeP!\n"; 
		outFile << "Escribir a un archivo de texto es facil.\n"; 
		outFile.close(); // Cierra el archivo 
		cout << "Archivo creado.\n"; 
	} else 
		err << "Error abriendo el archivo!\n"; 
	// No es necesario cerrar el archivo manualmente, 
	//pero esto hace que se guarden los datos. 
	return 0; 
} 
```

# Comandos del Preprocesador
Son comandos que se ejecutan en la etapa de preprocesamiento y generan codigo a compilar.
Se encuentran dentro de los archivos que ingresan al preprocesador:
![Pasted image 20260412134219.png](/img/user/Attachments/Pasted%20image%2020260412134219.png)
## Comando `#include`
Permite uncluir contenido del archivo indicado (standard de C/C++ o definido por usuario).
	Se puede considerar: que es reemplazado por el contenido del archivo

`#include <file>`:
	Busca archivos en un directorio predeterminado (standard include directories) para acceder a los headers solicitados
`#include "file.h"`:
	Busca el archivo header a incluir en el directorio del proyecto (pero se puede cambiar la ubicacion del archivo agregando un path)

==Si no se encuentran los archivos, los buscara en el flag de compilacion --l==

## Comando `#define`
Permite definir **macros y constantes** que son reemplazados en ==tiempo de compilacion==:
```cpp
#define nombreMacro tedxtoARemplazar
```
#### Ejemplos
```cpp
#define PI 3.14159 // Reemplazará la palabra PI por 3.14159 
#define MIN(a,b) ( ( (a) < (b) ) ? a : b ) // Macro para encontrar mínimo
```
*En caso de MIN*(a, b):* se reemplaza en tiempo de preprocesado pero se evalúa en tiempo de ejecución o compilación

## Comando `#undef`
Se utiliza para cancelar la definicion de una macro previamente definida. 
	Si se usa en una macro que no existe se ignora (no tira error)
```cpp
#include <iostream> 
#define PI 3.14159 
#undef PI 
int main () { // Error, PI no esta definido. 
	std::cout << "Value of PI: " << PI << std::endl; 
	return 0; 
}
```
## Instrucciones condicionales
Entre ellas: `#if, #elif, #else, #endif` 
Permiten incluir o excluir codigo basado en la condicion:
```cpp
#define FEATURE_1 1 // El feature 1 fue implementado 
#define TESTS_FEATURE_1 1 // El feature 1 paso las pruebas 
#define FEATURE_2 1 // El feature 2 fue implementado 
#define TESTS_FEATURE_2 0 // El feature 2 no paso las pruebas 

#if FEATURE_1 && TESTS_FEATURE_1 
	std::cout << "El feature 1 esta habilitado" << std::endl; 
#else 
	std::cout << "El feature 1 esta deshabilitado" << std::endl; 
#endif 

#if !TESTS_FEATURE_2 
	std::cout << "El feature 2 no pasó las pruebas " << std::endl; 
#elif FEATURE_2 
	std::cout << "Feature 2 esta habilitado para su instalación" << std::endl; 
#endif 
```

Tambien tenemos: `#ifdef, #ifndef, #elifdef` (C++23) y `#elifndef` (C++23).
Permiten incluir o excluir codigo basado en que una macro este o no definida (ifdef o ifndef)
```cpp
#define MIN_VELOC 100 
#define MAX_VELOC 200 

#ifdef MIN_VELOC 
	std::cout << "Hay una velocidad mínima" << std::endl; 
#elifndef MAX_VELOC 
	std::cout << "No hay limites de velocidad" << std::endl; 
#endif

#define MIN_VELOC 100 
#define MAX_VELOC 200 

#ifndef MAX_VELOC 
	#define MAX_VELOC 150 
#elifdef MIN_VELOC 
	#undef MIN_VELOC 
	#def MIN_VELOC 120 
#enfif 
// Si no está definida MAX_VELOC definirla. Si está definida eliminar definición de MIN_VEL y definirla nuevamente como 120. 
```

## Comando `#error`
Se utiliza para generar un error de compilacion con un mensaje definido
```cpp
#define MAIN_FEATURE 1 

#if MAIN_FEATURE 
	std::cout << "El feature principal esta habilitado" << std::endl; 
#else 
	#error "El feature principal no esta habilitado para las pruebas" 
#endif 
```

## Comando `#line`
Se utiliza para modificar el numero de linea y el nombre del archivo que reporta el preprocesador y el compilador
	Se utiliza con codigo generado automaticamente para facilitar debugging

![Pasted image 20260412135906.png](/img/user/Attachments/Pasted%20image%2020260412135906.png)

## Comando `#pragma`
Directiva que se usa para proveer instrucciones especiales o especificas al compilador.
El mas utilizado `#pragma once`
	Asegura que un determinado archivo no se incluya mas de una vez en compilacion
	Evita que ocurran problemas por multiples inclusiones del mismo header file

## Operadores `#` y`##`
`#` Reemplaza el texto con el mismo texto pero en comillas
`##` Utilizado para concatenar dos tokens
```cpp
#include <iostream> 
#define QUOTMRK( x ) #x 

int main () { 
	std::cout << QUOTMRK(HOLA C++) << std::endl; 
	return 0; 
} 
//Resulta en la línea de código: 
std::cout << "HOLA C++" << std::endl;
```

```cpp
#include <iostream> 
#define concat(a, b) a ## b 
int main() { 
	int xy = 100; std::cout << concat(x, y) << std::endl; 
	return 0; 
} 
// Resulta en la línea de código:
std::cout << xy << std::endl; 
```


# cont'd
[[Notes/Punteros y Referencias#Parametros a Funciones\|Punteros y Referencias#Parametros a Funciones]]