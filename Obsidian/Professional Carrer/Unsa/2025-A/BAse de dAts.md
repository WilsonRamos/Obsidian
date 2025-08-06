
## 🔍 **FLUJO REAL PASO A PASO:**

### **PASO 1: Hash Calculation**

cpp

```cpp
std::string imei = "868018071302858";
size_t hash_value = HashFunction::djb2(imei);
// hash_value = 2847593721 (ejemplo)
```

### **PASO 2: Directory Lookup**

cpp

```cpp
int global_depth = 2;  // Ejemplo
int bucket_index = hash_value & ((1 << global_depth) - 1);
// bucket_index = 2847593721 & 3 = 1
```

### **PASO 3: Bucket Access**

cpp

```cpp
Bucket* bucket = directory[bucket_index];  // directory[1]
```

### **PASO 4: Linear Search en Bucket**

cpp

```cpp
for (auto& entry : bucket->entries) {
    if (entry.key == "868018071302858") {
        return entry.record_reference;  // PhysicalAddress + SlotID
    }
}
```

### **PASO 5: Record Reference Obtenido**

cpp

```cpp
RecordReference ref(PhysicalAddress(2,1), 5);
// Página 2, Sector 1, Slot 5
```



```cpp
5
```

---

## 🌳 **B+ Tree se usa para TIMESTAMP:**

### **`SELECT * FROM dataGPS WHERE timestamp BETWEEN '2025-01-15 08:00:00' AND '2025-01-15 09:00:00'`**

**Aquí SÍ usas B+ Tree:**

### **PASO 1: Navegar a primera hoja**

```
Buscar "2025-01-15 08:00:00" en B+ Tree
→ Llegar a nodo hoja que contiene el rango
```

### **PASO 2: Recorrido horizontal**

```
Desde la hoja encontrada:
→ Recorrer hojas enlazadas horizontalmente
→ Recolectar todos los RecordReference en el rango
→ Hasta llegar a "2025-01-15 09:00:00"
```