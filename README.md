# Laboratorio 9 - Administración de Memoria

Consultar el apunte de práctica sobre administración de memoria en _xv6_ y el Capítulo 2 "Page Tables" del libro de _xv6_. Las respuestas a los ejercicios deben realizarse en un documento en formato PDF.

## Ejercicio 1: Contador de páginas
Agregar a _xv6_ una llamada al sistema `pgcnt()`, que retorne la cantidad de páginas virtuales que ocupa actualmente el proceso que invoca la llamada al sistema. El código de la llamada al sistema lo pueden encontrar en el archivo `sys_pgcnt.c` (copiar la función en el archivo `sysproc.c`). Deben modificar el resto de los archivos requeridos para agregar la llamada al sistema.

El archivo `pgcnt.c` es un programa de usuario que invoca la llamada al sistema `pgcnt()`. Deben agregarlo a la lista `UPROGS` en el `Makefile` de _xv6_.

Este programa invoca la llamada al sistema `pgcnt()` tres veces. La primera al inicio del programa y luego de invocar a `sbrk` y `malloc`:
```
$ pgcnt
2
3
11
$
```

### Responder:
* Explicar el resultado del programa, justificando el número de páginas que cada invocación a `pgcnt()` retorna. ¿Por qué la diferencia entre las invocaciones de `sbrk` y `malloc`?
* Explicar línea por línea, cómo funciona la función `sys_pgcnt()`.

## Ejercicio 2: Paginación bajo demanda
Muchos programas reservan memoria que pueden no utilizar nunca. Por ejemplo, un arreglo de gran tamaño del que no se hace uso. El sistema operativo puede posponer la asignación efectiva de memoria a estas secciones hasta que sean requeridas para lectura y/o escritura. Cuando un proceso quiere acceder a una de estas secciones de memoria aún no asiganadas, ocurre un _page fault_ (fallo de página) y el sistema operativo debe entonces cargar las páginas requeridas desde disco o bien reservar la memoria solicitada. En este ejercicio vamos a implementar un esquema de paginación bajo demanda rudimentario en _xv6_.

### Parte 1
Para incrementar el tamaño del segmento de datos se utiliza la llamada al sistema `sbrk()`, implementada en la función `sys_sbrk()` en el archivo `sysproc.c`. 

Comentar la llamada a `growproc()`, que es la función que invoca a `sys_sbrk()`. Hecha esta modificación, compilar y ejecutar _xv6_ y ejecutar el comando `echo hola`.
- ¿Qué error aparece? ¿A qué tipo de error se refiere el número indicado por `trap`?

### Parte 2
Modificar el código en el archivo `trap.c`, para que ante un _page fault_ producido por un programa a nivel de usuario, realice la carga de la página correspondiente a la dirección de memoria virtual que causó el _page fault_. No se necesitan cubrir todos los casos posibles de error, debe bastar para ejecutar comando simples como `echo` o `ls`.

El código a agregar en `trap.c` lo pueden encontrar en el archivo `pgflt.txt`. Como se invoca a `mappages()`, hay que eliminar la sentencia `static` de su declaración en el archivo `vm.c`, y agregar su prototipo en `trap.c`. Además, en la función `sys_sbrk()`, se debe actualizar el tamaño del proceso (`myproc()->sz`), en función del argumento `n`.

### Responder:
* ¿Donde en particular, se debe agregar el código en `trap.c`?
* Probar el correcto funcionamiento de las modificaciones.
* Explicar, línea por línea, el código del archivo `pgflt.txt`, agregado a `trap.c`.

---

¡Fín del Laboratorio!
