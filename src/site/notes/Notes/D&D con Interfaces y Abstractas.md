---
{"dg-publish":true,"permalink":"/notes/d-and-d-con-interfaces-y-abstractas/","tags":["ejemplo"],"dg-note-properties":{"subject":["[[Paradigmas de la Programación]]"],"clase":["[[Clase 7 - Polimorfismo]]"],"type":["Concept"],"topics":["[[OOP]]"],"tags":["ejemplo"]}}
---

# header
``` cpp
#pragma once 

class Enemy { 
public: 
	Enemy(int hp) : hp(hp){}; // Obliga iniciacion con hp
	virtual int getHP() = 0; // Todos tienen HP, 
	virtual bool isDead() = 0; // pueden morir 
	virtual int attack() = 0; // y atacar 
	virtual ~Enemy(); 
protected: 
	int hp; // Puntos de vida 
	bool dead = false; // Por default esta vivo 
};
```
Enemy es una [[Notes/Interfaces y Metodos Virtuales#Clases Abstractas\|Clase Abstracta]] pq tiene atributos
``` cpp
// "Override" se usa al reemplazar un método virtual de la clase base. 
class WalkingEnemy : public Enemy { 
public: 
	WalkingEnemy(int hp) : Enemy(hp){}; 
	int getHP() override; // Devuelve los puntos de vida 
	bool isDead() override; // Esta muerto el enemigo? 
	int attack() override; // Ataca restando hp 
	virtual int boneAttack() = 0; // Ataque especial 
	void walk(); // Se desplaza caminando 
	virtual ~WalkingEnemy(); 
}; 

class Skeleton : public WalkingEnemy { 
public: 
	Skeleton(int hp) : WalkingEnemy(hp){}; 
	int attack() override; // Hace el ataque del esqueleto 
	int boneAttack() override; // Hace el ataque especial 
	~Skeleton(); 
};
```
# source
``` cpp
#include <iostream> 
#include "enemies.h" 
// No se implementa nada de la clase base Enemy 
int WalkingEnemy::getHP() { 
	return hp; 
} // Getter de HP 

bool WalkingEnemy::isDead() { 
	return hp <= 0; 
} // Si no tiene mas HP esta muerto 

int WalkingEnemy::attack() { 
	return 0; 
} // Por default no remueve HPs 

void WalkingEnemy::walk() { 
	std::cout << "Is walking!!" << std::endl; 
} // El enemigo camina, no vuela 

int Skeleton::attack() { 
	return -5; 
} //El esqueleto ataca con -5 por default 

int Skeleton::boneAttack() { 
	return -10; // Bone attack hace -10 
} 

Skeleton::~Skeleton(){ 
	std:: cout << "Destructing Skeleton\n"; 
} 

WalkingEnemy::~WalkingEnemy() { 
	std::cout << "Destructing WalkingEnemy\n"; 
} 
	
Enemy::~Enemy() { 
	std::cout << "Destructing Enemy\n"; 
}
```

# main
``` cpp
#include <iostream> 
#include "enemies.h" 

int main(){ 
	Skeleton sk1(20); // El esqueleto tiene 20 HP 
	// Verificando puntos de vida 
	std::cout << "El esqueleto tiene " << sk1.getHP() 
			  << " puntos de vida." << std::endl; 
			  
	// Verificando si esta vivo 
	if (sk1.isDead()) 
		std::cout << "El esqueleto esta muerto" << std::endl; 
	else // En realidad siempre esta muerto, es un esqueleto! 
		std::cout << "El esqueleto esta vivo" << std::endl; 
		
	std::cout << "Prueba terminada" << std::endl << std::endl; 
	return 0; 
}
```

```
El esqueleto tiene 20 puntos de vida. 
El esqueleto esta vivo 
Prueba terminada 
Destructing Skeleton 
Destructing WalkingEnemy 
Destructing Enemy
```
*Siempre conviene verificar en una prueba que se estén destruyendo correctamente las clases*