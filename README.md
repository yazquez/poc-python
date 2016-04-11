---
Aproximacion funcional desde Python.
---

Funciones map, filter, and reduce
===

Estas funciones proporcionan un acercamiento funcional al lenguaje
Python. Otras funcionalidades a tener en cuenta en este acercamiento son
las listas de comprensión y las expresiones lambda, dado que se suelen
usar en conjunto con, o en lugar de, las funciones mencionadas.

map
===

Sintaxis:

> map(function, sequence)

Por poner el “problema” en contexto, digamos que tenemos una lista de
números y queremos obtener una lista con los cuadrados de estos números.
Una aproximación imperativa podría ser la siguiente:
```python
numbers = [2, 3, 4, 5,6\]
squared = []
for n in numbers:
squared.append(n**2)
print(squared)
```
> \# \[4, 9, 16, 25, 36\]

Es decir, en todo momento controlamos nosotros el flujo de las
operaciones, diciendo exactamente como queremos que se realicen las
operaciones.

Usando map el código nos quedaría de la siguiente forma:
```python
numbers = \[1, 2, 3, 4, 5\]
def sqr(x): return x \*\* 2
squared = list(map(sqr, numbers))
```
(Nótese que hacemos un cast al tipo list dado que la función map
devuelve un iterator)

El patrón siempre es el mismo, tenemos una lista de elementos (una
secuencia más exactamente) y una función que queremos que se aplique a
todos los elementos de la lista.

En realidad, si no vamos a usar esa función para más cosas, podemos
crear una función anónima usando una expresión lambda que sustituya al
código de la función. La sintaxis de las expresiones lambda es muy
simple:

> lambda argument\_list: expression 

Así pues, nuestro código quedaría:
```python
squared = list(map(lambda x: x\*\*2, numbers))
```
También podemos obtener este resultado usando listas por compresión
```python
[x**2 for x in numbers]
```
Si la función que vamos a mapear recibe más de un parámetro tendremos
que suministrar a map tandas secuencias como parámetros reciba dicha
función, por ejemplo:
```python
def sum(a, b): return a+b
list(map(sum,\[1,2,3\],\[2,4,8\]))
```
Usando lambda:
```python
list(map(lambda a, b: a+b, \[1,2,3\], \[2,4,8\]))
```

Usando compresión, en este caso tendremos que apoyarnos la función zip
que “empaqueta” dos o más secuencias:
```python
[a+b for a, b in zip([1,2,3], [2,4,8])]
```
filter
======

Sintaxis:

>filter(eval_function, sequence)


La función filter actúa como lo haría una condición where en sql,
devuelve cada elemento de la secuencia para el cual la función de
evaluación devuelve True.

Por ejemplo, el siguiente código filtrará la lista pasada, del 1 al 10,
y nos devolverá los números pares
```python
list( filter((lambda x: x%2 == 0), range(1,10)))
```

De Nuevo esto mismo lo podríamos haber hecho con compresión
```python
[x for x in range(1,10) if x%2 == 0]
```
reduce
======
Sintaxis:
> reduce(reduct_function, sequence)

La función reduce es un poco más compleja que las anteriores, toma como
entrada una secuencia y una función que opera sobre dicha secuencia
(sobre todos ellos), cuando acaba de operar sobre todos los elementos de
la secuencia devuelve el resultado final.

Para usar esta función debemos importarla desde functools
```python
from functools import reduce
```
Veamos un par de ejemplos

1.  Sumar los elementos de una lista
    ```python
    reduce((lambda x, y: x + y), \[1, 2, 3, 4, 5\])
    ```
    > #15
    
    ```python
    reduce((lambda x, y: x + y), \['a', 'e', 'i', 'o', 'u'\])
    ```
    > #aeiou
    
    En este caso, suma de elementos, podemos optar por usar directamente uno
    de los operadores incluidos por defecto.
    ```python
    functools.reduce(operator.add, \[1, 2, 3, 4, 5\])
    ```
    > \#15

2.  Hallar el máximo o el mínimo de un conjunto de valores

    Máximo
    ```python
    reduce(lambda a,b: a if a&gt;b else b, \[1,4,6,2,9,0\])
    ```
    Mínimo
    ```python
    reduce(lambda a,b: a if a&lt;b else b, \[1,4,6,2,9,0\])
    ```

Conclusión
==========

El código para el que se aplica las funciones map y filter puede ser
fácilmente sustituido por listas por compresión, en algunos casos
incluso una mejor legibilidad y mejor rendimiento.

De hecho, en su momento se barajó la posibilidad de quitar dichas
funciones del estándar de Python 3.0, finalmente se mantuvieron
cambiando el tipo de retorno a map.

<http://www.artima.com/forums/flat.jsp?forum=106&thread=98196>

Pero como casi siempre hay otros puntos de vista que, también, tiene
mucho sentido. Como muestra de esto:

<http://xahlee.info/perl-python/python_3000.html>

Punto aparte merecen las funcionalidades que ofrece spark y que será
abordado en un próximo post.
