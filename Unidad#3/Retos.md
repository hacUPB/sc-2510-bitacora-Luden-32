# Retos

en ofApp.cpp 

```c++
#include "ofApp.h"

void ofApp::setup() {
    ofSetVerticalSync(true);
    ofSetFrameRate(60);
    generateGrid();
}

void ofApp::update() {}

void ofApp::draw() {
    ofBackground(0);
    cam.begin();
    ofSetColor(255);
    for (auto& pos : spherePositions) {
        ofDrawSphere(pos, 5.0f); // Esferas de radio 5
    }
    if (sphereSelected) {
        ofSetColor(255, 0, 0);
        ofDrawSphere(selectedPosition, 8.0f); // Resaltar selección
    }
    cam.end();

    // Mostrar información
    ofSetColor(255);
    string info = "Controles:\n";
    info += "Tecla 'a/z': Aumentar/Disminuir amplitud\n";
    info += "Tecla 'd/c': Aumentar/Disminuir distDiv\n";
    info += "Tecla '+/-': Aumentar/Disminuir separación (xStep/yStep)\n";
    info += "Esfera seleccionada: ";
    if (sphereSelected) {
        info += "(" + ofToString(selectedPosition.x) + ", " + ofToString(selectedPosition.y) + ", " + ofToString(selectedPosition.z) + ")";
    }
    else {
        info += "Ninguna";
    }
    ofDrawBitmapString(info, 20, 20);
}

void ofApp::keyPressed(int key) {
    switch (key) {
    case 'a': amplitud += 5.0f; updateZValues(); break;
    case 'z': amplitud -= 5.0f; updateZValues(); break;
    case 'd': distDiv += 5.0f; updateZValues(); break;
    case 'c': distDiv -= 5.0f; updateZValues(); break;
    case '+': xStep += 5; yStep += 5; generateGrid(); break;
    case '-': xStep -= 5; yStep -= 5; generateGrid(); break;
    }
}

void ofApp::generateGrid() {
    spherePositions.clear();
    int width = ofGetWidth();
    int height = ofGetHeight();

    for (int x = -width / 2; x < width / 2; x += xStep) {
        for (int y = -height / 2; y < height / 2; y += yStep) {
            float distance = sqrt(x * x + y * y);
            float z = cos(distance / distDiv) * amplitud;
            spherePositions.emplace_back(x, y, z);
        }
    }
}

void ofApp::updateZValues() {
    for (auto& pos : spherePositions) {
        float distance = sqrt(pos.x * pos.x + pos.y * pos.y);
        pos.z = cos(distance / distDiv) * amplitud;
    }
}

void ofApp::mousePressed(int x, int y, int button) {
    glm::vec3 rayStart, rayEnd;
    convertMouseToRay(x, y, rayStart, rayEnd);
    glm::vec3 rayDir = rayEnd - rayStart;

    float minDistance = numeric_limits<float>::max();
    int selectedIdx = -1;
    for (size_t i = 0; i < spherePositions.size(); ++i) {
        glm::vec3 intersection;
        if (rayIntersectsSphere(rayStart, rayDir, spherePositions[i], 5.0f, intersection)) {
            float distance = glm::distance(rayStart, intersection);
            if (distance < minDistance) {
                minDistance = distance;
                selectedIdx = i;
            }
        }
    }

    if (selectedIdx != -1) {
        selectedPosition = spherePositions[selectedIdx];
        sphereSelected = true;
    }
    else {
        sphereSelected = false;
    }
}

void ofApp::convertMouseToRay(int mouseX, int mouseY, glm::vec3& rayStart, glm::vec3& rayEnd) {
    glm::mat4 modelview = cam.getModelViewMatrix();
    glm::mat4 projection = cam.getProjectionMatrix();
    ofRectangle viewport = ofGetCurrentViewport();

    float x = 2.0f * (mouseX - viewport.x) / viewport.width - 1.0f;
    float y = 1.0f - 2.0f * (mouseY - viewport.y) / viewport.height;

    glm::vec4 rayStartNDC(x, y, -1.0f, 1.0f);
    glm::vec4 rayEndNDC(x, y, 1.0f, 1.0f);

    glm::vec4 rayStartWorld = glm::inverse(projection * modelview) * rayStartNDC;
    glm::vec4 rayEndWorld = glm::inverse(projection * modelview) * rayEndNDC;

    rayStartWorld /= rayStartWorld.w;
    rayEndWorld /= rayEndWorld.w;

    rayStart = glm::vec3(rayStartWorld);
    rayEnd = glm::vec3(rayEndWorld);
}

bool ofApp::rayIntersectsSphere(const glm::vec3& rayStart, const glm::vec3& rayDir, const glm::vec3& sphereCenter, float sphereRadius, glm::vec3& intersectionPoint) {
    glm::vec3 oc = rayStart - sphereCenter;

    float a = glm::dot(rayDir, rayDir);
    float b = 2.0f * glm::dot(oc, rayDir);
    float c = glm::dot(oc, oc) - sphereRadius * sphereRadius;

    float discriminant = b * b - 4 * a * c;

    if (discriminant < 0) {
        return false;
    }
    else {
        float t = (-b - sqrt(discriminant)) / (2.0f * a);
        intersectionPoint = rayStart + t * rayDir;
        return true;
    }
}
```

en  ofApp.h 

```c++
#pragma once

#include "ofMain.h"
#include "ofEasyCam.h"

class ofApp : public ofBaseApp {
public:
    void setup();
    void update();
    void draw();
    void keyPressed(int key);
    void mousePressed(int x, int y, int button);

    ofEasyCam cam;
    vector<glm::vec3> spherePositions;
    glm::vec3 selectedPosition;
    bool sphereSelected = false;

    // Parámetros modificables
    float amplitud = 50.0f;
    float distDiv = 50.0f;
    int xStep = 30;
    int yStep = 30;

    // Funciones auxiliares
    void generateGrid();
    void updateZValues();
    void convertMouseToRay(int mouseX, int mouseY, glm::vec3& rayStart, glm::vec3& rayEnd);
    bool rayIntersectsSphere(const glm::vec3& rayStart, const glm::vec3& rayDir, const glm::vec3& sphereCenter, float sphereRadius, glm::vec3& intersectionPoint);
};
```

### **Controles de la Aplicación**

- **Teclado**:
    - **`a/z`**: Aumenta/Disminuye la amplitud (altura de las esferas en Z).
    - **`d/c`**: Aumenta/Disminuye **`distDiv`** (controla la frecuencia de la onda).
    - **`+/-`**: Aumenta/Disminuye la separación entre esferas (**`xStep`** y **`yStep`**).
- **Ratón**:
    - Click izquierdo: Selecciona una esfera (se mostrarán sus coordenadas).
    - Arrastrar: Rota la cámara para explorar la escena.
    - Rueda del ratón: Acerca/Aleja la cámara.
