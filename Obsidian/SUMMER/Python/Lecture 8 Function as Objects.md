
Funciones:
* Se usan para escribir codigo limpio,legible facil de depurar 
* **Sintaxsis : **
	* return : Devuelve ul valor al llamador
	* **docstring : ** :
		* Comentarios en formato de triple comillas que explican qué hace la función, los parámetros y el valor retornado. Se consideran un contrato entre el escritor y el usuario de la función.

## return vs print
- `return`: Devuelve un valor y termina la ejecución de la función.
- `print`: Muestra algo en la consola, pero no afecta al flujo del programa.

## Funciones como objetos : 
- En Python, las funciones son objetos y pueden:
    - Asignarse a variables.
    - Pasarse como parámetros a otras funciones.
    - Retornarse desde funciones.

UnBoundLocalError:
```python
def f(y):
	x=x+1
x=4
f(x)
print)x
```

### BIG IDEAS:
-> Everything in Python is an Object

![[Pasted image 20250123101431.png]]
So the return only has a meaning inside a function