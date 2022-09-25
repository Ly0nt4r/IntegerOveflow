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

![image](https://user-images.githubusercontent.com/87484792/192158589-150cab70-6191-4b13-bc66-bb200b02a151.png)

*El código es un poco caca, lo sé, no lo tengais en cuenta :3*

## Explotación

Actualmente tenemos una página web supuestamente vulnerable. No hay comprobación de ningún tipo y nuestro objetivo es llevarnos ese rolex gratis.
La idea es sobrepasar la capacidad máxima de un entero (**2147483647**)


