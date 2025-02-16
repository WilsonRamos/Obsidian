 ❌ **no se puede reutilizar fuera de ese contexto porque no tiene nombre.**

![[Pasted image 20250129155718.png|6700]]

# TUPLES
* Indexable ordered sequence of object
	* Object can be any type - int. string , tuple , tuple of tuple, ...
* Cannot change element values, inmutable

te = () -> Empty tuple

![[Pasted image 20250129174020.png|750]]

```python
def quotient_and_remainder(x,y):
q=x//y
r=x%y
return (q,r)

(quot,rem) = quotient_and_remainder(5,2)
print(f"quotient es : {quot}")
print(f"remainder is : {rem}")
```


BIG IDEA : Returning one object(a tuple) allows you to return multiple values (tuple elements) 

# LIST
* Indexable ordered sequence of object
	* usually homogeneous (i.e , all integer, all string, all list)
	* But can contain mixed types(NOT COMMON)
* MUTABLE,  this mean you can change values of specific elements of list
![[Pasted image 20250129183003.png]]