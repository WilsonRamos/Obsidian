
---

### **Idea de Proyecto: Sistema de Gestión Bancaria BCP-UNSA**

**Empresa:** BCP-UNSA  
**Área:** Gestión de Cuentas, Transacciones y Clientes  
**Descripción:** Un sistema que permita la gestión de clientes, cuentas bancarias, y transacciones en un entorno bancario, con diferentes niveles de permisos para administradores y clientes.

---

### **1. Análisis del Minimundo**

1. **Usuarios Principales:**
    
    - **Administrador del banco:** Puede gestionar todas las cuentas y generar reportes.
    - **Clientes:** Pueden realizar transacciones, consultar su saldo, y actualizar datos personales.
2. **Procesos Clave:**
    
    - Gestión de clientes: Creación, actualización y eliminación de clientes.
    - Gestión de cuentas bancarias: Apertura, consulta y cierre de cuentas.
    - Transacciones: Depósitos, retiros, transferencias internas.
    - Reportes: Generación de reportes como movimientos por cuenta o balance general del banco.

---

### **. Modelo Conceptual (Diagrama EER)**

#### **Entidades Principales:**

1. **Cliente:**
    - ID_Cliente, Nombre, Apellidos, DNI, Dirección, Teléfono, Correo Electrónico.
2. **Cuenta Bancaria:**
    - Número de cuenta, Tipo (Ahorros, Corriente), Saldo, Fecha de apertura.
3. **Transacción:**
    - ID_Transacción, Tipo (Depósito, Retiro, Transferencia), Monto, Fecha.
4. **Empleado:**
    - ID_Empleado, Nombre, Cargo, Sucursal, Usuario, Contraseña.
5. **Sucursal:**
    - ID_Sucursal, Nombre, Dirección.
6. **Reporte:**
    - ID_Reporte, Tipo, Fecha, Descripción.

#### **Relaciones:**

- Un **Cliente** puede tener varias **Cuentas Bancarias.**
- Una **Cuenta Bancaria** puede estar asociada a varias **Transacciones.**
- Una **Sucursal** puede tener varios **Empleados.**
- Un **Empleado** puede generar varios **Reportes.**

---

### **3. Modelo Lógico**

El modelo lógico transformará el diagrama EER en tablas relacionales. Ejemplo:

#### **Tablas Principales:**

1. **Clientes (Clientes):**
    - `ID_Cliente` (PK), `Nombre`, `DNI`, `Correo`, etc.
2. **Cuentas (Cuentas_Bancarias):**
    - `Número_Cuenta` (PK), `Tipo`, `Saldo`, `ID_Cliente` (FK).
3. **Transacciones:**
    - `ID_Transacción` (PK), `Tipo`, `Monto`, `Número_Cuenta` (FK), `Fecha`.
4. **Empleados:**
    - `ID_Empleado` (PK), `Nombre`, `Sucursal_ID` (FK).
5. **Sucursales:**
    - `ID_Sucursal` (PK), `Nombre`, `Dirección`.

---

### **4. Modelo Físico**

- Implementa las tablas y relaciones en **MySQL** o cualquier SGBD.
- Llena cada tabla con al menos 25 registros. Ejemplo:
    - Clientes: Personas ficticias con nombres y DNIs.
    - Cuentas: Números de cuenta con saldo inicial.
    - Transacciones: Historial de depósitos, retiros, y transferencias.

---

### **5. Funcionalidades**

1. **Sistema Web (PHP o Java):**
    
    - Registro de nuevos clientes y cuentas.
    - Realización de transacciones:
        - **Depósitos:** Registrar monto y actualizar saldo.
        - **Retiros:** Verificar saldo antes de permitir el retiro.
        - **Transferencias internas:** Entre cuentas del banco.
    - Generación de reportes:
        - Movimientos de cuenta por fecha.
        - Balance general del banco.
2. **Roles y Permisos:**
    
    - **Administrador:**
        - Acceso completo al sistema.
        - Generación de reportes.
        - Gestión de clientes, cuentas y empleados.
    - **Cliente:**
        - Consultar saldo.
        - Realizar transacciones (retiros, depósitos, transferencias).
        - Actualizar datos personales.

---

### **6. Reportes (Mínimo 10)**

Algunos ejemplos:

1. Movimientos por cuenta (filtro: fechas).
2. Transacciones por tipo (depósitos, retiros, transferencias).
3. Saldo total del banco por tipo de cuenta.
4. Clientes con más de una cuenta bancaria.
5. Cuentas con saldo bajo.
6. Sucursales con más transacciones.
7. Clientes que han actualizado datos recientemente.
8. Resumen de transacciones diarias.
9. Empleados que generaron más reportes.
10. Clientes por sucursal.

---

### **7. Video de Presentación**

Para cumplir con los requisitos:

1. Explica:
    - La empresa (BCP-UNSA) y su área.
    - Cómo definiste el minimundo.
    - Modelos conceptual, lógico, y físico.
    - Funcionalidades del sistema.
2. Demuestra:
    - Inserción y actualización de datos.
    - Uso del sistema por los diferentes roles.
    - Generación de reportes.

---

### **Beneficios de Elegir un Banco**

- **Complejidad Realista:** Los bancos tienen procesos y entidades bien definidas.
- **Relaciones Claras:** Clientes, cuentas, y transacciones son fáciles de modelar.
- **Flexibilidad:** Puedes ajustar el alcance según el tiempo disponible.