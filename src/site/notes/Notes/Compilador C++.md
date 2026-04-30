---
{"dg-publish":true,"permalink":"/notes/compilador-c/","dg-note-properties":{"subject":["[[Paradigmas de la Programación]]"],"clase":["[[Clase 1 - Introduccion a C++]]"],"type":["Concept"],"topics":null,"tags":null}}
---


# Compiler y Linker
#resumir 
- El código en C++ debe ser transformado en un archivo ejecutable.
- Los archivos de C++ (.cpp, .h, etc) entran al preprocesador que genera un archivo temporal según los comandos de preprocesador (#define, # include, etc).
- Los archivos temporales van al compilador, produciendo un archivo binario .obj/.o (Win, Linux o Mac) con los comandos en assembler y un diccionario de las funciones y variables definidas.
- El LINKER combina archivos .obj/.o, librerías y el código necesario para crear el ejecutable.
- Errores pueden encontrarse tanto en el compilador (Ccode) como en el linker (LNKcode).
- El producto final no tiene que ser exclusivamente un archivo ejecutable. 
- Existen librerías dinámicas y estáticas, utilizadas para distribuir funcionalidad entre proyectos. 
- Se verá en su momento que las librerías estáticas (obtenidas por static linking) y dinámicas (dynamic linking) tienen sus ventajas y desventajas. 
- Las librerías también son generadas por el sistema de compilación y linkeo.

![Pasted image 20260405152105.png](/img/user/Attachments/Pasted%20image%2020260405152105.png)
## Preprocessing
#resumir 
- No sabe qué es C++ ni sabe sobre tipos de datos
- Básicamente se encarga de 2 tareas:
	- Expansión de Macros (#define): Reemplaza texto por texto antes de que el compilador vea el código
	- Resolución de Inclusiones (#include): Copia y pega el contenido completo del archivo .h dentro del .cpp
![Pasted image 20260405152824.png](/img/user/Attachments/Pasted%20image%2020260405152824.png)
## Compiler
#resumir 
- El input es el output del Preprocessing.
- Algunas de las operaciones que realiza son:
	- Análisis sintáctico: Convierte el texto en un AST. 
	- Análisis semántico: Se verifican las reglas de tipos, entre otros. 
	- Código Objeto (.o/.obj): El resultado no es un ejecutable aún. Es código de máquina (instrucciones de CPU) con "huecos". 
	- Tabla de Símbolos: cada archivo .o incluye un "diccionario": "Aquí implemento la función sumar, pero necesito usar la función printf y no sé dónde está".
![Pasted image 20260405153017.png](/img/user/Attachments/Pasted%20image%2020260405153017.png)
## Linker
#resumir 
- El input es el output del Compiler. 
- Algunas de las operaciones que realiza son: 
	- Resolución de Símbolos: Tapa los “huecos”: si el archivo A.o llama a printf, busca la implementación binaria de printf en la librería estándar de C y los conecta. 
	- Binario Final: Empaqueta todo siguiendo un formato específico del sistema operativo, generando un ejecutable independiente (.exe, .out) o una librería dinámica (.dll, .so) 
- Recordar: el ejecutable final pierde los nombres de las variables y las referencias a las líneas del código fuente original (default).
![Pasted image 20260405153112.png](/img/user/Attachments/Pasted%20image%2020260405153112.png)
# Comandos G++ 
## Opciones/flags:
#resumir 
- -c: solo compilar, devuelve los archivos de objetos (.obj/.o). o 
- -o: especifica el archivo de salida (ejecutable o objeto, default “a.out”) 
- -Wall: muestra casi todos los mensajes de warnings. 
- -Wextra: muestra todos los mensajes de warnings. 
- -std=standard: especifica la norma de C++ (-std=c++11, -std=c++17, etc) 
- -g: habilita información de debugging (para usar con debuggers como gdb) 
- -I folder: (i mayus) agrega directorios para incluir header files búsquedas (preprocessor) 
- -Lfolder: agregar directorios para incluir librerías (linker) 
- -lfile: se indica la librería (estática o dinámica) a linkear en el folder indicado por -L 
- -lstdc++: linkea el proyecto (usualmente no necesario dado que se hace automáticamente)
## Ejemplo

```
g++ -g *.cpp –I ./include –o my_Project
```

# GNU Debugger o gdb
#completar
# Comandos GitHub
#completar 
# Comandos Linux
#completar 