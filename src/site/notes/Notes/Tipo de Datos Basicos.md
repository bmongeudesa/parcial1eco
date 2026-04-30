---
{"dg-publish":true,"permalink":"/notes/tipo-de-datos-basicos/","dg-note-properties":{"subject":["[[Paradigmas de la Programación]]"],"clase":["[[Clase 2 - Conceptos Basicos C++]]"],"type":["Concept"],"topics":["[[C++]]"],"tags":null}}
---

# Variables
## Tipos de Variables
Existen las tipicas:
```
bool
char
int 
float 
double 
void
```
### bool
1 byte - pero usa 1bit
``` cpp
bool a; // Por default un bool se inicializa con false (cero)
bool b = true; 
bool c{false}; 
```

`std::boolalpha` escribe al booleano con su valor alfanumérico (true o false)

### char
1byte - rango entre -128 y 127
``` cpp
char a; 
char b{'+'}; // + en char 
char c = 0b00101011; // + en binario 
char d = 053; // + en octal 
char e = 43; // + en decimal 
char f = 0x2b; // + en hexadecimal
```
#### No gráficos
![Pasted image 20260406152530.png](/img/user/Attachments/Pasted%20image%2020260406152530.png)
Tabla ASCII: es.wikipedia.org/wiki/ASCII

### int
Tipos de enteros para Windows
![Pasted image 20260406152723.png](/img/user/Attachments/Pasted%20image%2020260406152723.png)
``` cpp
unsigned int a = 85; // decimal 
long long b{07747677}; // octal (usar cero antes del número) 
short c = {0xA}; // hexadecimal (usar 0x) 
short int d = +32768;
```
### Punto flotante
En C++ existen:
- float: presicion simple 32 bits
	- 8 bits exponente
	- 23 bits mantisa
- double: presicion doble 64 bits
	- 11 bits exponente
	- 52 bits mantisa
- long double: precision cuadruple 128 bits
	- 15 exponente
	- 112 mantisa
![Pasted image 20260406153110.png](/img/user/Attachments/Pasted%20image%2020260406153110.png)

``` cpp
#include <iostream> 
#include <iomanip> 

using namespace std; 

int main(){ 
	long double value = 3.1415926535897932384626433832795; 
	cout << "El valor es: " << setprecision(30) << value << endl; 
	// El valor es: 3.14159265358979311599796346854 
	return 0;
}
# sin setprecision se muestra: 3.14159
```
`setprecision()` permite proveer toda la informacion en pantalla

![Pasted image 20260406153414.png](/img/user/Attachments/Pasted%20image%2020260406153414.png)
Si se quiere usar el **float**:
```
	#include <iomanip>
```
### void 
El compilador reconoce la leyenda *void* como "no tipo".
- Se utiliza para funciones que no devuelven nada
### void*
Se interpreta como un puntero a una variable que no fue definida aún.
Las funciones `malloc/calloc/etc` devuelven void* y deben ser casteadas
``` cpp
int *myArr = (int*) malloc( size*sizeof(int) )
```

## Casting
### static_cast
Casting mas basico y seguro de C++
- Tiene verificacion de tipo de dato
```cpp
static_cast<DataType>(Variable)
```

```cpp
#include <iostream> 

int main() { 
	int n = 10; // En hexa es: 0xA 
	// Con el cast tradicional, compila y muestra 0xa 
	void* ptr = (void*)n; // Define posiciones de memoria con valores 
	int std::cout << ptr << std::endl; 
	
	// Con static_cast se tendrá: Invalid type conversion 
	void* ptr2 = static_cast<void*>(n); // No compila por esta linea 
	
	return 0;
}
```

# Entrada y Salida de Datos
Se realizan mediante `cout` y `cin` respectivamente,
Estos son objetos de las clases `std::istream` y `std::ostream`
- `namespace std`: evita tener q repetir *std::*
- `endl`: es un salto de linea (manipulador de flujo)
### Salida
```cpp
include <iostream> 
int main(){ 
	std::cout << "Hello, World!" << std::endl; 
	return 0; 
}
```
```cpp
include <iostream> 
using namespace std; 

int main(){ 
	cout << "Hello, World!" << endl; 
	return 0;
}
```

### Entrada
La entrada `std::cin` y el operador de extracción `>>`

#### Recordatorios
`cin >> var:`
- Lee datos desde el buffer hasta el primer whitespace (espacio, tab o \n). 
- Ignora espacios en blanco iniciales automáticamente. 
- No permite leer cadenas con espacios. 
- `Deja   el   '\n'   en   el   buffer` 

`getline(cin, var):`
- Lee todos los caracteres hasta encontrar un '\n'. 
- Permite leer espacios dentro de la cadena. 
- No ignora automáticamente whitespace inicial. 
- Consume y descarta el '\n' del buffer. 
- Si hay un '\n' pendiente, puede devolver una cadena vacía. 

#### Codigo
```cpp
#include <iostream> 
#include <string> 
using namespace std; 

int main() { 
	string name, fullName; 
	cout << "Ingrese su nombre: "; 
	cin >> name; 
	cout << "Ingrese su nombre completo: "; 
	cin >> ws; // Elimina los whitespaces del buffer 
	getline(cin, fullName); 
	
	cout << "Hola, " << name << "!" << endl; 
	cout << "Tu nombre completo es: " << fullName << endl; 

	return 0;
}
```
El nombre ingresado por el usuario se almacena en la variable name, que es de tipo `std::string`

Cuando el usuario ingresa el nombre y presiona Enter, los caracteres escritos (incluido el carácter de salto de línea `\n`) se almacenan en el buffer de entrada.

El operador `>>` lee caracteres hasta encontrar un espacio en blanco y deja el carácter '\n' pendiente en el buffer.

Luego, cuando se utiliza `std::getline`, esta función lee hasta encontrar un salto de línea. Si ese `\n` ya estaba pendiente en el buffer, getline lo encuentra inmediatamente y devuelve una cadena vacía

Para evitar este problema, se eliminan los caracteres de espacio en blanco pendientes utilizando `cin >> ws;`
#### Precaucion!
Analizar diferencia
##### Codigo Problematico
```cpp
#include <iostream> 
#include <string> 
using namespace std; 

int main() { 
	string name, fullName; 
	
	cout << "Ingrese su nombre: "; 
	cin >> name; 
	cout << "Ingrese su nombre completo: "; 
	getline(cin, fullName); 
	
	cout << "Hola, " << name << "!" << endl;
	cout << "Tu nombre completo es: " << fullName << endl; 
	
	return 0; 
}
```
##### Solucion (ws)
```cpp
#include <iostream> 
#include <string> 
using namespace std; 
int main() { 
	string name, fullName; 
	
	cout << "Ingrese su nombre: "; 
	cin >> name; 
	cout << "Ingrese su nombre completo: "; 
	cin >> ws; // Elimina los whitespaces del buffer 
	getline(cin, fullName); 
	
	cout << "Hola, " << name << "!" << endl; 
	cout << "Tu nombre completo es: " << fullName << endl; 
	
	return 0; 
}
```
#### Multiples Entradas
``` cpp
#include <iostream> 
#include <typeinfo> 

int main(){ 
	char c = ‘a’; int i = 0; double d = 0.0; 
	
	std::cout << "Ingrese un carácter, un entero y una variable tipo double (separados por espacios): "; 
	std::cin >> c >> i >> d; 
	std::cout << "Usted ingreso: " << c << ", " << i << ", " << "y " << d << std::endl; 
	std::cout << "Tipo de dato de c: " << typeid(c).name() << std::endl; 
	std::cout << "Tipo de dato de i: " << typeid(i).name() << std::endl; 
	std::cout << "Tipo de dato de d: " << typeid(d).name() << std::endl; 
	return 0; 
}

```


# Otro tipo de Variables
## Arrays
 Datos ordenados por un indice que posee una cantidad de elementos fijos y no puede ser cambiada en tiempo de ejecución
Formas de declararlo
```cpp
int myArr[4] = {2, 3, 6, 1}; 
int myArr2[4] = {2}; 
int myArr3[] = {10, -8, 13, 17}; 
```
Otra forma sin asignacion
```cpp
char myStr[20];
```
Con input
```cpp
char myStr[20]; 
std::cin >> myStr; 
```
### Matrices o Arrays Multidimensionales
```cpp
#include <iostream> 
int main(){ 
	int myMtx[4][3] = {{1, 2, 3}, {4, 5, 6}, {7, 8, 9}, {10, 11, 12}}; 
	std::cout << myMtx[2][0] << std::endl; // Imprime el valor 7 
	std::cout << myMtx[1][1] << std::endl; // Imprime el valor 5 
	std::cout << myMtx[3][2] << std::endl; // Imprime el valor 12 
	return 0;
}
```

## std:string
Es una variable de tipo string implementada con una clase
```cpp
#include <iostream> 
#include <string> 
int main(){ 
	std::string s; 
	std::string s1 = "Hello "; 
	std::string s2 = "World"; 
	char c = '!';
	
	s = s1; 
	s += s2; // Lo mismo que hacer s = s1 + s2 
	s += c; // También permite concatenar chars 
	std::cout << s << std::endl; 
	
	return 0; 
} // Imprime: Hello World!
```
#### Funciones que posee
- `s.at(i) / s[i]`: permite acceder el carácter i-esimo del string ‘s’ 
- `s1 == s2`: compara el contenido de los dos strings ‘s1’ y ‘s2’. 
- `s.c_str():` retorna un puntero a su primer elemento. Esta este puntero permite utilizar el string ‘s’ como un array de chars, es útil cuando una función requiere que se le pase un puntero a char y no un string. 
- `s.substr(x, y):` retorna un std:string de ‘s’ que comienza en la posición x y tiene una longitud y. 
- `s.find(char|string):` busca un substring en ‘s’. Si se encuentra, retorna la primer posición de la primer ocurrencia de la substring. Si no encuentra la substring, se retorna el valor `std::string::npos`.

## Enum y Enum Class
Funcionan similarmente en C.
Con `class` se evitan conflictos con los nombres de los valores al utilizar la referencia
```cpp
#include <iostream> 
int main(){ 
	enum class Meses {ENERO=2, FEBRERO, MARZO, ABRIL, MAYO, JUNIO,
		 JULIO, AGOSTO, SEPTIEMBRE, OCTUBRE, NOVIEMBRE, DICIEMBRE}; 
	// Si Nombres no fuera class, habria conflicto por JULIO 
	enum class Nombres {CERGIO, JUAN, JULIO}; 
	
	std::cout << static_cast<int>(Meses::JULIO) << std::endl; 
	std::cout << static_cast<int>(Nombres::JULIO) << std::endl; 
	//std::cout << Meses::MARZO << std::endl; // ESTO PRODUCE UN ERROR! 
	return 0; 
} // Imprime 8 y 2 (valores de mes y nombre julio)
```


# Deduccion Automatica de Tipos
C++ puede deducir automaticamente el tipo de variable con el calificador `auto`
- Solo se puede utilizar para inferir un tipo en tiempo de compilacion
- Hay posibilidad de que el resultado sea ambiguo (ej con int y char)
``` cpp
#include <iostream> 
using namespace std; 

int func1 (){return 1} 
double func2 (){return 2.5;} 

int main(){ 
	auto a = 1, b = 2, c = 3; 
	auto d = 4, e = 5; 
	auto f = '7'; // ASCII Dec = 55 
	auto g = a+b+c+d+e+f;
	
	cout << "La variable a es tipo: " << typeid(a).name() << endl; 
	cout << "La variable f es tipo: " << typeid(f).name() << endl; 
	cout << "La variable g es tipo: " << typeid(g).name() << endl; 
	cout << "El valor de g es: " << g << endl; 
	
	// El resultado puede no ser el esperado 
	auto result = func2() + func1(); 
	cout << "Result es tipo: " << typeid(result).name() << endl; 
	cout << "El valor de result es: " << result << endl; 
	
	return 0; 
} 

```