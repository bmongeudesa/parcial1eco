---
{"dg-publish":true,"permalink":"/notes/punteros-y-referencias/","dg-note-properties":{"subject":["[[Paradigmas de la Programación]]"],"clase":["[[Clase 3 - Manejo de Memoria]]"],"type":["Concept"],"topics":["[[C++]]"],"tags":null}}
---

# Punteros
Son variables que almacenan una direccion a memoria
- Utilizables como en C
- Permiten gestion eficiente de memoria
- Se liberan con `free`
#### Ejemplo
```cpp
#include <iostream> 
int main(){ 
	int *p1 = 0; // Declaración e inicialización a cero 
	int *p2 = NULL; // Declaración null pointer 
	int *p3 = nullptr; // Declaración null pointer 
	int *p4; // Declaración sin inicializar 
	int *p5 = (int*)malloc(100*sizeof(int)); 
	
	free(p1); 
	free(p2); 
	free(p3); 
	free(p4); 
	free(p5); 
	return 0; 
} 
```
==recordar:== un puntero es una direccion **con tipo**
### new
Al definir un [[Notes/Punteros y Referencias#raw pointer\|#raw pointer]], lo hacemos con la keyword `new`
- no requiere un cast, es type safety (a diferencia de malloc)
- valores del array se inicializan en 0 (como en calloc)
- si se utiliza con objetos, ==llama al constructor default== del mismo
```cpp
double *ptrD = new double; 
int* ptrI = new int[100];
```

### delete
Libera la memoria a la que esta apuntando el puntero
- usado en conjunto con new
- en caso de objetos, ==llama al destructor default== del mismo
```cpp
delete pointer;
delete[] ptrll;
```

#### Ejemplo:
```cpp
#include <iostream> 
int main(){ 
	// Inicializar contenido con 3 
	int *iP = new int(3); 
	// Inicializar contenido con 8.3 
	float *fP = new float(8.3); // Inicializar array con contenido default 
	int *iArrP1 = new int[30]; // Inicializar contenido con 5, 3, 1 de un array de 3 elementos 
	int *iArrP2 = new int[3]{5, 3, 1}; 
	
	delete iP; delete fP; 
	delete[] iArrP1; delete[] iArrP2; 
	
	return 0; 
}
```

## Aritmetica de Punteros
![Pasted image 20260412164227.png](/img/user/Attachments/Pasted%20image%2020260412164227.png)

#### Ejemplo
```cpp
#include <iostream> 
void imprimirArray(int arr[], int size) { 
	for (int i = 0; i < size; i++) { 
		std::cout << arr[i] << " - "; // Elementos mediante índice 
		// Elementos con aritmética de punteros 
		std::cout << *(arr+i) << std::endl; 
	} 
	std::cout << std::endl; 
} int main() { 
	int vals[] = {10, 20, 30, 40, 50}; // Calcula el tamaño del array 
	int size = sizeof(vals) / sizeof(vals[0]); // El nombre del array es la dirección de su primer elemento 
	imprimirArray(vals, size); 
	return 0; 
} 
// Ambas formas imprimen la secuencia 10 20 30 40 50
``` 

## Smart Pointers
Se introdujeron en C++ moderno con la libreria `#include <memory>`
- Son clases con [[Notes/Punteros y Referencias#templates\|#templates]] 
- Eliminan la necesidad de liberar memoria
- Evitan *memory leaks*
### *unique_ptr*
Se puede definir como:
```cpp
std::unique_ptr<int> ptr1 = std::make_unique<int>(3);
// tambien es valido
std::unique_ptr<int> p (new int{10}); 
```
- El puntero NO puede compartirse
	- Si se intenta tira error
- Puede cambiar de ownership:
	- Se logra con `std::move`
	- Se libera el puntero original

![Pasted image 20260412165104.png](/img/user/Attachments/Pasted%20image%2020260412165104.png)
#### Ejemplo
```cpp
#include <iostream> 
#include <memory> 

void myFunc() { 
	// Se crea el puntero 
	std::unique_ptr<int> p (new int{10}); 
	// Cambia el ownership de p a ptr 
	std::unique_ptr<int> ptr = std::move(p); 
	// Imprime 10 
	std::cout << *ptr << std::endl; 
	// Imprime 10 
	std::cout << *ptr.get() << std::endl; 
	// Imprime la dirección de memoria 
	std::cout << ptr.get() << std::endl; 
	// Se liberara la memoria automáticamente al llegar al 
} 

int main() { 
	myFunc(); 
	return 0;
}
```
### *shared_ptr*
Se definen como:
```cpp
std::shared_ptr<int> ptr2 = std::make_shared<int>(4);
```
- Pueden compartir direcciones
- Tienen un contador de cantidad de punteros
- La memoria se libera cuando TODOS los punteros salen del scope
![Pasted image 20260412165739.png](/img/user/Attachments/Pasted%20image%2020260412165739.png)
#### Ejemplo
```cpp
#include <iostream> 
#include <memory> 

void myFunc() { 
	std::shared_ptr<int> ptr1 = std::make_shared<int>(10); 
	// scope block
	{ // ptr2 comparte la dirección con ptr1 
		std::shared_ptr<int> ptr2 = ptr1; 
		std::cout << *ptr2 << std::endl; // Imprime 10 
		std::cout << ptr1.use_count() << std::endl; // Imprime 2 
	} // Solo ptr2 sale de scope, ptr2 se elimina, pero no ptr1 
	std::cout << ptr1.use_count() << std::endl; // Imprime 1 
} // La memoria es liberada al salir ptr1 de scope 

int main() { 
	myFunc(); 
	return 0; 
} 
```

#### Dependencia Ciclica
Problematica comun con los *shared_ptr*, 

```cpp
struct A; 
struct B { 
	std::shared_ptr<A> ptrA; // B tiene un shared_ptr a A 
	~B() { std::cout << "B es liberado\n"; }
}; 

struct A { 
	std::shared_ptr<B> ptrB; // A tiene un shared_ptr a B 
	~A() { std::cout << "A es liberado\n"; }
}; 

int main() { 
	std::shared_ptr<A> a = std::make_shared<A>(); // +1
	std::shared_ptr<B> b = std::make_shared<B>();  // +1
	// Se crea la referencia ciclica: 
	a->ptrB = b; // +1
	b->ptrA = a; // +1
	// Los punteros de A y B no se liberan porque aun 
	// existen dos referencias, los count de a y b son 2. 
	std::cout << "El use_count de a es: " << a.use_count() << std::endl; 
	// Imprime 2 
	std::cout << "El use_count de b es: " << b.use_count() << std::endl; 
	// Imprime 2 
	return 0; 
} // No se ejecutan ~A() y ~B() porque los count > 1 
```
- Se soluciona seteando cualquiera de los 2 (solo uno) dentro de la clase como un `weak_ptr`
### Matriz de Smart Pointers
- Para crear un array unidimensional se pasa la longitud del array en el parentesis
	`std::unique_ptr<int[]> arr1D = std::make_unique<int[]>(10);`
- Para 2 dimensiones, se crea un array de arrays
	- primero 2 posiciones de unique_pt (inicialmente vacio) pero contendra punteros a arrays de a int
		`	std::unique_ptr<std::unique_ptr<int[]>[]> arr2D = std::make_unique<std::unique_ptr<int[]>[]>(filas); `
	- se asignan los puinteros a los arrays para las 2 posiciones
	- se recorre la matriz y se imprime su contenido
#### Codigo
```cpp 
#include <iostream> 
#include <memory> 
int main () { 
	// Crear un array de 10 unique pointers 
	std::unique_ptr<int[]> arr1D = std::make_unique<int[]>(10); 
	int filas = 2, columnas = 5; 
	
	// Crear un array de 2 unique pointers, cada uno inicialmente 
	// vacío, que luego podrá almacenar un array de 5 elementos. 
	std::unique_ptr<std::unique_ptr<int[]>[]> arr2D = std::make_unique<std::unique_ptr<int[]>[]>(filas); 
	
	for ( int i = 0; i < 2; i++ ) { 
			// Aqui se define el largo de los array en cada una de las 2 posiciones del vector de array. 
			arr2D[i] = std::make_unique<int[]> ( columnas ); 
			for ( int j = 0; j < 5; j++ ) { 
				arr2D[i][j] = j; // Se accede como un array normal. 
				std::cout << arr2D[i][j]; 
			} 
		std::cout << std::endl; 
	} 
	
	return 0; 
}
```

# Referencias
Es un *alias* a una variable en memoria, se logra mediante la asignacion de un ampersand `&` LUEGO del tipo de variable
```cpp
nombreTipo x = <debe ser inicializada>; 
nombreTipo &y = x;
```
==*No confundir con acceder a la direccion de memoria de una variable*==
Si se modifica x o y, ambos reciben el cambio
	Es como que dos variables acceden a la misma pos en memoria

## Implementacion
Dos maneras de implementar:
``` cpp
#include <iostream> 
int main(){ 
	int x = 3; int &y = x; 
	
	std::cout << "x = " << x << ", y = " << y << std::endl; 
	// Imprime “x=3, y=3” 
	x = 4; 
	std::cout << "x = " << x << ", y = " << y << std::endl; 
	// Imprime “x=4, y=4” 
	y = 5; std::cout << "x = " << x << ", y = " << y << std::endl; 
	// Imprime “x=5, y=5” 
	return 0; 
}
```
### utilzando [[Notes/Calificadores const y static#*const*\|const]]
```cpp
#include <iostream> 

int main(){ 
	int x = 3; 
	const int &y = x; // Referencia constante 
	
	std::cout << "x = " << x << ", y = " << y << std::endl; 
	// Imprime x = 3 e y = 3 
	x = 4; 
	std::cout << "x = " << x << ", y = " << y << std::endl;
	// Imprime x = 4 e y = 4 
	y = 5; // Error de compilación 
	std::cout << "x = " << x << ", y = " << y << std::endl;
	
	return 0;
} 
```

# Parametros a Funciones
*(cont'd [[Notes/Funciones y Archivos\|Funciones y Archivos]])*

Los pasajes de parametros pueden ser:
1. **Por valor**: copia el valor del parametro y se lo pasa a la funcion
	Cuando:
	- no son necesarias las modificaciones en las variables
	- son pocas las variables a pasar
```cpp
int square(int value) {return value*value;}; 
squareValue = square(value);
```
2. **Por referencia**: se pasa la variable a la funcion declarando un alias
	Cuando:
	- se necesitan retornar multiples valores
	- la performance es importnate (no tanta memoria en stack)
	- es necesaria una modificacion en los parametros a pasar
	Para acceder a una clase/struct: `.`
``` cpp
void square(int& val) { val *= val; }; 
square(value);
```
3. **Por puntero**: se pasa a la funcion la direccion de la variable
	Cuando:
	- se necesitan retornar multiples valores
	- la performance es importnate (no tanta memoria en stack)
	- es necesaria una modificacion en los parametros a pasar
	Para acceder a una clase/struct: `->`
```cpp
void square(int* val) { *val *= *val; }; 
square(&value);
```

## Pasaje de Smart Pointers
### unique_ptr
#### Pasaje:
##### por valor
Se transfiere el ownership a la funcion y queda null el puntero
```cpp
#include <iostream> 
#include <memory> 

void myFunc(std::unique_ptr<int> ptr) { 
	std::cout << "Valor en la función: " << *ptr << std::endl; 
	// `ptr` se va fuera de scope y se libera 
}

int main() { 
	std::unique_ptr<int> ptr = std::make_unique<int>(5); 
	// std::move transfiere el ownership a la función 
	myFunc(std::move(ptr)); 
	// Luego de myFunc, el puntero es null 
	if (!ptr) 
		std::cout << "Ahora ptr es null." << std::endl; 
	return 0; 
}
```
##### por referencia
Nos ahorramos transferir el ownership y no perdemos el puntero
``` cpp
#include <iostream> 
#include <memory> 

void myFunc(std::unique_ptr<int>& ptr) { 
	*ptr = 42; // Modifica el dato al cual ptr apunta 
	std::cout << "Dentro de myFunc: " << *ptr << std::endl; 
} 

int main() { 
	std::unique_ptr<int> ptr = std::make_unique<int>(5); 
	std::cout << "Antes de myFunc : " << *ptr << std::endl; 
	// Pasa por referencia para evitar transferir ownership 
	myFunc(ptr); 
	std::cout << "Fuera de myFunc: " << *ptr << std::endl; 
	return 0; 
}
```
#### Retorno:
Como solo puede existir UNA instancia de un unique_ptr:
	Al retornar debemos transferir la ownership del puntero con `std::move`
##### Transferencia Implicita:

``` cpp
std::unique_ptr<int[]> allocateArray(int size) {
	std::unique_ptr<int[]> arr = std::make_unique<int[]>(size); 
	for (int i = 0; i < size; ++i) 
		arr[i] = i + 1; // Inicializa el array con valores de 1 a size 
	return arr; // Retorna el unique_ptr 
}
```
Como no nos pasan un unique_ptr sino un int, se permite devolver el puntero directamente, debido a que es una variable local de la función.
El compilador se encarga de hacer el cambio de ownership de arr implicitamente
	*Nótese que si se retorna std::move(arr) también funcionará*
##### Transferencia Necesaria:

```cpp
std::unique_ptr<int[]> modifyArray(std::unique_ptr<int[]> &arr, int size) { 
	for (int i = 0; i < size; ++i) 
		arr[i] *= 2; // Procesa los datos del array 
	return std::move(arr); // Transfiere ownership 
}
```
Como nos pasan una referencia al unique_ptr, si es necesario usar std::move para transferir el ownership del puntero
	*Notese que si sólo se retorna arr sin std::move, se estaría intentando copiar el unique_ptr y generaria un error en tiempo de compilacion*
### shared_ptr
#### Pasaje
##### Por valor
```cpp
#include <iostream> 
#include <memory> 
// Por valor, ambos ptr apuntan a la misma dirección 
void myFunc(std::shared_ptr<int> ptr) { 
	*ptr = 6; // Cambiar el valor se vera reflejado en main() 
	std::cout << "Count en myFunc: " << ptr.use_count() << std::endl; 
	// Por referencia el count se incrementa a 2. 
} 

int main() { 
	std::shared_ptr<int> ptr = std::make_shared<int>(5);
	myFunc(ptr); // Pasar por valor incrementa el count, se hace una copia. 
	std::cout << "Count en ptr: " << ptr.use_count() << std::endl; 
	// Imprime 1, el otro se libero, 
	std::cout << "Valor luego de usar la funcion: " << *ptr << std::endl; 
	// Imprime 6 
	return 0; 
} 
```
##### Por Referencia
``` cpp
#include <iostream> 
#include <memory> 

void myFunc(const std::shared_ptr<int>& ptr) { 
	*ptr = 6; // Como es un alias, el valor también cambia en main() 
	std::cout << "Count en myFunc: " << ptr.use_count() << std::endl; 
	// El count de la referencia no cambia, imprime 1. 
} 

int main() { 
	std::shared_ptr<int> ptr = std::make_shared<int>(5); 
	myFunc(ptr); // Pasando por refecencia no modifica el use_count al retornar
	std::cout << "Count en ptr: " << ptr.use_count() << std::endl; 
	// Imprime 1 
	std::cout << "Valor luego de usar la funcion: " << *ptr << std::endl; 
	// Imprime 6 
	return 0; 
}
```

# Punteros a Funciones
Se declaran de la siguiente forma:
```cpp
tipoARetornar (*nombrePuntero) (tipoArg1, tipoArg2, …, tipoArgN)
```
## Declaracion
```cpp
#include <iostream> 
int foo(){}; int goo(){}; double hoo(int){}; 

int main(){ 
	int (*fncPtr1)(){&foo}; // puntero a foo, retorna int 
	int (*fncPtr2)() = &foo; // puntero a foo, retorna int 
	fncPtr1 = &goo; // cambia el puntero a goo 
	
	double (*fncPtr3)(int) = &hoo; // puntero a hoo (con parámetro) 
	fncPtr3 = &foo; // Error por tipos. 
	
	float (*fncPtr4)(){nullptr}; // puntero a null pointer 
	float (*fncPtr5)() = NULL; // puntero a null pointer 
	
	return 0; 
}
```
## Llamada
```cpp
#include <iostream> 
int square(int x){ return x*x;} 

int main(){ 
	// Inicializar puntero a función 
	int (*fncPtr)(int) = &square; 
	// Llamar la función con fncPtr: 
	(*fncPtr)(5); // Desreferenciación explícita 
	fncPtr(5); // Desreferenciación implícita 
	
	// Imprime las direcciones de las funciones (son las mismas) 
	std::cout << reinterpret_cast<void*>(fncPtr) << std::endl; 
	std::cout << reinterpret_cast<void*>(square) << std::endl; 
	// recordar: square es una dirección, la función es square() 
	return 0; 
}
```