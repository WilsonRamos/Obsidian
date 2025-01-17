
# f - string o string formateada
* Available starting with python 3.6
* Character f followed by a **==formated string literal==**
	* anything that can be appear in a normal string literal 
	* Expresions bracketed by curly braces {}
* Expressions in curly braces evaluated at runtime, automatically converted to string, and concatenated to the string preceding them

```python
num = 3000
fraction = 1/3
print(num*fraction, 'es', fraction*100, '% de', num) # ,introduces an extra space
print(num*fraction, 'es', str(fraction*100) + '% de', num)
print(f'{num*fraction} es {fraction*100}% de {num}')

# EXpresiones lo evalua antes de imprimir en pantalla
```

En el metodo de Newton-Raphson, decimos que una estimacion(suposicion) converge cuando se aproxima al valor real de la raiz dentro una tolerancia aceptable

## converge : 
-> Es el proceso de acercarce progresivamente a un valor o estado final estable 
- **Algoritmos numéricos**:
    
    - En programación y algoritmos, un proceso iterativo **converge** si las soluciones o valores generados por el algoritmo se acercan cada vez más a la solución deseada o a un resultado estable dentro de una tolerancia aceptable.
- **Redes y telecomunicaciones**:
    
    - Se dice que un sistema **converge** cuando los datos o los nodos en una red alcanzan un estado estable después de un cambio, como la actualización de tablas de enrutamiento.
## mutuamente excluyentes :
cuando dos o mas eventos, condiciones o situaciones son mutuamente excluyentes, significa que no pueden ocurrir o se verdaderas al mismo tiempo. una cosa excluye a la otra

# ITERATION

## WHILE LOOPS
* Loop as long as a condition is true*
* Need to make  sure you don`t enter an infinitive loop*
```python
while <condition>:
    <code>
    <code>
    ...
```


- `<condition>` **<span style="color:red;">evaluates to a Boolean</span>**
- If `<condition>` is `True`, **<span style="color:red;">execute all the steps inside</span>** the `while` code block.
- **Check** `<condition>` again.
- **<span style="color:red;">Repeat</span>** until `<condition>` is `False`.
- If `<condition>` is never `False`, **<span style="color:red;">then will loop forever!!</span>**

## FOR LOOPS
* can loop over ranges of numbers
* can loop over elements  of a string
### Range

- **Generates a sequence of integers**, following a pattern.
- `range(start, stop, step)`
  - `start`: **First integer** generated.
  - `stop`: **Controls the last integer** generated (up to, but not including this integer).
  - `step`: **Used to generate the next integer** in the sequence.

- A lot like what we saw for **slicing**.

- Often omit `start` and `step`:
  - **Examples**:
    ```python
    # start defaults to 0, step defaults to 1
    for i in range(4):
        print(i)
    ```

    ```python
    # step defaults to 1
    for i in range(3, 5):
        print(i)
    ```
    ```python
    for me in range(4,0,-1):
	    print(me*"$")
```

# GUESS-and-CHECK
## SQUARE ROOT with while loop

```python
guess = 0

x = int(input("Enter an integer: "))

while guess**2 < x:
    guess = guess + 1

if guess**2 == x:
    print("Square root of", x, "is", guess)
else:
    print(x, "is not a perfect square")

```

how do i do know that this thing happened o someting? ->Boolean Flag

BIG IDEA : Booleans can be used as signal that something happened.
we calll them Boolean flag

BIG IDEA : Operations on some floats introduces a very small error.
The small error can have a big effect if operation are done many times

human acctually work in base 10
computer work in base 2

# Floats ans Aproximation Method