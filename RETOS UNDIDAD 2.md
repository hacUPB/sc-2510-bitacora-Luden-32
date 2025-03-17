### **Programa en Ensamblador para Sumar los Primeros 100 Números Naturales**

**Código (RAE1):**

```asm
// Suma de 1 a 100 (ciclo while)
@i      // Dirección 16 (variable i)
M=1     // i = 1
@sum    // Dirección 17 (variable sum)
M=0     // sum = 0

(LOOP)
@i
D=M     // D = i
@100
D=D-A   // D = i - 100
@END
D;JGT   // Si i > 100, salta a END
@i
D=M     // D = i
@sum
M=D+M   // sum += i
@i
M=M+1   // i++
@LOOP
0;JMP   // Repetir

(END)
@END
0;JMP   // Bucle infinito (fin del programa)
```

**Explicación:**

- **Variables `i` y `sum`**:
    - Implementadas en direcciones de memoria (`@i` = 16, `@sum` = 17).
    - `i` se inicializa en 1, `sum` en 0.
- **Ciclo `while`**:
    - Se compara `i` con 100 (`D = i - 100`).
    - Si `i > 100`, termina el programa.
    - Si no, suma `i` a `sum` e incrementa `i`.

---

### **Implementación con Ciclo `for`**

**Código (RAE1):**

```asm
// Suma de 1 a 100 (ciclo for)
@sum
M=0     // sum = 0

@i
M=1     // i = 1 (inicialización del for)

(FOR)
@i
D=M
@100
D=D-A
@END
D;JGT   // Si i > 100, termina
@i
D=M
@sum
M=D+M   // sum += i
@i
M=M+1   // i++
@FOR
0;JMP   // Repetir

(END)
@END
0;JMP
```

**Diferencias**:

- La estructura del `for` se simula con la inicialización de `i` antes del bucle y el incremento al final.

---

### **Traducción de Programas con Punteros**

**Caso 1: Modificar variable mediante puntero (C++ → Ensamblador)**

```asm
// int a = 10; int *p; p = &a; *p = 20;
@10
D=A
@a      // Dirección de a (ej. 18)
M=D     // a = 10

@a
D=A
@p      // Dirección de p (ej. 19)
M=D     // p = &a (guarda dirección de a en p)

@p
A=M     // Accede a la dirección almacenada en p (a)
M=20    // *p = 20
```

**Caso 2: Leer variable mediante puntero (C++ → Ensamblador)**assembly

```asm
// int a=10; int b=5; int *p; p=&a; b=*p;
@10
D=A
@a      // Dirección de a (ej. 20)
M=D     // a = 10

@5
D=A
@b      // Dirección de b (ej. 21)
M=D     // b = 5

@a
D=A
@p      // Dirección de p (ej. 22)
M=D     // p = &a

@p
A=M     // Accede a la dirección en p (a)
D=M     // D = *p (valor de a)
@b
M=D     // b = D (b = 10)
```

---

### **Preguntas Conceptuales**

1. **`int *pvar;`**: Declara un puntero `pvar` a un entero.
2. **`pvar = var;`**: Asigna la dirección de `var` a `pvar` (si `var` es una variable, debería ser `pvar = &var`).
3. **`var2 = *pvar;`**: Asigna a `var2` el valor de la variable apuntada por `pvar`.
4. **`pvar = &var3;`**: Hace que `pvar` apunte a `var3`.

---

### **Traducción de Función `suma` en Ensamblador**

**Código (RAE1):**

```asm
// Función suma(a, b)
(SUMA)
@ARG1   // Parámetro a (dirección 23)
D=M
@ARG2   // Parámetro b (dirección 24)
D=D+M
@RET    // Valor de retorno (dirección 25)
M=D
@RET
A=M
0;JMP   // Retorna al caller

// Llamada desde main
(MAIN)
@6
D=A
@ARG1
M=D     // a = 6
@9
D=A
@ARG2
M=D     // b = 9
@SUMA_RETURN // Dirección de retorno
D=A
@RET_ADDR
M=D
@SUMA
0;JMP   // Llama a suma

(SUMA_RETURN)
@RET
D=M
@c      // Variable c (dirección 26)
M=D     // c = suma(6,9)
```

**Explicación**:

- **Paso de parámetros**: Se usan direcciones `ARG1` y `ARG2`.
- **Valor de retorno**: Almacenado en `RET`.
- **Manejo de retorno**: `RET_ADDR` guarda la dirección para retornar después de la función.

---

### **Pruebas (RAE2)**

1. **Pruebas de Partes**:
    - **Variables**: Verificar que `i` y `sum` se inicialicen correctamente.
    - **Ciclo**: Simular manualmente 2-3 iteraciones para confirmar que `sum` se actualiza.
    - **Punteros**: Chequear que las direcciones se asignen correctamente y que el contenido se modifique.
2. **Pruebas del Programa Completo**:
    - **Suma 1-100**: Ejecutar en el simulador y verificar que `sum` sea 5050 al final.
    - **Punteros**: Usar el simulador para rastrear cómo `p` apunta a `a` y modifica su valor.
