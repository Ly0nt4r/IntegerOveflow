# IntegerOveflow
Explicación del desbordamiento de enteros.

## Explicación

A grandes rasgos, y como introducción, podemos definir el desbordamiento de enteros como una vulnerabilidad provocada por un almacenamiento de un entero mayor a lo establecido en memoria.

Ahora que tenemos una pequeña definición, vamos a explicarlo más en profundidad.

## Estados de un dígito - Complemento a 2

En la arquitectura x86 una variable tipo int permite almacenar valores de 32 bits. Internamente se almacenan en un formato llamado complemento a 2. En esta forma de representación, si el bit más significativo (el de la izquierda) está a 1, el valor se considera negativo, así que sólo tenemos que desbordar el valor de la variable int y poner este bit a 1 para conseguir un valor negativo.

![complemento-a-2](https://user-images.githubusercontent.com/87484792/192157249-370fe2bf-5131-4569-9fc9-0df0d9403805.png)

*En la imagen podemos observar lo mencionado anteriormente, tratando un entero de 4 bits*


Así pues, el mayor valor positivo de 32 bits que puede almacenar una variable entera en complemento a 2 es 0x7fffffff, que en binario es 01111111111111111111111111111111 y en decimal 2147483647.


## Vulnerabilidad

La vulnerabilidad reside en el aumento de este número entero maximo, provocando una des-secuenciación de su tamaño completo.
En otras palabras...¿Que pasa si intentamos sumarle a este entero maximo un entero positivo más?  Ej: *2147483647+1*

Si ahora sumamos 1 a este valor, obtendremos 0x80000000 que en binario es 10000000000000000000000000000000. Como tiene el bit más significativo a 1, se interpreta como un valor negativo.

Aqui es donde entra el problema del desbordamiento. 

Imaginemos que intentamos comprar un producto muy caro en una pagina web (un rolex de diamantes por ejemplo 😧), y el código no está sanetizado para evitar desbordamientos.

![image](https://user-images.githubusercontent.com/87484792/192159407-e7422ff4-77c8-4cae-8e53-05a9862238af.png)

*El código es un poco caca, lo sé, no lo tengais en cuenta :3*

## Explotación

Actualmente tenemos una página web supuestamente vulnerable. No hay comprobación de ningún tipo y nuestro objetivo es llevarnos ese rolex gratis.
La idea es sobrepasar la capacidad máxima de un entero (**2147483647**)

Usaré ese donativo intencionalmente puesto para la ocasión tal como en una pelicula de ficción donde el protagonista esta a punto de morir, pero no.

El reloj tiene un valor de 13.578€, lo que nos daría un margen de 2.147.470.069€. Añadiremos esa cantidad (+1) como donativo para ver que sucede.  (Menudo donativo más jugoso)

![image](https://user-images.githubusercontent.com/87484792/192159598-1582a72a-dd84-4a1c-bb1a-d09461281730.png)

¡Vaya! hemos conseguido desbordar el entero, el problema es que nos sale un número negativo (Nos deberian de pagar eso, cosa que no haran 😲)
La solución sería intentar entrar en la casuistica donde el valor es 0, dando así una "Compra Gratis". 

¿Como hacemos esto? analizemos un segundo lo que tenemos.
Por el momento el desbordamiento se ha producido, por lo que es vulnerable. 

Al sumar el valor en su mismo valor, obtendremos: "4294967294", que en binario es 11111111111111111111111111111110 (0xFFFFFFFE). Si a este valor le sumamos 1 obtendremos 11111111111111111111111111111111 (0xFFFFFFFF) y si le volvemos a sumar 1 tendremos... ¡¿0?!

En realidad tenemos 100000000000000000000000000000000, que es un valor de 33 bits, pero como sólo podemos almacenar 32 en la variable, el bit más significativo (el 1) se descarta y nos queda 00000000000000000000000000000000. 


