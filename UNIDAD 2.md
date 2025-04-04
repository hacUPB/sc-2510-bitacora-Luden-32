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
(START)
    @KBD
    D=M
    @ERASE
    D;JEQ         // Si no se presiona ninguna tecla, ir a ERASE

    @KBD
    D=M
    @100
    D=D-A
    @DRAW
    D;JEQ         // Si se presiona 'd', ir a DRAW

    @ERASE
    0;JMP        // Si se presionó otra tecla, borrar

// ----------------------------------------------------------------
// DRAW: Dibuja la imagen (código generado por nand2tetris)
// ----------------------------------------------------------------
(DRAW)
    @SCREEN
    D=A
    @R12
    AD=D+M
    @1024 
    D=D+A 
    A=D-A 
    M=D-A 
    AD=A+1 
    @64 
    D=D+A 
    A=D-A 
    M=D-A 
    D=A 
    @31
    AD=D+A
    @1024 
    D=D+A 
    A=D-A 
    M=D-A 
    AD=A+1 
    @64 
    D=D+A 
    A=D-A 
    M=D-A 
    D=A 
    @31
    AD=D+A
    @1024 
    D=D+A 
    A=D-A 
    M=D-A 
    AD=A+1 
    @64 
    D=D+A 
    A=D-A 
    M=D-A 
    D=A 
    @31
    AD=D+A
    @1024 
    D=D+A 
    A=D-A 
    M=D-A 
    AD=A+1 
    @64 
    D=D+A 
    A=D-A 
    M=D-A 
    D=A 
    @31
    AD=D+A
    @1024 
    D=D+A 
    A=D-A 
    M=D-A 
    AD=A+1 
    @64 
    D=D+A 
    A=D-A 
    M=D-A 
    D=A 
    @31
    AD=D+A
    @1024 
    D=D+A 
    A=D-A 
    M=D-A 
    AD=A+1 
    @64 
    D=D+A 
    A=D-A 
    M=D-A 
    D=A 
    @31
    AD=D+A
    @1024 
    D=D+A 
    A=D-A 
    M=D-A 
    AD=A+1 
    @64 
    D=D+A 
    A=D-A 
    M=D-A 
    D=A 
    @31
    AD=D+A
    @1024 
    D=D+A 
    A=D-A 
    M=D-A 
    AD=A+1 
    @64 
    D=D+A 
    A=D-A 
    M=D-A 
    D=A 
    @159
    AD=D+A
    M=-1
    AD=A+1 
    M=-1
    @START
    0;JMP

// ----------------------------------------------------------------
// ERASE: Borra la pantalla (SCREEN)
// ----------------------------------------------------------------
(ERASE)
    @SCREEN
    D=A
    @R13
    M=D

    @8192
    D=A
    @R14
    M=D

(ERASE_LOOP)
    @0
    D=A
    @R13
    A=M
    M=0

    @R13
    M=M+1

    @R14
    M=M-1
    D=M
    @ERASE_LOOP
    D;JGT

    @START
    0;JMP
```

### Actividad 4: 'd' muestra imagen, 'e' borra
```asm
// Entrada: espera por la tecla D (ASCII 68) o E (ASCII 69)
(START)
    @KBD
    D=M
    @68
    D=D-A
    @DRAW
    D;JEQ

    @KBD
    D=M
    @69
    D=D-A
    @ERASE
    D;JEQ

    @START
    0;JMP

// ----------------------------------------------------------------
// Rutina DRAW: Dibuja la imagen (código generado por nand2tetris)
// ----------------------------------------------------------------
(DRAW)
    @SCREEN
    D=A
    @R12
    AD=D+M
    @1024 
    D=D+A 
    A=D-A 
    M=D-A 
    AD=A+1 
    @64 
    D=D+A 
    A=D-A 
    M=D-A 
    D=A 
    @31
    AD=D+A
    @1024 
    D=D+A 
    A=D-A 
    M=D-A 
    AD=A+1 
    @64 
    D=D+A 
    A=D-A 
    M=D-A 
    D=A 
    @31
    AD=D+A
    @1024 
    D=D+A 
    A=D-A 
    M=D-A 
    AD=A+1 
    @64 
    D=D+A 
    A=D-A 
    M=D-A 
    D=A 
    @31
    AD=D+A
    @1024 
    D=D+A 
    A=D-A 
    M=D-A 
    AD=A+1 
    @64 
    D=D+A 
    A=D-A 
    M=D-A 
    D=A 
    @31
    AD=D+A
    @1024 
    D=D+A 
    A=D-A 
    M=D-A 
    AD=A+1 
    @64 
    D=D+A 
    A=D-A 
    M=D-A 
    D=A 
    @31
    AD=D+A
    @1024 
    D=D+A 
    A=D-A 
    M=D-A 
    AD=A+1 
    @64 
    D=D+A 
    A=D-A 
    M=D-A 
    D=A 
    @31
    AD=D+A
    @1024 
    D=D+A 
    A=D-A 
    M=D-A 
    AD=A+1 
    @64 
    D=D+A 
    A=D-A 
    M=D-A 
    D=A 
    @31
    AD=D+A
    @1024 
    D=D+A 
    A=D-A 
    M=D-A 
    AD=A+1 
    @64 
    D=D+A 
    A=D-A 
    M=D-A 
    D=A 
    @31
    AD=D+A
    @1024 
    D=D+A 
    A=D-A 
    M=D-A 
    AD=A+1 
    @64 
    D=D+A 
    A=D-A 
    M=D-A 
    D=A 
    @159
    AD=D+A
    M=-1
    AD=A+1 
    M=-1
    @R13
    A=M
    D;JMP

// ----------------------------------------------------------------
// Rutina ERASE: Borra la pantalla (SCREEN)
// ----------------------------------------------------------------
(ERASE)
    @SCREEN
    D=A
    @R13
    M=D

    @8192
    D=A
    @R14
    M=D

(ERASE_LOOP)
    @0
    D=A
    @R13
    A=M
    M=0

    @R13
    M=M+1

    @R14
    M=M-1
    D=M
    @ERASE_LOOP
    D;JGT

    @START
    0;JMP
```
