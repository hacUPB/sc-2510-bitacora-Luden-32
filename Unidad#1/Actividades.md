# Unidad 1
## **Actividad 1**
**¿Qué es un computador digital moderno?**

Una computadora digital moderna es una máquina electrónica que procesa datos en formato binario para resolver problemas. Los datos se almacenan en la memoria y se procesan mediante una secuencia de instrucciones, conocida como programa informático.

**¿Cuáles son sus partes?**

Las partes principales son:

- **Gabinete**: Carcasa de metal/plástico que protege los componentes internos.
- **Fuente de alimentación**: Proporciona energía eléctrica a todos los componentes.
- **Placa base**: Circuito principal que interconecta los componentes.
- **Memoria RAM**: Almacena datos e instrucciones de forma temporal.
- **CPU (Unidad Central de Procesamiento)**: Ejecuta instrucciones y procesa datos.
- **Disco duro**: Almacena datos de forma permanente.
- **GPU (Tarjeta gráfica)**: Procesa gráficos y vídeos.
- **Monitor**: Muestra la interfaz gráfica al usuario.
- **Teclado**: Dispositivo de entrada para texto/comandos.
- **Ratón**: Dispositivo de entrada para interactuar con la interfaz.

---

## **Actividad 2**

**¿Qué es un programa?**

Es un conjunto de instrucciones estructuradas que indican a la computadora cómo realizar una tarea específica.

**¿Qué es un lenguaje ensamblador?**

Lenguaje de programación de bajo nivel que representa simbólicamente el código máquina, permitiendo control directo del hardware.

**¿Qué es el lenguaje de máquina?**

Código binario (ceros y unos) que la CPU ejecuta directamente. Es el único lenguaje entendido por el hardware.

---

## **Actividad 3**

**¿Qué son PC, D y A?**

- **PC (Program Counter)**: Registro que almacena la dirección de la próxima instrucción a ejecutar.
- **D**: Registro de datos para almacenar valores temporales en operaciones.
- **A (Acumulador)**: Registro especial para resultados intermedios de operaciones aritméticas/lógicas.

**¿Para qué los usa la CPU?**

- **PC**: Controla el flujo del programa, actualizándose automáticamente o con saltos.
- **D**: Almacena operandos para operaciones (ej: sumar dos números).
- **A**: Facilita cálculos al mantener resultados parciales accesibles.

Estos registros optimizan la ejecución de instrucciones y el manejo de datos.

---

## **Actividad 4**

**Código original:**

```
@16384
D = A     // Almacena 16384 en el registro D
@16
M = D     // Guarda D (16384) en la dirección 16 de la RAM
```

**Explicación:**
```
El primer `@` carga un valor en el registro A, y el segundo `@` selecciona una dirección de memoria.
@100
D = A     // Carga el valor 100 en D
@32
M = D     // Almacena D (100) en la dirección 32 de la RAM
```
