En Series y Dataframes :
	Pandas asigna índices numéricos predeterminados si no se especifica uno
	Si trabajas con datos que tienen identificadores únicos o claves importantes, puedes personalizar el índice para facilitar el acceso y análisis de datos.
	
### **¿Qué significa `axis` en Pandas?**
- **`axis=0` → Operación en columnas (por defecto)** → Se realiza la operación a lo largo de las filas.
	- La operación se aplica **a lo largo de las filas**, es decir, **afecta a las filas**.
- **`axis=1` → Operación en filas** → Se realiza la operación a lo largo de las columnas.
	- La operación se aplica **a lo largo de las columnas**, es decir, **afecta a las columnas**.

💡 Cuando usas `axis`, el eje en el que **trabajas** se "elimina" y el eje que **no trabajaste** **se conserva en el resultado**.

### **Regla clave:**

- **El eje que se especifica con `axis` es el que **"trabajas" o "operas"**, y el otro eje es el que se conserva.**