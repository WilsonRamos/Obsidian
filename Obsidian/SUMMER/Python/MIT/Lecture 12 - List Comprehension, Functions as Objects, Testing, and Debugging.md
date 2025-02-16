# List Comprehension
```python
List = [Expression for elem in iterable if test]

#Codigo Tradicional
def square_elements(L)
	newList = []
	for i in L:
		newList.append(i**2)

	return newList
#List Comprehension equivalente

List1 = [e**2 for e in L]
List2 = [e**2 for e in L if e%2 == 0] # Solo elemetos pares
```

# Funciones y parametros por defecto
```python
def bisection_root(x,epsilon=0.01):
	#codigo para aproximar la raiz cuadrada de x
	return aproximation
```

## uso:
Los Parametros por defecto deben ir al final de parametros

```python
bisection_root(123)                 #usa epsilon = 0.01 por defecto
bisection_root(123,epsilon = 0.5)   #usa epsilon e = 0.5 
bisection_root(epsilon = 0.5, x= 67)#Se Puede pasar argumentos por nombre
```

Function as Object
![[Pasted image 20250215190939.png]]

# Testing and Debugging
![[Pasted image 20250215195714.png]]

