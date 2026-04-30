---
{"dg-publish":true,"permalink":"/notes/funciones-y-clases-friend/","dg-note-properties":{"subject":["[[Paradigmas de la Programación]]"],"clase":["[[Clase 5 - Sobrecarga de Operadores]]"],"type":["Concept"],"topics":["[[OOP]]"],"tags":null}}
---

## Funciones *friend*
- no forman parte de un objeto (no son un método)
- pueden tener acceso a miembros *public, private y protected* de la clase
- no tiene acceso a *this*

- declaracion de funcion dentro de la clase, 
- definicion fuera de la clase
- denominadas **funciones no miembro** 

La **amistad** no es mutua:
	*si un objeto tiene una función friend, la función puede acceder los miembros public, private y protected, pero la clase no puede acceder a ninguna variable de la función*
### Ejemplo
``` cpp
class Plane { 
private: 
	double oilStatus; 
	double waterStatus; 
public: 
	Plane(double oil, double water): oilStatus(oil), waterStatus(water){} 
	
	void setOilStatus(double oil){oilStatus = oil;}; 
	
	void setWaterStatus(double water){waterStatus = water;}; 
	
	friend void showPlaneData(Plane pl); 
};
```
 *La función tiene acceso a los atributos private oilStatus y private waterStatus*
```cpp
void showPlaneData(Plane pl){ 
	std::cout << "The oil status is: " << pl.oilStatus 
			  << ", the water status is: " << pl.waterStatus 
			  << std::endl; 
} 

int main(){ 
	Plane myPlane(75, 100); 
	showPlaneData(myPlane); 
	// Imprime: Plane oil is: 75, Plane water is: 100 
	
	return 0;
}
```
## Clases *friend*
Clase cuyos métodos pueden acceder a los miembros *public, private y protected* de otra clase que la declara como **friend**.
- La declaracion se realiza a nivel de clase
- La "amistad" no es mutua
- Se puede declarar friend en ambas direcciones
### Ejemplo
``` cpp
class Car; 

class Engine { 
private: 
	double horsepower; 
	bool isOn; 
	void displayEngine() { 
		std::cout << "Power: " << horsepower 
			<< ", Engine on? " << ( isOn ? "Yes" : "No" ) 
			<< std::endl;
} 
public: 
	Engine(double horsepower, bool isOn) : 
		horsepower(horsepower),isOn(isOn) {} 
	
	// Se hace amigo de la classe Engine 
	friend class Car; 
};
```

``` cpp
class Car { 
private: 
	std::string maker; double length; 
	
public: 
	Car(std::string maker, double length) : maker(maker), length(length) {} 

	void displayInfo(Engine eg){ 
		std::cout << "Car maker: " << maker << ", Length: " 
		<< length << std::endl << "Engine data: ";
		 eg.displayEngine(); // Método privado de la clase Engine 
		 std::cout << "Engine is: " << (eg.isOn ? "Yes" : "No") 
		 << std::endl; } 
};
```

``` cpp
int main() { 
	Car car("Toyota", 4.5); 
	Engine eng(300, true); // Un motor de 300 hp activado 
	car.displayInfo(eng); 
	
	return 0; 
} 
```
## [[Notes/Sobrecarga de Operadores#Operadores con *friend*\|Sobrecarga con Operadores con friend]]
