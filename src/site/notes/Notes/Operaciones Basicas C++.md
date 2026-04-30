---
{"dg-publish":true,"permalink":"/notes/operaciones-basicas-c/","dg-note-properties":{"subject":["[[Paradigmas de la Programación]]"],"clase":["[[Clase 2 - Conceptos Basicos C++]]"],"type":["Concept"],"topics":["[[C++]]"],"tags":null}}
---

# Operadores
Hay distintos tipos
1. Operador de asignación (`=`) y comparación (`==`) 
2. Operador aritmético (operador binario): 
	`+, -, *, /, %`: suma, resta, multiplicación, división y módulo, 
3. Operador de bits (operadores unarios y binario): 
	`&, |, ^, <<, >>, ~`: AND, OR, XOR, shift izquierda, shift derecha y negación. 
4. Operador compuesto de asignación (operador binario): 
	`+=, -=, *=, /=, %=, &=, |=, ^=`: realiza la operación y asigna el resultado. 
5. Operadores de incremento y decremento (operador unario): 
	`++i, --i`: pre incremento/decremento, suma 1 y luego hace operaciones.
	 `i++, i--`: post incremento/decremento, hace operaciones y luego suma 1.
#### Ejemplo
```cpp
#include <iostream> 
int main(){ 
	int x = 1, y = 1; 
	
	std::cout << x++ * y++ << std::endl; 
	// imprime 1 (1*1), pero serán x = 2 e y = 2 
	std::cout << x++ * ++y << std::endl; 
	// imprime 6 (2*3), pero serán x = 3 e y = 3 
	std::cout << --x * y++ << std::endl; 
	// imprime 6 (2*3), pero serán x = 2 e y = 4 
	std::cout << x-- * --y << std::endl; 
	// imprime 6 (2*3), pero serán x = 1 e y = 3 
	std::cout << x * y << std::endl; 
	// imprime 3 (1*3), pues serán x = 1 e y = 3 
	
	return 0; 
}
```

# Sentencias
## Condicionales
### Operador Ternario
```cpp
Condición ? Expresión 1 : Expresión 2
```
Si la condición es verdadera, la expresión 1 es ejecutada, sino, la expresión 2.
- La condición a verificar se escribe mediante los operadores lógicos.

| Operador | Operación       |
| -------- | --------------- |
| &&       | AND             |
| \|\|     | OR              |
| !        | Negacion        |
| <        | Menor que       |
| >        | Mayor que       |
| ==       | Igual a         |
| <=       | Menor o igual a |
| >=       | Mayor o igual a |
| !=       | No es igual a   |
![Pasted image 20260412014110.png\|288](/img/user/Attachments/Pasted%20image%2020260412014110.png)
### if/else
Se utilizan igual que en c
### switch
- Igual que en C solo que se pueden utilizar `enum class`
- Permite evitar problemas de nombrado
- Permite tipear los datos en el enum `short int`
```cpp
#include <iostream> 
using namespace std; 

enum class COLORES: short int { ROJO, VERDE, AZUL }; 

int main() { 
	COLORES colorAuto = COLORES::ROJO; 
	
	switch (colorAuto) { 
		case COLORES::ROJO: 
			cout << "El auto es rojo." << endl << endl; break; 
		case COLORES::VERDE: 
			cout << "El auto es verde." << endl << endl; break; 
		case COLORES::AZUL: 
			cout << "El auto es azul." << endl << endl; break; 
		default: 
			cout << "No es un color contemplado." << endl; 
	} 
	return 0; 
} // Imprime: El auto es rojo
```
## De Iteracion
### while y do/while
Igual que en C: Repiten una porción de código y se detiene cuando la condición se cumple (si la terminación no se fuerza)
![Pasted image 20260412014629.png](/img/user/Attachments/Pasted%20image%2020260412014629.png)
- Si la condición no se cumple, en el `do/while`, se ejecuta el contenido entre llaves sólo una vez, mientras que en el while, no se ejecuta nunca.
### for 
#### de Condiciones
Igual a C pero con funciones extra:
- Las expresiones 1 y 3 permiten definir y modificar una variable que se puede evaluar en la condición para continuar la iteración.
![Pasted image 20260412014736.png](/img/user/Attachments/Pasted%20image%2020260412014736.png)
#### de rango
Permite iterar sobre rangos de variables como ser un array, un std::vector, o algún otro container
	Usualmente el tipo de elemento se define como auto y lo que recorre es el contenido del contenedor
##### Ejemplo
```cpp
#include <iostream> 
#include <array> 

int main(){ 
	std::array<int, 6> v = { 0, 1, 2, 3, 4, 5 }; // definición de 
	std::array int a[] = {0, 1, 2, 3, 4, 5}; 
	
	for ( const int& i : v ) // accede por referencia constante el valor entero 
		std::cout << i << ""; std::cout << "\n"; 
	for ( auto i : v ) // accede por valor, el tipo de i es int 
		std::cout << i << ""; std::cout << "\n"; 
	for ( auto n : a ) // para el array ‘a’ 
		std::cout << n << ""; 
	return 0; 
} 
```

### break y continue
- `break` interrumpe la iteracion saliendo del ciclo
- `continue` interrumpe la iteracion acutal pero continua con el ciclo
![Pasted image 20260412015319.png](/img/user/Attachments/Pasted%20image%2020260412015319.png)
# Error Handling
## Try / Catch y Throw
Los comando`try` y `catch` funcionan juntos para manejo de expeciones. 
El bloque `try` define un scope actual, si se ejecuta un `throw`, la ejecucion normal se interrumpe y se transfiere el control al `catch` correspondiente 
	*(se abandona el scope de try y se ingresa al de catch)*
Cuando se ejecuta un `throw` se deja de ejecutar el bloque donde ocurre y se comienza la busqueda de un `catch` compatible, si no existe uno, el programa termina su ejecucionj
```cpp
try { 
	// Codigo que puede llegar a arrojar una excepcion } 
catch (exception_type e) { 
	// Maneja la excepción de tipo exception_type } 
catch (...) { 
	// Maneja cualquier tipo de excepción (catch-all)
}
```
`throw+mensaje` es un tipo texto dado por una cadena de caracteres. `catch(...)` tambien funciona con `throw`, pero se usa cuando no es uno especifico
```cpp
#include <iostream> 
int main() { 
	try { 
		int x = 10, y = 0; 
		if (y == 0) // Arroja un error con el texto dado. 
			throw "Division por zero!"; 
		int result = x / y; 
	} catch (const char* e) { // Captura la excepcion 
		std::cout << "Error: " << e << std::endl; 
	} // Imprime el error con el texto dado en throw. 
	std::cout << "El program continua..." << std::endl; 
	return 0; 
}
```

Pueden producirse distintos tipos de errores
- Se pueden definir múltiples bloques catch, cada uno asociado a un tipo de excepción distinto
- Se evalúan en orden, y el primero cuyo tipo sea compatible con la excepción lanzada es el que la captura
- Si la excepción no coincide con ninguno de los tipos especificados, se ejecuta el bloque catch(...), que actúa como manejador genérico

```cpp
#include <iostream> 
#include <stdexcept> // Para atrapar el std::runtime_error 

int main() { 
	try { 
		throw std::runtime_error("Algo salió mal!"); 
		//throw "Algo salió muy mal!"; 
		//throw std::logic_error("Una cuenta salió muy mal!"); 
	} catch (const std::runtime_error& e) { // Objeto con & 
		std::cout << "Runtime error: " << e.what() << std::endl; 
	} catch (const char* e) { // cadena de caracteres char* 
		std::cout << "Error: " << e << std::endl; 
	} catch (...) { // cualquier otro tipo 
		std::cout << "Arrojó una excepción desconocida" << std::endl; 
	} 
	return 0; 
} 
```
