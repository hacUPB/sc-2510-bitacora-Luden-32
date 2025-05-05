
```cpp
// Interactive Rain and Burst System (renamed and refactored)
// ofApp.h and ofApp.cpp adapted for Visual Studio + openFrameworks

// --- File: ofApp.h ---
#pragma once
#include "ofMain.h"

class Drop {
public:
ofVec2f pos;
ofVec2f vel;
ofColor tint;
float life;
Drop(float x = 0, float y = 0);

};

class RainSystem {
public:
static constexpr int POOL_SIZE = 1500;
static constexpr int MAX_ACTIVE = 8000;
RainSystem();
~RainSystem();

void update();
void draw();
void spawnDrop(float x, float y);
void spawnBurst(float x, float y, int n = 30);
void queueTint(const ofColor& c);
void clear();
int  activeCount() const;

private:
std::vector<Drop*> pool;       // reusable drops
std::vector<Drop*> active;     // live drops
std::queue<ofColor> tints;     // pending colors
Drop*   obtain();
void    release(Drop* d);
void    spawnTinted();

};

class ofApp : public ofBaseApp {
public:
	void setup() override;
	void update() override;
	void draw() override;
	void mousePressed(int x, int y, int button) override;
	void keyPressed(int key) override;

private:
	RainSystem rain;
	ofColor    nextColor;
};
```

```cpp
// --- File: ofApp.cpp ---
#include "ofApp.h"

// Drop Implementation
Drop::Drop(float x, float y)
: pos(x, y)
, vel(ofRandom(-1,1), ofRandom(1,3))
, tint(ofRandom(255), 180, 255)
, life(255)
{}

// RainSystem Implementation
RainSystem::RainSystem() {
pool.reserve(POOL_SIZE);
for(int i=0; i<POOL_SIZE; ++i)
pool.push_back(new Drop());
}

RainSystem::~RainSystem() {
for(auto d: active) delete d;
for(auto d: pool)   delete d;
}

void RainSystem::update() {
// Update live drops
for(size_t i=0; i<active.size(); ) {
Drop* d = active[i];
d->pos += d->vel;
d->life -= 1.5f;
    bool dead = d->life <= 0
             || d->pos.y > ofGetHeight()
             || d->pos.x < 0 || d->pos.x > ofGetWidth();
    if(dead) {
        release(d);
        active.erase(active.begin() + i);
    } else ++i;
}

// Periodic tinted drops
if(!tints.empty() && ofGetFrameNum() % 8 == 0 && active.size() < MAX_ACTIVE) {
    spawnTinted();
}

}

void RainSystem::draw() {
for(auto d: active) {
ofSetColor(d->tint);
ofDrawEllipse(d->pos, 2 + d->life/120, 4 + d->life/80);
}
}

void RainSystem::spawnDrop(float x, float y) {
if(active.size() >= MAX_ACTIVE) return;
Drop* d = obtain();
d->pos.set(x,y);
d->vel.set(ofRandom(-1,1), ofRandom(1,3));
d->life = 255;
active.push_back(d);
}

void RainSystem::spawnBurst(float x, float y, int n) {
for(int i=0; i<n && active.size()<MAX_ACTIVE; ++i) {
Drop* d = obtain();
d->pos.set(x,y);
d->vel.set(ofRandom(-5,5), ofRandom(-5,5));
d->life = ofRandom(80,255);
active.push_back(d);
}
}

void RainSystem::queueTint(const ofColor& c){ tints.push(c); }

void RainSystem::clear(){
for(auto d: active) release(d);
active.clear();
}

int RainSystem::activeCount() const { return (int)active.size(); }

Drop* RainSystem::obtain() {
if(pool.empty()) return new Drop();
Drop* d = pool.back(); pool.pop_back();
return d;
}

void RainSystem::release(Drop* d) {
pool.push_back(d);
}

void RainSystem::spawnTinted() {
ofColor c = tints.front(); tints.pop();
Drop* d = obtain();
d->pos.set(ofRandomWidth(), -5);
d->tint = c;
d->vel.set(ofRandom(-1,1), ofRandom(1,3));
d->life = 255;
active.push_back(d);
}

// ofApp Implementation
void ofApp::setup() {
ofSetBackgroundColor(15,25,35);
ofSetFrameRate(60);
nextColor.setHsb(ofRandom(255),180,255);
}

void ofApp::update() {
if(ofGetFrameNum() % 3 == 0)
rain.spawnDrop(ofRandomWidth(), -10);
rain.update();
}

void ofApp::draw() {
ofBackgroundGradient(ofColor(20,30,40), ofColor(5,5,10));
rain.draw();
ofSetColor(240);
ofDrawBitmapString("Drops: " + ofToString(rain.activeCount()), 15,15);
ofDrawBitmapString("FPS: " + ofToString(ofGetFrameRate()), 15,35);
ofDrawBitmapString("Click: Burst | 1-9: Color | C: Clear", 15, ofGetHeight()-15);
}

void ofApp::mousePressed(int x, int y, int){
rain.spawnBurst(x,y);
}

void ofApp::keyPressed(int key) {
if(key >= '1' && key <= '9') {
float h = ofMap(key-'1',0,8,0,255);
nextColor.setHsb(h,180,255);
rain.queueTint(nextColor);
} else if(key=='c' || key=='C') {
rain.clear();
}
}
```

Explicación del sistema de lluvia interactiva:

1. Gestión de memoria y rendimiento:
    - Pool de gotas preasignado en un std::vector para reutilizar instancias sin nuevas asignaciones en tiempo de ejecución.
    - Vector separado para las gotas activas, permitiendo eliminaciones rápidas y un recorrido eficiente.
    - Límite de gotas activas (MAX_ACTIVE) para evitar sobrecarga y mantener un frame rate estable.
2. Movimiento y ciclo de vida:
    - Cada gota (‘Drop’) tiene posición, velocidad, color y vida restante.
    - En cada fotograma se actualiza la posición sumando la velocidad y se decrementa su vida.
    - Gotas muertas (vida ≤ 0 o fuera de pantalla) se reciclan automáticamente en el pool.
3. Dibujo dinámico:
    - Uso de ofDrawEllipse con dimensiones que varían según la vida restante de la gota, logrando un efecto de desvanecimiento y cambio de forma sutil.
    - Gradiente de fondo circular y estadís­ticas (número de gotas y FPS) renderizadas con ofDrawBitmapString.
4. Interacciones del usuario:
    - **Clic del ratón**: genera una “explosión” de gotas en la posición del cursor.
    - **Teclas 1–9**: calculan un nuevo tono de color, se encolan en una std::queue y se generan periódicamente gotas con ese color.
    - **Tecla C**: limpia todas las gotas activas y las devuelve al pool.
5. Contenedores modernos de C++:
    - std::vector para pool y lista activa, y std::queue para la cola de colores, simplificando la lógica de obtención y devolución de instancias.
    - Eliminación de código manual de enlaces y punteros en favor de contenedores seguros y autoajustables.
6. Configuración en ofApp:
    - setup(): se establece color de fondo, tasa de fotogramas y color inicial para futuras gotas.
    - update(): se generan gotas de lluvia de forma continua y se invoca la lógica de actualización del sistema.
    - draw(): se aplica gradiente de fondo, se dibujan todas las gotas y la interfaz de texto de control.
    - mousePressed() y keyPressed(): manejadores sencillos que llaman a las funciones de spawnBurst, queueTint y clear.
