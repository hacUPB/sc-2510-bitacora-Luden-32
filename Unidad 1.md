# Unidad 1
## Actividad 1
### ¿Qué es un computador digital moderno?

- Una computadora digital moderna, es una maquina electrónica que procesa datos de forma binaria para resolver problemas. Los datos se almacenan en la memoria y se procesan mediante una secuencia de instrucciones, o programa.

### ¿Cuáles son sus partes?

Las partes de un computador digital moderno son:

- **Gabinete**: La carcasa de metal o plástico que contiene los componentes internos
- **Fuente de alimentación**: El componente que suministra energía a la computadora
- **Placa base**: La tarjeta madre que conecta los componentes internos
- **Memoria RAM**: El componente que almacena temporalmente datos e instrucciones
- **Unidad central de procesamiento (CPU)**: El componente que ejecuta las órdenes que se dan a través del teclado y el ratón
- **Disco rígido**: El componente que almacena información
- **Tarjeta gráfica (GPU)**: El componente que procesa y renderiza imágenes, videos y gráficos
- **Pantalla (monitor)**: El componente que muestra la interfaz gráfica del sistema operativo
- **Teclado**: El dispositivo de entrada que permite al usuario introducir datos y comandos
- **Ratón**: El dispositivo de entrada que permite al usuario realizar clics y navegar por páginas web y documentos

## Actividad 2
### ¿Qué es un programa?

- Un programa es un conjunto de acciones o actividades ordenadas para alcanzar un objetivo especifico. Tambien puede referirse a un conjunto de instrucciones que permiten a una computadora realizar tareas.

### ¿Qué es un lenguaje ensamblador?

- es un lenguaje de programación de bajo nivel que se utiliza para comunicarse directamente con el hardware de una computadora. Es una representación simbólica del código maquina, que es el lenguaje que entiende directamente la computadora.

### ¿Qué es el lenguaje de maquina?

- Tambien conocido como código de maquina, es el lenguaje fundamental que entiende directamente el hardware de una computadora.

## Actividad 3
### ¿Qué son PC, D y A?

- **PC**: Program Counter (contador de programa) Es un registro en la CPU que guarda la dirección de la siguiente instrucción a ejecutar.
- **D**: Puede referirse al Dato en ciertos contextos, o a registros como el Registro D en algunas arquitecturas.
- **A**: Generalmente representa el **Acumulador,** un registro especial en la CPU donde se almacenan temporalmente resultados de operaciones aritméticas y lógicas.

### ¿Para qué los usa la CPU?

- **PC: l**leva la dirección de la siguiente instrucción a ejecutar. Se incrementa automáticamente despues de cada instrucción o salta a otra dirección en caso de una instrucción de control de flujo
- **D**: Se usa para almacenar valores temporales que la CPU necesita para operar, como operandos en operaciones aritméticas o lógicas. En algunas arquitecturas, puede referirse a un registro específico.
- **A**: Es un registro especial donde se guardan resultados intermedios de operaciones aritméticas con el acumulador.

Básicamente, estos registros ayudan a la CPU a llevar el control de la ejecución del programa y procesar datos eficientemente.

## Actividad 4

Considera el siguiente fragmento de código en lenguaje ensamblador:

```asm
@16384
D = A
@16
M = D
```

<aside>
💡

Al parecer, el primer @ es el valor, el segundo @ es la posición.

</aside>

El resultado de este programa es que guarda en la posición 16 de la RAM el valor 16384. Ahora escribe un programa en lenguaje ensamblador que guarde en la posición 32 de la RAM un 100.

```asm
@100
D = A
@32
M = D
```

<aside>
💡

Aquí aplica lo mismo, el valor del primer arroba, va en la posición dada por el numero del segundo arroba

</aside>


## Reto
```asm
// 1. Cargar en D el valor 1978
@1978
D=A

// 2. Guardar en la posición 100 de la RAM el número 69
@69
D=A
@100
M=D

// 3. Guardar en la posición 200 de la RAM el contenido de la posición 24 de la RAM
@24
D=M
@200
M=D

// 4. Leer lo que hay en la posición 100 de la RAM, restar 15 y guardar el resultado en la posición 100
@100
D=M
@15
D=D-A
@100
M=D

// 5. Sumar el contenido de la posición 0 de la RAM, el contenido de la posición 1 de la RAM y la constante 69; guardar el resultado en la posición 2 de la RAM
@0
D=M
@1
D=D+M
@69
D=D+A
@2
M=D

// 6. Si el valor almacenado en D es igual a 0, saltar a la posición 100 de la ROM
@100
D;JEQ

// 7. Si el valor almacenado en la posición 100 de la RAM es menor a 100, saltar a la posición 20 de la ROM
@100
D=M      // D = RAM[100]
@100
D=D-A    // D = RAM[100] - 100
@20
D;JLT
```

## 8 Considera el siguiente programa:
```asm
@var1
D = M
@var2
D = D + M
@var3
M = D
```
### ¿Qué hace este programa?

El programa realiza lo siguiente:

1. **Carga en D** el valor almacenado en la dirección asignada a `var1`.
2. **Suma a D** el valor almacenado en la dirección asignada a `var2`.
3. **Guarda en `var3`** el resultado de la suma.

En resumen, **suma los valores de `var1` y `var2` y almacena el resultado en `var3`**.

**¿En qué posición de la memoria están `var1`, `var2` y `var3`? ¿Por qué en esas posiciones?**

En la máquina Hack, las variables definidas en el código (simbolizadas, por ejemplo, como `var1`, `var2` y `var3`) se asignan automáticamente a direcciones de memoria a partir de la **posición 16**. Esto se debe a que las direcciones **0 a 15** están reservadas para registros predefinidos (R0 a R15).

Por lo tanto, si estas son las primeras variables que se encuentran en el código, se asignarán de la siguiente forma:

- `var1` estará en la posición **16**.
- `var2` estará en la posición **17**.
- `var3` estará en la posición **18**.

## 9 Considera el siguiente programa:
```c++
i = 1
sum = 0
sum = sum + i
i = i + 1
```
La traducción a lenguaje ensamblador del programa anterior es:
```asm
// i = 1
@i
M=1
// sum = 0
@sum
M=0
// sum = sum + i
@i
D=M
@sum
M=D+M
// i = i + 1
@i
D=M+1
@i
M=D
```
### ¿Qué hace el programa?

El programa realiza lo siguiente:

1. **Inicializa** la variable `i` con el valor **1**.
2. **Inicializa** la variable `sum` con el valor **0**.
3. **Suma** el valor de `i` a `sum`, es decir, realiza:`sum = sum + i`(Después de esta operación, `sum` vale 1).
4. **Incrementa** la variable `i` en 1, de forma que:`i = i + 1`(Ahora `i` vale 2).

### ¿En qué parte de la memoria RAM están `i` y `sum`? ¿Por qué en esas posiciones?

En el sistema Hack, cuando se usan variables simbólicas en ensamblador, el ensamblador asigna direcciones de memoria a partir de la **dirección 16**, ya que las direcciones **0 a 15** están reservadas para registros predefinidos (R0 a R15).

Por ello, si `i` y `sum` son las primeras variables que aparecen en el código:

- `i` se asignará a la **dirección 16**.
- `sum` se asignará a la **dirección 17**.

### Optimización de la instrucción `i = i + 1`

El código original para incrementar `i` es:

```asm
// i = i + 1
@i
D=M+1
@i
M=D
```

Este fragmento **ya está optimizado** en el sentido de que utiliza únicamente **dos instrucciones** para realizar la operación de incremento.

- La primera instrucción `@i` carga la dirección de `i` y `D=M+1` lee su valor, le suma 1 y almacena el resultado en D.
- La segunda instrucción `@i` vuelve a cargar la dirección de `i` y `M=D` guarda el nuevo valor.


