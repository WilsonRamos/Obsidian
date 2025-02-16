![[Pasted image 20250207085957.png]]

```python
L=[1,2,3,4]
listaAlias = L #Alias ambas apuntas al mismo objeto en memoria
L_copy = L[:]  # También puedes usar L.copy()
```


![[Pasted image 20250207100930.png]]

![[Pasted image 20250207103303.png]]![[Pasted image 20250207103659.png]]

BIG IDEA : When you pass a list as  a parameter to a funtion, you are making an alias.
-> The actual parameter(from the function call)) is an alias for the formal parameter(from the function definition).