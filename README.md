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
