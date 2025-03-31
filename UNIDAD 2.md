## Actividad 1
### Qué es la entrada-salida mapeada a memoria?

La **entrada-salida mapeada a memoria** (memory-mapped I/O) es una técnica de hardware/software que permite interactuar con dispositivos periféricos (como pantallas, teclados, sensores, etc.) utilizando **direcciones de memoria específicas**. En lugar de usar instrucciones especializadas para E/S, los dispositivos se controlan leyendo o escribiendo datos en posiciones de memoria predefinidas. Cada dirección de memoria reservada para este fin está asociada a un registro físico del dispositivo periférico, permitiendo manipular su comportamiento mediante operaciones de lectura/escritura en la RAM.

---

### ¿Cómo se implementa en la plataforma Hack?

En la plataforma Hack, la E/S mapeada a memoria se implementa así:

- **Pantalla**:
    - Dirección base: `16384 (0x4000)`.
    - Organización: 256 filas × 512 píxeles (8K de memoria).
    - Cada fila se representa con 32 palabras de 16 bits.
    - Para modificar un píxel en la posición `(r, c)`:
        - Calcular la dirección: `RAM[16384 + r*32 + c//16]`.
        - Modificar el bit `(c % 16)` (LSB = columna 0, MSB = columna 15).
    - Ejemplo: Escribir `1` en el bit 0 de `SCREEN` pinta un píxel negro en la esquina superior izquierda.
- **Teclado**:
    - Dirección: `24576 (0x6000)`.
    - El valor en `RAM[24576]` contiene el código ASCII de la tecla presionada (o `0` si no hay tecla presionada).

Los dispositivos se sincronizan mediante **bucles de actualización continua** que leen/escriben estas posiciones de memoria.

## Actividad 3: Mostrar imagen con 'd', borrar sin tecla

**Programa en Hack Assembly**:

```asm
(MAIN)
    @KBD
    D=M
    @68 // 'd' en ASCII
    D=D-A
    @DRAW
    D;JEQ

    // Borrar pantalla
    @SCREEN
    D=A
    @addr
    M=D
(CLEAR_LOOP)
    @addr
    A=M
    M=0
    @addr
    M=M+1
    D=M
    @KBD
    D=D-A
    @MAIN
    D;JEQ
    @CLEAR_LOOP
    0;JMP

(DRAW)
    // Asume datos de imagen en IMAGE_DATA
    @IMAGE_DATA
    D=A
    @img
    M=D
    @SCREEN
    D=A
    @addr
    M=D
(DRAW_LOOP)
    @img
    A=M
    D=M
    @addr
    A=M
    M=D
    @img
    M=M+1
    @addr
    M=M+1
    @KBD
    D=A
    @addr
    D=D-M
    @MAIN
    D;JEQ
    @DRAW_LOOP
    0;JMP
```

### Actividad 4: 'd' muestra imagen, 'e' borra
```asm
(MAIN)
    @KBD
    D=M
    @current
    M=D

    // Verificar 'd'
    @current
    D=M
    @68
    D=D-A
    @DRAW
    D;JEQ

    // Verificar 'e'
    @current
    D=M
    @69
    D=D-A
    @CLEAR
    D;JEQ

    @MAIN
    0;JMP

(DRAW)
    @IMAGE_DATA
    D=A
    @img
    M=D
    @SCREEN
    D=A
    @addr
    M=D
(D_LOOP)
    @img
    A=M
    D=M
    @addr
    A=M
    M=D
    @img
    M=M+1
    @addr
    M=M+1
    @KBD
    D=A
    @addr
    D=D-M
    @MAIN
    D;JEQ
    @D_LOOP
    0;JMP

(CLEAR)
    @SCREEN
    D=A
    @addr
    M=D
(E_LOOP)
    @addr
    A=M
    M=0
    @addr
    M=M+1
    @KBD
    D=A
    @addr
    D=D-M
    @MAIN
    D;JEQ
    @E_LOOP
    0;JMP
```
