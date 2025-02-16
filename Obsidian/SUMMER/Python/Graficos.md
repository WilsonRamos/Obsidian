
## **1. Gráfico de Línea (`plt.plot()`)**

### **📌 Definición**

El gráfico de línea se utiliza para mostrar tendencias o relaciones entre variables continuas. Se representa mediante una línea que conecta puntos en el plano cartesiano.

### **📌 Uso Principal**

- Seguimiento de tendencias en el tiempo.
- Representación de funciones matemáticas.
- Comparación de múltiples conjuntos de datos.

### **📌 Parámetros Clave**

- `x, y`: Datos a graficar en los ejes X e Y.
- `color`: Color de la línea (`'red'`, `'blue'`, `'#00FF00'`).
- `linestyle`: Tipo de línea (`'-'`, `'--'`, `':'`).
- `linewidth`: Grosor de la línea.
- `marker`: Tipo de marcador en los puntos (`'o'`, `'*'`, `'s'`).

### **📌 Ejemplo**


```python

import matplotlib.pyplot as plt 
import numpy as np  
x = np.linspace(0, 10, 100) 
y = np.sin(x)  
plt.plot(x, y, color='blue', linestyle='--', linewidth=2, marker='o', markersize=5, label='Seno(x)') 
plt.xlabel("Eje X") 
plt.ylabel("Eje Y") 
plt.title("Gráfico de Línea") 
plt.legend() 
plt.grid() 
plt.show()`

```


## **2. Gráfico de Dispersión (`plt.scatter()`)**

### **📌 Definición**

Representa la relación entre dos variables mediante puntos dispersos en el plano cartesiano.

### **📌 Uso Principal**

- Análisis de correlaciones entre variables.
- Visualización de datos experimentales.
- Identificación de agrupaciones o patrones.

### **📌 Parámetros Clave**

- `x, y`: Datos a representar.
- `color`: Color de los puntos (`'red'`, `'green'`).
- `s`: Tamaño de los puntos.
- `alpha`: Transparencia de los puntos.
- `marker`: Forma de los puntos (`'o'`, `'x'`, `'*'`).

### **📌 Ejemplo**
```python
x = np.random.rand(50) 
y = np.random.rand(50) 
plt.scatter(x, y, color='red', s=100, alpha=0.5, marker='o') plt.xlabel("Eje X") 
plt.ylabel("Eje Y") 
plt.title("Gráfico de Dispersión") 
plt.grid() 
plt.show()
```

---

## **3. Gráfico de Barras (`plt.bar()`)**

### **📌 Definición**

Muestra la relación entre una variable categórica y una numérica utilizando barras verticales o horizontales.

### **📌 Uso Principal**

- Comparación de valores entre diferentes categorías.
- Representación de datos de encuestas o estadísticas.

### **📌 Parámetros Clave**

- `x`: Categorías en el eje X.
- `height`: Valores asociados a cada categoría.
- `color`: Color de las barras.
- `width`: Ancho de las barras.
- `edgecolor`: Color del borde.

### **📌 Ejemplo**
```python
categorias = ["A", "B", "C", "D"] 
valores = [3, 7, 1, 8]  
plt.bar(categorias, valores, color='blue', edgecolor='black', width=0.5) plt.xlabel("Categorías")
plt.ylabel("Valores") 
plt.title("Gráfico de Barras") 
plt.grid(axis='y', linestyle='--', alpha=0.7) 
plt.show()`
```
---

## **4. Histograma (`plt.hist()`)**

### **📌 Definición**

Muestra la distribución de una variable numérica agrupando los valores en intervalos.

### **📌 Uso Principal**

- Análisis de la distribución de datos.
- Identificación de patrones en grandes volúmenes de información.

### **📌 Parámetros Clave**

- `x`: Datos a graficar.
- `bins`: Número de intervalos.
- `color`: Color de las barras.
- `alpha`: Transparencia.
- `edgecolor`: Color del borde.

### **📌 Ejemplo**

```python
data = np.random.randn(1000)  
plt.hist(data, bins=30, color='green', alpha=0.7, edgecolor='black') plt.xlabel("Valor") 
plt.ylabel("Frecuencia") 
plt.title("Histograma") 
plt.grid(axis='y', linestyle='--', alpha=0.7) 
plt.show()`
```



---

## **5. Gráfico de Pastel (`plt.pie()`)**

### **📌 Definición**

Muestra proporciones de diferentes categorías en un círculo.

### **📌 Uso Principal**

- Representación de porcentajes.
- Comparación de proporciones en encuestas o informes.

### **📌 Parámetros Clave**

- `x`: Valores de cada categoría.
- `labels`: Etiquetas de las categorías.
- `autopct`: Formato de los porcentajes.
- `colors`: Colores personalizados.
- `startangle`: Ángulo de inicio.

### **📌 Ejemplo**

```python
valores = [30, 25, 20, 15, 10] 
etiquetas = ["A", "B", "C", "D", "E"]  
plt.pie(valores, labels=etiquetas, autopct='%1.1f%%', startangle=140, colors=['red', 'blue', 'green', 'purple', 'orange']) 
plt.title("Gráfico de Pastel") 
plt.show()`
```


---

## **6. Gráfico de Áreas (`plt.fill_between()`)**

### **📌 Definición**

Muestra una región sombreada entre una curva y el eje X.

### **📌 Uso Principal**

- Visualización de tendencias acumuladas.
- Análisis de áreas bajo una curva.

### **📌 Parámetros Clave**

- `x, y`: Datos de la curva.
- `color`: Color del área.
- `alpha`: Transparencia.

### **📌 Ejemplo**

python

CopyEdit

`x = np.linspace(1, 6, 100) y = x ** 2  plt.fill_between(x, y, color='skyblue', alpha=0.5) plt.plot(x, y, color='blue') plt.xlabel("Eje X") plt.ylabel("Eje Y") plt.title("Gráfico de Área") plt.show()`

---

## **Conclusión**

Cada tipo de gráfico en Matplotlib tiene sus propias aplicaciones y configuraciones. Para elegir el adecuado, considera:

- **Datos categóricos** → Gráficos de barras o pastel.
- **Tendencias** → Gráfico de líneas.
- **Relaciones entre variables** → Gráfico de dispersión.
- **Distribución de datos** → Histogramas.