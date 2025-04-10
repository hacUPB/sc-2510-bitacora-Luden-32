## 1. Suma de los Primeros 100 Números Naturales en Hack Assembly (Implementación con While)

### Código (RAE1

```nasm
Programa para sumar los primeros 100 números naturales
// Variables:
//   i = R0 (contador)
//   sum = R1 (acumulador)

@R0
M=1         // Inicializa i a 1
@R1
M=0         // Inicializa sum a 0

(LOOP)
@R0         // Cargar i
D=M
@100        // Cargar el valor 100
D=D-A       // Realiza la operación i - 100
@END
D;JGT       // Si i > 100, salta a END

@R0         // Recupera i
D=M
@R1         // Recupera sum
M=D+M       // Actualiza sum sumando i

@R0         // Incrementa i
M=M+1       // i = i + 1

@LOOP
0;JMP       // Repite el ciclo

(END)
@END
0;JMP       // Bucle infinito para finalizar

```

### Pruebas (RAE2)

### Pruebas de Componentes

1. **Inicialización:**
    - Se verificó que R0 (i) se establezca en 1.
    - Se comprobó que R1 (sum) inicie en 0.
    - Con el simulador se inspeccionaron los valores tras ejecutar las dos primeras instrucciones.
2. **Evaluación de la condición del bucle:**
    - Se hizo un test manual del cálculo de i-100 para distintos valores de i.
    - Se confirmó que el salto a END solo se produce cuando i supera 100.
3. **Acumulación:**
    - Se validó que la suma se actualizara correctamente en cada iteración.
    - Se comprobó que i aumente en 1 en cada ciclo.

### Prueba Integral

1. **Ejecución completa:**
    - Se ejecutó el programa hasta alcanzar el bucle infinito en END.
    - Se verificó que el valor final en R1 fuese 5050 (suma correcta de 1 a 100).
    - Se constató que R0 finalizara en 101, lo que indica que el bucle se repitió 100 veces.
2. **Casos límite:**
    - Se modificó temporalmente el límite a 5 para confirmar que la suma (1+2+3+4+5=15) fuese correcta.
    - Se cambió el valor inicial para probar que el algoritmo funcione con diferentes rangos.

### Análisis Conceptual

1. **Implementación de variables:**
    - La variable `i` se ubica en R0.
    - La variable `sum` se usa en R1.
    - En Hack Assembly, las variables se implementan en posiciones específicas de memoria o registros.
2. **Direcciones de memoria:**
    - R0 se encuentra en la dirección 0.
    - R1 se encuentra en la dirección 1.
    - Los registros R0-R15 son posiciones reservadas en la RAM (0-15).
3. **Implementación del ciclo while:**
    - Se utiliza una etiqueta (LOOP) para marcar el inicio del ciclo.
    - La condición se evalúa restando i-100 y saltando cuando el resultado es positivo.
    - El salto incondicional (0;JMP) genera la repetición.
4. **Naturaleza de las variables:**
    - Una variable es una ubicación de memoria identificada por un nombre que almacena datos.
    - Su dirección indica dónde se encuentra en memoria (por ejemplo, R0=0).
    - El contenido es el valor almacenado (por ejemplo, M=1).
5. **Almacenamiento:**
    - La variable i se almacena en RAM[0] (R0).
    - La memoria Hack dispone de 32K palabras de 16 bits.
    - Los primeros 16 registros (R0-R15) tienen un uso especial y predefinido.

---

## 2. Suma de los Primeros 100 Números Naturales Usando Estructura For en Hack Assembly (Retos 2 y 3)

### Código Transformado (RAE1)

```nasm

// Programa para sumar los primeros 100 números naturales utilizando la estructura for
// Variables:
//   sum = R0 (acumulador)
//   El contador i se maneja de forma implícita en el flujo del for

@R0
M=0         // Inicializa sum a 0
@1
D=A
@R1
M=D         // Inicializa i a 1 (almacenado en R1)

(FOR_LOOP)
@R1         // Recupera i
D=M
@100        // Cargar límite 100
D=D-A       // Realiza i - 100
@END
D;JGT       // Si i > 100, salta a END

@R1         // Dentro del cuerpo del for: sum += i
D=M
@R0
M=D+M       // Actualiza sum sumando i

@R1         // Incrementa i
M=M+1

@FOR_LOOP
0;JMP       // Vuelve al inicio del ciclo

(END)
@END
0;JMP       // Bucle infinito de finalización

```

Comparación con la Versión While

### Estructura For vs While

1. **Inicialización:**
    - En el for, la inicialización de i se hace justo antes del ciclo.
    - En el while, la inicialización se realizó al inicio del programa.
2. **Flujo de control:**
    - La estructura for combina inicialización, condición e incremento de manera compacta.
    - La versión while dispersaba estos elementos en distintas partes del código.
3. **Uso de registros:**
    - En esta implementación, se utiliza R1 para el contador i (antes estaba en R0).
    - R0 queda dedicado exclusivamente para el acumulador sum.

### Pruebas Realizadas (RAE2)

### Pruebas de Componentes

1. **Inicialización:**
    - Se verificó que R0 (sum) se inicialice en 0.
    - Se comprobó que R1 (i) inicie en 1.
    - Se inspeccionaron los valores iniciales con el simulador.
2. **Condición del for:**
    - Se probó el cálculo de i-100 para diferentes valores.
    - Se confirmó que el salto a END se produce cuando i > 100.
3. **Cuerpo e incremento:**
    - Se validó que la suma se actualice correctamente.
    - Se comprobó que i incremente en 1 en cada iteración.

### Prueba Integral

1. **Ejecución completa:**
    - El programa concluyó con R0 = 5050, sumando correctamente 1 a 100.
    - R1 terminó con el valor 101, lo que confirma que se realizaron 100 iteraciones (más la iteración final).
2. **Casos de prueba:**
    - Se cambió temporalmente el límite a 10 (resultado esperado 55).
    - Se modificó el valor inicial a 50 para probar sumas parciales.

### Análisis Conceptual

1. **Estructura for en Assembly:**
    - La inicialización se realiza antes de la etiqueta del bucle.
    - La condición se evalúa al inicio de cada iteración.
    - El incremento se ejecuta al final del cuerpo del bucle.
2. **Diferencia Conceptual (código en C++):**
    
    ```cpp
    // Versión while original:
    int i = 1;
    int sum = 0;
    while (i <= 100) {
        sum += i;
        i++;
    }
    
    // Versión for equivalente:
    int sum = 0;
    for (int i = 1; i <= 100; i++) {
        sum += i;
    }
    ```
    
3. **for vs while:**
    - Ambas implementaciones generan código máquina equivalente.
    - La versión for organiza de manera más clara los componentes lógicos.
    - El rendimiento de ambas versiones es idéntico.

---

## 3. Respuestas sobre Punteros en C++ (Reto 4)

### 1. ¿Cómo se declara un puntero en C++?

**Respuesta:**

Se declara utilizando el operador `*` junto al tipo de dato al que apuntará.

**Ejemplo:**

```cpp
int *p;  // p es un puntero a un entero
```

- Aquí, `int *p` indica que `p` almacenará la dirección de memoria (obtenida con `&`) de una variable de tipo entero.
- Es importante notar que `p` no contiene un valor entero, sino una dirección donde se encuentra dicho entero.

---

### 2. ¿Cómo se define un puntero en C++?

**Respuesta:**

Se define asignándole la dirección de una variable mediante el operador `&`.

**Ejemplo:**

```cpp
int a = 10;
int *p = &a;  // p apunta a a

```

- `&a` devuelve la dirección de memoria de la variable `a`.
- Al asignar `p = &a`, se guarda esa dirección en `p`, haciendo que `p` apunte a `a`.

---

### 3. ¿Cómo se almacena en C++ la dirección de memoria de una variable?

**Respuesta:**

Utilizando el operador `&` (ampersand).

**Ejemplo:**

```cpp

int x = 5;
int *ptr = &x;  // ptr almacena la dirección de x

```

- `&x` obtiene la ubicación en memoria donde se encuentra `x`.
- Así, `ptr` contiene dicha dirección y no el valor 5.

---

### 4. ¿Cómo se modifica el valor de la variable a la que apunta un puntero?

**Respuesta:**

Se utiliza el operador `*` (asterisco) para desreferenciar el puntero.

**Ejemplo:**

```cpp
int a = 10;
int *p = &a;  // p apunta a a
*p = 20;      // Ahora a vale 20

```

- `p` representa "el valor almacenado en la dirección que contiene p".
- Con `p = 20` se modifica directamente el valor de `a`, ya que `p` apunta a esa dirección.

---

### Resumen Gráfico

```nasm

int a = 10;    // a se encuentra en memoria, por ejemplo en la dirección 0x1000
int *p = &a;   // p guarda 0x1000, que es la dirección de a
*p = 20;       // Se modifica el contenido de 0x1000; ahora a es 20

```

### ¿Por qué son útiles los punteros?

- Permiten modificar variables de forma indirecta.
- Se utilizan en arreglos, estructuras dinámicas y en funciones que requieren alterar variables externas.

---

## 4. Traducción a Lenguaje Ensamblador (Hack Assembly) – Implementación de Punteros (Reto 5)

### Código en Hack Assembly

```nasm

// int a = 10;
@10        // Cargar el valor 10 en A
D=A        // Copiar 10 a D
@a        // Crear etiqueta 'a' (dirección de memoria)
M=D        // Guardar 10 en RAM[a]

// int *p; (declaración, opcional en Hack)
@p        // Crear etiqueta 'p' (puntero)
M=0        // Inicializar p a NULL (opcional)

// p = &a; (p apunta a a)
@a        // Cargar la dirección de 'a'
D=A       // Copiar la dirección a D
@p        // Seleccionar la dirección de 'p'
M=D       // Guardar la dirección de a en p

// *p = 20; (modificar a través del puntero)
@p        // Cargar el valor de p (la dirección de 'a')
A=M       // Acceder a la dirección apuntada por p
M=20      // Asignar 20 en RAM[a]

(END)
@END
0;JMP     // Bucle infinito para finalizar

```

### Explicación Paso a Paso

1. **`int a = 10;`**
    - `@10` carga 10 en el registro A.
    - `D=A` copia ese valor a D.
    - `@a` define la etiqueta 'a' (por ejemplo, en la dirección 16 de RAM).
    - `M=D` almacena 10 en RAM[a].
2. **`int *p;` (Declaración)**
    - `@p` define la etiqueta 'p' (por ejemplo, en la dirección 17).
    - `M=0` inicializa p en 0 (equivalente a NULL).
3. **`p = &a;`**
    - `@a` obtiene la dirección de 'a'.
    - `D=A` guarda esta dirección en D.
    - `@p` accede a la dirección de 'p'.
    - `M=D` almacena la dirección de a en p (ahora p apunta a a).
4. **`p = 20;`**
    - `@p` carga el valor de p (la dirección de a).
    - `A=M` desreferencia p para acceder a RAM[a].
    - `M=20` asigna 20 a RAM[a].

---

### Paso a Paso (RAE2)

1. **Verificación de `a = 10`:**
    - Tras `M=D`, se debe confirmar que RAM[a] sea 10.
2. **Puntero `p` contiene `&a`:**
    - Después de guardar la dirección, RAM[p] debe mostrar la dirección de a (por ejemplo, 16).
3. **Modificación mediante `p`:**
    - Al ejecutar `M=20`, se verifica que RAM[a] se actualice a 20.
4. **Flujo de Ejecución:**
    - Con el simulador se observa cómo cambian A y D en cada paso.

### Diagrama de Memoria Final

```
RAM[16] (a): 20   // a se modificó a 20 a través de *p
RAM[17] (p): 16   // p sigue apuntando a a

```

---

## 5. Implementación en Hack Assembly con Explicación Detallada (Reto 6)

### Código Completo

```nasm

// Inicialización de variables
@10       // Cargar valor 10
D=A       // Guardar 10 en D
@a        // Seleccionar dirección 'a'
M=D       // Guardar 10 en a (RAM[a] = 10)

@5        // Cargar valor 5
D=A       // Guardar 5 en D
@b        // Seleccionar dirección 'b'
M=D       // Guardar 5 en b (RAM[b] = 5)

// Manejo del puntero
@a        // Cargar dirección de 'a'
D=A       // Guardar esa dirección en D
@p        // Seleccionar dirección 'p'
M=D       // Guardar la dirección de a en p (p = &a)

// Lectura mediante puntero
@p        // Cargar valor de p (que es &a)
A=M       // Acceder a la dirección almacenada en p (a)
D=M       // Leer el valor en a (10)
@b        // Seleccionar dirección de 'b'
M=D       // Almacenar el valor leído en b (b = *p)

// Bucle infinito para finalizar
(END)
@END
0;JMP

```

### Explicación Detallada

1. **Inicialización de variables:**
    - Se asigna 10 a `a` y 5 a `b` usando las etiquetas correspondientes.
2. **Configuración del puntero:**
    - Se obtiene la dirección de `a` y se guarda en `p`, haciendo que p apunte a a.
3. **Lectura mediante puntero:**
    - Se desreferencia `p` para leer el valor en `a` (10) y se copia ese valor a `b`.

### Visualización del Estado de Memoria

- **Estado inicial:**
    
    ```
    
    a: 10
    b: 5
    p: ?
    
    ```
    
- **Después de `p = &a`:**
    
    ```
    
    a: 10
    b: 5
    p: dirección_de_a
    
    ```
    
- **Después de `b = *p`:**
    
    ```
    
    a: 10
    b: 10  (valor copiado desde a)
    p: dirección_de_a
    
    ```
    

### Pruebas y Verificación (RAE2)

1. **Inicialización:**
    - Verificar que RAM[a] sea 10 y RAM[b] sea 5.
2. **Verificación del puntero:**
    - Confirmar que RAM[p] contenga la dirección correcta de a.
3. **Verificación de lectura:**
    - Asegurar que, tras la operación, RAM[b] se actualice a 10.
4. **Flujo de Ejecución:**
    - Comprobar que A, D y M se actualicen según lo esperado en el simulador.

---

## 6. Traducción a Hack Assembly (Reto 7)

### Código

```nasm

// ------ Inicialización de variables ------
@10       // Cargar 10 en A
D=A       // Copiar 10 a D
@a        // Seleccionar dirección de 'a'
M=D       // Guardar 10 en RAM[a] (int a = 10;)

@5        // Cargar 5 en A
D=A       // Copiar 5 a D
@b        // Seleccionar dirección de 'b'
M=D       // Guardar 5 en RAM[b] (int b = 5;)

// ------ Manejo del puntero ------
@a        // Cargar la dirección de 'a'
D=A       // Copiar la dirección a D
@p        // Seleccionar dirección de 'p'
M=D       // Guardar la dirección de 'a' en RAM[p] (p = &a;)

// ------ Lectura mediante puntero ------
@p        // Cargar el valor de 'p' (que contiene &a)
A=M       // Acceder a la dirección guardada en p (RAM[a])
D=M       // Leer el valor (10) y almacenarlo en D
@b        // Seleccionar dirección de 'b'
M=D       // Guardar el valor leído en RAM[b] (b = *p;)

// ------ Bucle infinito para finalizar ------
(END)
@END
0;JMP

```

### Paso a Paso

### 1. Inicialización de variables (`a` y `b`)

- Para `int a = 10;` se hace:
    
    ```nasm
    
    @10
    D=A
    @a
    M=D
    
    ```
    
- Para `int b = 5;` se repite el mismo proceso:
    
    ```nasm
    
    @5
    D=A
    @b
    M=D
    
    ```
    

### 2. Declaración y asignación del puntero (`p = &a`)

- Se obtiene la dirección de `a` y se asigna a `p`:
    
    ```nasm
    
    @a
    D=A
    @p
    M=D
    
    ```
    

### 3. Lectura mediante puntero (`b = *p`)

- Se desreferencia `p` para obtener el valor en `a`:
    
    ```nasm
    
    @p
    A=M
    D=M
    @b
    M=D
    
    ```
    

### 4. Bucle infinito (para finalizar el programa)

```nasm

(END)
@END
0;JMP

```

### Diagrama de Memoria Final

| Dirección | Etiqueta | Valor |
| --- | --- | --- |
| 16 | a | 10 |
| 17 | b | 10 |
| 18 | p | 16 |

**Resultado Final:**

- `a` se mantiene en **10**.
- `b` se actualiza de **5** a **10** (ya que se copia el valor de a mediante *p).
- `p` contiene la dirección de a (por ejemplo, 16).

### Pruebas (RAE2)

1. **Inicialización:**
    - RAM[a] debe ser 10 y RAM[b] debe comenzar en 5.
2. **Asignación del puntero:**
    - Tras `p = &a`, RAM[p] debe tener la dirección de a.
3. **Lectura mediante puntero:**
    - Luego de `b = *p`, RAM[b] debe actualizarse a 10.
4. **Verificación del flujo:**
    - Usar el simulador para confirmar que A, D y M se modifican como se espera.

---

## 7. Implementación de Función con Llamado y Retorno en Hack Assembly (Reto 9)

### Estructura General del Código

```nasm

// Programa principal
(MAIN)
    // Llamada a suma(6, 9)
    @6
    D=A
    @ARG
    M=D     // ARG[0] = 6 (primer parámetro)
    @9
    D=A
    @ARG+1
    M=D     // ARG[1] = 9 (segundo parámetro)

    @RETURN_ADDRESS
    D=A
    @SP
    A=M
    M=D     // Guarda la dirección de retorno
    @SP
    M=M+1   // Incrementa el puntero de pila

    @SUMA
    0;JMP   // Salta a la función suma

(RETURN_ADDRESS)
    // Continuación de main tras el retorno
    @SP
    AM=M-1  // Decrementa el puntero de pila
    D=M     // Obtiene el valor de retorno (15)
    @c
    M=D     // c = resultado de suma(6,9)

    // Simulación de cout: se muestra el valor en pantalla
    @c
    D=M
    @SCREEN
    M=D     // Se muestra el valor en pantalla

    @END
    0;JMP   // Termina el programa

// Función suma
(SUMA)
    // Prólogo: guardar el frame pointer
    @LCL
    D=M
    @SP
    A=M
    M=D
    @SP
    M=M+1

    // Cuerpo de la función: realizar la suma
    @ARG
    D=M
    @0
    A=D+A
    D=M     // D = a (primer argumento)
    @ARG
    D=M
    @1
    A=D+A
    D=D+M   // D = a + b (suma)

    // Almacenar en variable local
    @SP
    A=M
    M=D     // var = a + b
    @SP
    M=M+1

    // Epílogo: preparar el retorno
    @SP
    AM=M-1
    D=M     // D = valor de retorno (var)
    @R13
    M=D     // Guardar el valor de retorno temporalmente

    // Restaurar el frame pointer
    @SP
    AM=M-1
    D=M
    @LCL
    M=D

    // Saltar a la dirección de retorno
    @SP
    AM=M-1
    A=M
    0;JMP   // Retorna al llamador

(END)
    @END
    0;JMP   // Bucle infinito para finalizar el programa

```

### Explicación Detallada

1. **Manejo de la Pila y Llamada a Función**
    - **Paso de parámetros:** Se colocan los argumentos (6 y 9) en posiciones consecutivas en memoria (ARG y ARG+1).
    - **Dirección de retorno:** Se guarda la dirección a la que se debe volver en la pila antes de saltar a la función.
    - **Frame pointer:** Se guarda el contexto actual antes de entrar a la función.
2. **Implementación de la Función suma**
    - **Acceso a parámetros:** Se recuperan los argumentos desde ARG.
    - **Operación de suma:** Se carga el primer argumento, se suma el segundo y se almacena el resultado.
    - **Variable local:** El resultado se guarda temporalmente en la pila.
3. **Retorno de la Función**
    - **Preparación del valor de retorno:** Se guarda en R13 de forma temporal.
    - **Restauración del contexto:** Se recupera el frame pointer.
    - **Salto de retorno:** Se utiliza la dirección guardada en la pila para volver al programa principal.
4. **Continuación en main**
    - **Obtención del resultado:** Se retira el valor de retorno de la pila y se asigna a `c`.
    - **Salida:** Se simula una salida mediante la escritura en SCREEN.
5. **Optimización y Limitaciones**
    - Se podría optimizar eliminando variables locales innecesarias.
    - Hack Assembly no soporta entrada/salida directa, por lo que se simula la salida escribiendo en SCREEN.
    - El manejo de strings es complejo y se omite en esta implementación.
