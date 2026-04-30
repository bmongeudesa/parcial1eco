---
{"dg-publish":true,"permalink":"/notes/tipados-de-programacion/","dg-note-properties":{"subject":["[[Paradigmas de la Programación]]"],"clase":["[[Clase 1 - Introduccion a C++]]"],"type":["Concept"],"topics":null,"tags":null}}
---

## Tipado Estático
C++ utiliza este
``` cpp
int main() { 
	int valor = 10; 
	// ERROR DE COMPILACIÓN: 
	// error: invalid conversion from 'const char*' to 'int' 
	valor = "Hola"; 
	return 0; 
}
```
## TIpado Dinámico
Python usa este
``` python
valor = 10 # 'valor' apunta a un objeto de tipo int 
valor = "Hola" # Totalmente válido. Ahora apunta a un objeto de tipo str 
print(valor) # Salida: Hola
```
## Tipado Débil
C++ usa este
``` cpp
#include <iostream> 
int main() { 
	int numero = 5; 
	char letra = 'A'; // En ASCII, 'A' es el valor 65 
	
	// Válido. C++ promueve el 'char' a 'int' silenciosamente. 
	int resultado = numero + letra; 
	std::cout << resultado << "\n"; // Salida: 70 
	return 0; 
}
```
## Tipado Fuerte
Python usa este
``` python
numero = 5 
letra = 'A' 
# ERROR EN TIEMPO DE EJECUCIÓN: 
# TypeError: unsupported operand type(s) for +: 'int' and 'str' 
resultado = numero + letra
```