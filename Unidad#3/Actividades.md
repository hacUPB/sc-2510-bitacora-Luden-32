# Actividad 1

### ¿Cuál es el resultado que se obtiene al ejecutar este programa?

- Al ejecutar el programa, se abre una pestaña con un fondo negro, al mover el cursor por la ventana, este tiene un circulo blanco que sigue al cursor.

# Actividad 2

### ¿Qué fue lo que incluimos en el archivo .h?

- Lo que se incluyó en el código fue simplemente:

```cpp
void mouseMoved(int x, int y );
void mousePressed(int x, int y, int button);

    private:

        vector<ofVec2f> particles;
        ofColor particleColor;
```

Justo despues donde dice “void draw();”eliminando lo que hay debajo y reemplazándolo por lo anterior.

Este pedazo del código permite detectar si el mouse clickea.

### ¿Cómo funciona la aplicación?

- La aplicación lo que hace es detectar cuando se clickea en la pantalla y cambia de color el rastro que deja el circulo que se encuentra alrededor del cursor. Una vez que el rastro del circulo llega a cierta longitud, este empieza a eliminar el excedente. 
Es decir, que el rastro siempre va a tener cierta longitud especifica. A mi parecer, se asemeja a una serpiente.

### ¿Qué hace la función mouseMoved?

- Esta función detecta la posición del mouse en la pantalla con valores de x,y

### ¿Qué hace la función mousePressed?

- esta función detecta cuando el mouse es presionado o se clickea con el

### ¿Qué hace la función setup?

- Esta función lo que hace es colocar los valores iniciales en la aplicación, por asi decirlo. 
Es lo que se ejecuta al inicio de los programas y se usa para configurar el estado inicial de la aplicación.

### ¿Qué hace la función update?

- Actualmente… nada
Pero, su función es actualizar el estado de la aplicación antes que draw() renderice el siguiente frame.

### ¿Qué hace la función draw?

- se encarga de dibujar/renderizar todos lo elementos gráficos en la ventana.

# Actividad 3

## ¿Qué hace cada función?

### ¿Qué hace la función mouseMoved?

- Esta función detecta la posición del mouse en la pantalla con valores de x,y

### ¿Qué hace la función mousePressed?

- esta función detecta cuando el mouse es presionado o se clickea con el

### ¿Qué hace la función setup?

- Esta función lo que hace es colocar los valores iniciales en la aplicación, por asi decirlo. 
Es lo que se ejecuta al inicio de los programas y se usa para configurar el estado inicial de la aplicación.

### ¿Qué hace la función update?

- Actualmente… nada
Pero, su función es actualizar el estado de la aplicación antes que draw() renderice el siguiente frame.

### ¿Qué hace la función draw?

- se encarga de dibujar/renderizar todos lo elementos gráficos en la ventana.

## ¿Qué hace cada línea de código?

### Setup:

- `ofBackground(0);` - Establece el fondo de la ventana a negro (0).
- `particleColor = ofColor::white;` - Inicializa el color de las partículas a blanco.

### Update:

- Esta función está vacía, no realiza ninguna actualización en este momento.

### Draw:

- `for (auto& pos : particles) {` - Recorre cada posición almacenada en el vector de partículas.
- `ofSetColor(particleColor);` - Establece el color actual para dibujar los círculos.
- `ofDrawCircle(pos.x, pos.y, 50);` - Dibuja un círculo en cada posición con radio de 50 píxeles.

### MouseMoved:

- `particles.push_back(ofVec2f(x, y));` - Añade la posición actual del cursor al final del vector de partículas.
- `if (particles.size() > 100) {` - Verifica si el vector tiene más de 100 posiciones.
- `particles.erase(particles.begin());` - Elimina la partícula más antigua (la primera del vector) cuando hay más de 100.

### MousePressed:

- `particleColor = ofColor(ofRandom(255), ofRandom(255), ofRandom(255));` - Cambia el color de las partículas a un color aleatorio.
- `ofBackground(ofRandom(255), ofRandom(255), ofRandom(255));` - Cambia el color de fondo a un color aleatorio cuando se hace clic.

# Actividad 5

- **Puntero:** Variable que almacena una dirección de memoria.
- **Localización en el código:**
    - Vector `vector<Sphere*> spheres;`
    - Variable `Sphere* selectedSphere;`
- **Inicialización:**
    - Se asignan objetos dinámicos al vector con `new Sphere(...)`.
    - Se establece `selectedSphere = nullptr;` en `setup()` y luego se actualiza cuando se detecta un clic.
- **Uso:**
    - Permiten gestionar dinámicamente una colección de esferas.
    - Facilitan la selección de una esfera que luego se actualiza para seguir el cursor del mouse.
- **Contenido:**
    - La dirección de memoria donde reside un objeto `Sphere`.

# Actividad 6

El problema principal en el código es que una vez seleccionada una esfera (al hacer clic sobre ella), el puntero **selectedSphere** nunca se "libera" o se deselecciona. Esto provoca que, aunque se haga clic en otra parte o se termine la acción de arrastrar, la esfera seleccionada siga siguiendo el movimiento del mouse, ya que su posición se actualiza continuamente en el método **update()**.

### ¿Por qué sucede esto?

En el ejemplo actual se asigna la esfera seleccionada en el evento **mousePressed**:

```cpp

void ofApp::mousePressed(int x, int y, int button){
    if(button == OF_MOUSE_BUTTON_LEFT){
        for (auto sphere : spheres) {
            float distance = ofDist(x, y, sphere->getX(), sphere->getY());
            if (distance < sphere->getRadius()) {
                selectedSphere = sphere;
                break;
            }
        }
    }
}

```

Luego, en el método **update()**, se actualiza la posición de la esfera seleccionada:

```cpp

void ofApp::update(){
    if (selectedSphere != nullptr) {
        selectedSphere->update(ofGetMouseX(), ofGetMouseY());
    }
}

```

Debido a que no se implementa ningún mecanismo para volver a asignar **selectedSphere** a `nullptr` o para deseleccionar la esfera, ésta continúa moviéndose con el mouse incluso si ya no se desea arrastrarla.

### ¿Cómo solucionarlo?

Una forma común de resolver este problema es agregar el evento **mouseReleased**. Este evento se dispara cuando se suelta el botón del mouse y es el momento adecuado para "soltar" la esfera, es decir, restablecer el puntero **selectedSphere** a `nullptr`.

Por ejemplo, puedes hacer lo siguiente:

1. **Declararlo en el archivo `ofApp.h`:**
    
    ```cpp
    
    class ofApp : public ofBaseApp{
        public:
            void setup();
            void update();
            void draw();
    
            void mouseMoved(int x, int y );
            void mousePressed(int x, int y, int button);
            void mouseReleased(int x, int y, int button);  // Agregado
    
        private:
            vector<Sphere*> spheres;
            Sphere* selectedSphere;
    };
    
    ```
    
2. **Implementarlo en el archivo `ofApp.cpp`:**
    
    ```cpp
    
    void ofApp::mouseReleased(int x, int y, int button){
        if(button == OF_MOUSE_BUTTON_LEFT){
            selectedSphere = nullptr;
        }
    }
    
    ```
    

De esta forma, cuando se suelte el botón izquierdo del mouse, se deselecciona la esfera, evitando que siga moviéndose junto al cursor.

# **Actividad 7**

- *Antes:* Al presionar `'c'`, se creaba un objeto en el stack y se almacenaba su dirección, generando un puntero inválido una vez que la función terminaba.
- *Después:* Con la modificación, se crea un objeto en el heap, que persiste mientras no se libere, permitiendo que la esfera se dibuje correctamente.

# **Actividad 8**

**En ofApp.cpp**

```cpp
#include "ofApp.h"

void ofApp::setup(){
    ofBackground(0);
}

void ofApp::update(){
}

void ofApp::draw(){
    // Dibuja todas las esferas creadas en el heap.
    for(auto sphere : heapSpheres){
        if(sphere != nullptr) {
            sphere->draw();
        }
    }
    
    // Si se ha creado un objeto "stackSphere", dibujalo.
    if(stackSphere != nullptr){
        stackSphere->draw();
    }
    
    // Opcional: Mostrar instrucciones en pantalla.
    ofSetColor(255);
    ofDrawBitmapString("Presiona 'h' para crear una esfera en el heap (persistente).", 20, 20);
    ofDrawBitmapString("Presiona 's' para crear una esfera en el stack y copiarla (persistente).", 20, 40);
}

void ofApp::keyPressed(int key){
    if(key == 'h'){
        // Crear objeto en el heap y almacenarlo en el vector.
        Sphere* s = new Sphere(ofRandomWidth(), ofRandomHeight(), 30);
        heapSpheres.push_back(s);
    }
    else if(key == 's'){
        // Crear un objeto en el stack.
        Sphere localSphere(ofRandomWidth(), ofRandomHeight(), 30);
        
        // Si ya existe un objeto en stackSphere, lo liberamos.
        if(stackSphere != nullptr){
            delete stackSphere;
            stackSphere = nullptr;
        }
        
        // Copiamos el objeto local (creado en el stack) a uno nuevo en el heap
        // y lo asignamos a la variable miembro stackSphere.
        stackSphere = new Sphere(localSphere);
    }
}

```

**En ofApp.h**

```cpp
#pragma once

#include "ofMain.h"

// Definición de la clase Sphere.
class Sphere {
public:
    // Constructor.
    Sphere(float x, float y, float radius)
    : x(x), y(y), radius(radius) {
        color = ofColor(ofRandom(255), ofRandom(255), ofRandom(255));
    }
    
    // Constructor de copia (para poder copiar objetos de forma segura).
    Sphere(const Sphere& other) {
        x = other.x;
        y = other.y;
        radius = other.radius;
        color = other.color;
    }
    
    // Método para dibujar la esfera.
    void draw() const {
        ofSetColor(color);
        ofDrawCircle(x, y, radius);
    }
    
    float x, y, radius;
    ofColor color;
};

class ofApp : public ofBaseApp {
public:
    void setup();
    void update();
    void draw();
    
    void keyPressed(int key);
    
private:
    std::vector<Sphere*> heapSpheres; // Almacena objetos creados en el heap.
    Sphere* stackSphere = nullptr;      // Variable para almacenar la copia persistente del objeto creado en el stack.
};

```

**Inicio de la aplicación:**

El fondo se inicializa en negro. No se dibuja ninguna esfera al inicio.

**Creación de esferas en el heap ('h'):**

Cada vez que presionas la tecla **'h'**, se crea una nueva esfera en el heap y se agrega al vector `heapSpheres`. Todas estas esferas se dibujan de forma persistente en cada actualización de la pantalla.

**Creación de una esfera en el stack y copia a heap ('s'):**

Al presionar la tecla **'s'**, se crea una esfera de forma temporal en el stack. Debido a que un objeto en el stack desaparecería al finalizar la función, se copia a una variable miembro llamada `stackSphere` (almacenada en el heap). De esta forma, dicha esfera se dibuja cada ciclo en el método `draw()`. Si vuelves a presionar la tecla 's', se elimina la esfera anterior (liberando memoria) y se crea una nueva.

**Dibujo de todas las esferas:**

Durante cada ciclo de `draw()` se actualiza la pantalla:

- Se recorren y dibujan las esferas del vector `heapSpheres`.
- Se dibuja la esfera almacenada en `stackSphere`, si existe.
- Se muestran instrucciones para el usuario.

# **Actividad 9**

- Al presionar `'f'` se elimina el último objeto creado en el heap mediante `delete`, liberando memoria y removiendo el puntero del vector, lo que evita fugas de memoria y mantiene el contenedor actualizado.
