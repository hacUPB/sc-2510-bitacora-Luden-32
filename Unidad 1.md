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

```bash
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

```bash
@100
D = A
@32
M = D
```

<aside>
💡

Aquí aplica lo mismo, el valor del primer arroba, va en la posición dada por el numero del segundo arroba

</aside>
