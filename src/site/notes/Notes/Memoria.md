---
{"dg-publish":true,"permalink":"/notes/memoria/","dg-note-properties":{"subject":["[[Paradigmas de la Programación]]"],"categories":["[[Lecture]]"],"clase":["[[Clase 3 - Manejo de Memoria]]"],"type":["Concept"],"date":"2026-03-17","tags":null,"topics":["Memoria"]}}
---

## Scope / Alcance 
- Una variable podra ser utilizada o existe dentro de un scope
- Si se declara dentro de una funcion, tendra un scope local (desde la declaracion hasta el fin de funcion)
- Tambien puede tener un bloque de alcance (scope block) si se declara dentro de un bloque `{ ... }`
``` cpp
#include <iostream> 
void f() { 
	int x = 5; 
	std::cout << "Dentro de la función f(): " << x << std::endl; 
} 

int main() { 
	int x = 15; 
	f(); 
	std::cout << "En main(): " << x << std::endl; 
	return 0;
}
```
```cpp
#include <iostream> 
int x = 5; //Variable global 

void f() { 
	std::cout << "Dentro de la función f(): " << x << std::endl; 
} 

int main() { 
	f(); 
	std::cout << "En main(): " << x << std::endl; 
	return 0; 
} 
```
## Memoria en Ejecución
La memoria se divide en 4 segmentos:
- **[[Notes/Memoria#Segmento de Codigo\|#Segmento de Codigo]]**: 
	- Contiene el codigo a ejecutar
	- Solo lectura 
	- Tiempo de vida largo
- **[[Notes/Memoria#Segmento de Datos\|#Segmento de Datos]]**:
	- Contiene **global** y **static** variables
	- Lectura y Escritura
	- Tiempo de vida largo
- **[[Notes/Memoria#Stack\|#Stack]]**:
	- Contiene variables **locales** 
	- Contiene los marcos de las funciones (con info necesaria para ejecutarla)
		parametros, direccion de retorno y variables
	- Lectura y Escritura
	- Tiempo de vida corto
- **[[Notes/Memoria#Heap\|#Heap]]**
	- Para almacenar variables requeridas dinamicamente
	- Accesibles mediante punteros o funciones como new, malloc, etc
	- Lectura y Escritura
	- Tiempo de vida corto
	- Manejada Manualmente
### Segmento de Datos 
Para definir una variable tipo *global* o *static*.
Se divide en dos partes
- una de solo lectura donde se almacenan constantes
#### Variables static
Las variables *static* retienen sus valores entre llamadas a funciones y solo se pueden inicializar una vez.
```cpp
void myFunction() { 
	static int contador; // Inicializada con cero una vez, se usa la misma variable que ya tiene un valor. 
	contador++; // La variable se incremente en cada llamada a la función. 
	std::cout << "Contador: " << contador << std::endl; 
} 

int main() { 
	myFunction(); // Imprime: Contador: 1 
	myFunction(); // Imprime: Contador: 2 
	myFunction(); // Imprime: Contador: 3 
	
	return 0; 
} 
```
### Stack
- Implementada como una Pila (First In Last Out)
- Tiene un limite en su tamaño y no se puede cambiar
- Se puede producir un *stack overflow* (recursion sin caso base)
![Pasted image 20260412162436.png](/img/user/Attachments/Pasted%20image%2020260412162436.png)
### Heap
- Se utiliza con punteros requiriendo memoria: `malloc`, `calloc`, `realloc` y `new`
- Se debe liberar utilizando `free` o `delete`
- Tamaño variable y accesible de forma global
- Si no se libera queda asignado y no se puede reutilizar, *memory leak*
	- degrada la performance de los programas en ejecucion
- Solo liberarla crea un **[[Notes/Memoria#dangling pointer\|#dangling pointer]]**, apunta a objeto borrado, puede causar bugs o errores.
	- Resoluble convirtiendolo en un **[[Notes/Memoria#nullptr\|#nullptr]]**, pero genera un ==***runtime errror***==
![Pasted image 20260412162856.png](/img/user/Attachments/Pasted%20image%2020260412162856.png)