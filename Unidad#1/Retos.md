### **Reto 1: Cargar 1978 en D**

```
@1978
D = A
```

- **Explicación**: `@1978` carga el valor 1978 en el registro A. `D = A` copia este valor al registro D.

---

### **Reto 2: Guardar 69 en RAM[100]**

```
@69
D = A
@100
M = D
```

- **Explicación**: Carga 69 en D y luego almacena D en la dirección 100 de la RAM.

---

### **Reto 3: Copiar RAM[24] a RAM[200]**

```
@24
D = M
@200
M = D
```

- **Explicación**: Lee el valor en RAM[24] a D, luego lo guarda en RAM[200].

---

### **Reto 4: Restar 15 a RAM[100]**

```
@100
D = M     // D = RAM[100]
@15
D = D - A // D = D - 15
@100
M = D     // Actualiza RAM[100]
```

- **Explicación**: Carga RAM[100], resta 15 y guarda el resultado.

---

### **Reto 5: Sumar RAM[0], RAM[1] y 69 → RAM[2]**

```
@0
D = M
@1
D = D + M
@69
D = D + A
@2
M = D
```

- **Explicación**: Suma secuencialmente los valores y guarda el resultado en RAM[2].

---

### **Reto 6: Salto si D == 0**

```
@100
D; JEQ
```

- **Explicación**: Si D es cero, salta a la instrucción en la dirección 100 de la ROM.

---

### **Reto 7: Salto si RAM[100] < 100**

```
@100
D = M
@100
D = D - A
@20
D; JLT
```

- **Explicación**: Resta 100 a RAM[100]. Si el resultado es negativo (RAM[100] < 100), salta a ROM[20].

---

### **Reto 8: Suma de Variables**

- **Qué hace**: `var3 = var1 + var2`.
- **Posiciones**: `var1`, `var2`, `var3` están en direcciones 16, 17 y 18 de la RAM (asignadas secuencialmente por el ensamblador).

---

### **Reto 9: Optimización de Incremento**

- **Código original**:
    
    ```
    @i
    D = M + 1
    @i
    M = D
    ```
    
- **Optimizado**:
    
    ```
    @i
    M = M + 1
    ```
    
- **Explicación**: Incrementa directamente `i` sin usar D.

---

### **Reto 10: 2 * R0 → R1**

```
@R0
D = M
D = D + D
@R1
M = D
```

- **Explicación**: Duplica el valor de R0 (equivalente a multiplicar por 2).

---

### **Reto 11: Programa de Decremento**

- **Qué hace**: Decrementa `i` desde 1000 hasta 0.
- **Variables**: `i` está en RAM (dirección asignada por el ensamblador, ej. 16).
- **Instrucción inicial**: `@1000` en ROM[0].
- **Símbolos**: `LOOP` y `CONT` son etiquetas en la ROM.

---

### **Reto 12: R4 = R1 + R2 + 69**

```
@R1
D = M
@R2
D = D + M
@69
D = D + A
@R4
M = D
```

---

### **Reto 13: If R0 >= 0 → R1 = 1 else -1**

```
@R0
D = M
@POSITIVE
D; JGE
@R1
M = -1
@LOOP
0; JMP
(POSITIVE)
@R1
M = 1
(LOOP)
@LOOP
0; JMP
```

---

### **Reto 14: R4 = RAM[R1]**

```
@R1
A = M   // A = dirección almacenada en R1
D = M   // D = valor en esa dirección
@R4
M = D
```

---

### **Reto 15: Inicializar Memoria con -1**

```
@R0
D = M
@ptr
M = D    // ptr = dirección inicial
@R1
D = M
@count
M = D    // count = tamaño
(LOOP)
@count
D = M
@END
D; JEQ   // Si count == 0, termina
@ptr
A = M
M = -1   // Escribe -1
@ptr
M = M + 1 // Avanza ptr
@count
M = M - 1 // Decrementa count
@LOOP
0; JMP
(END)
@END
0; JMP
```

---

### **Reto 16: Suma de un Arreglo**

```
// Asume arr en RAM[16], sum en 17, j en 18
@sum
M = 0
@j
M = 0
(LOOP)
@j
D = M
@10
D = D - A
@END
D; JGE    // Si j >= 10, termina
@arr
D = M
@j
A = D + M // Dirección de arr[j]
D = M
@sum
M = M + D
@j
M = M + 1
@LOOP
0; JMP
(END)
@END
0; JMP
```

---

### **Reto 17: Salto si D == 7**

```
@69      // Dirección de salto
D = D - A // A = 7, D = D - 7
D; JEQ   // Salta si D == 0 (originalmente D == 7)
```

---

### **Reto 19: Análisis de Código de Máquina**

- **Qué hace**: El programa probablemente dibuja en la pantalla. Las instrucciones manipulan direcciones de memoria relacionadas con el display y realizan operaciones de carga/almacenamiento.

---

### **Reto 20: Dibujar Bitmap al Presionar "d"**
```
(LOOP)
@KBD
D = M
@68     // Código ASCII de 'd'
D = D - A
@DRAW
D; JEQ  // Si se presiona 'd', dibuja
@LOOP
0; JMP
(DRAW)
// Código para copiar el bitmap a la pantalla (dirección 16384)
```
