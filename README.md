# Vig.IA MVP

Sistema de evaluación de riesgo espacial basado en visión artificial para entornos domésticos.

---

# Descripción

Este MVP tiene como objetivo identificar automáticamente la ubicación de un individuo dentro de una maqueta de vivienda utilizando visión artificial en tiempo real.

La maqueta está compuesta por tres zonas:

- Habitación
- Sala
- Cocina

Un muñeco será ubicado dentro de una de estas zonas y un modelo YOLO entrenado con imágenes etiquetadas determinará en qué espacio se encuentra.

Una vez identificada la ubicación, el sistema asignará un nivel de riesgo asociado:

| Zona | Nivel de riesgo |
|--------|--------|
| Habitación | Bajo |
| Sala | Medio |
| Cocina | Alto |

Si el sistema no puede determinar con suficiente confianza la ubicación observada, mostrará un estado temporal de validación:

```text
IDENTIFICANDO NIVEL DE RIESGO...
```

---

# Objetivo

Desarrollar un prototipo funcional que demuestre la capacidad de:

1. Capturar video en tiempo real desde la cámara de un celular.
2. Identificar visualmente la zona donde se encuentra el individuo.
3. Clasificar el nivel de riesgo asociado a dicha ubicación.
4. Generar alertas según el nivel de riesgo detectado.

Este MVP representa la primera fase de Vig.IA, una plataforma de supervisión inteligente orientada a población dependiente dentro del entorno doméstico.

---

# Arquitectura del sistema

```text
Cámara del celular
          ↓
Captura de video
          ↓
Modelo YOLO
          ↓
Clasificación de zona
          ↓
Motor de riesgo
          ↓
Generación de alertas
```

---

# Dataset

El conjunto de entrenamiento estará conformado por imágenes de una maqueta doméstica.

Cada imagen será etiquetada según la habitación observada.

## Clases

```text
habitacion
sala
cocina
```

## Ejemplos

```text
img_001.jpg → habitacion
img_002.jpg → habitacion
img_003.jpg → sala
img_004.jpg → sala
img_005.jpg → cocina
img_006.jpg → cocina
```

---

# Lógica de evaluación

Una vez procesado cada frame de video:

## Habitación

```text
Ubicación detectada:
HABITACIÓN

Nivel de riesgo:
BAJO
```

## Sala

```text
Ubicación detectada:
SALA

Nivel de riesgo:
MEDIO
```

## Cocina

```text
Ubicación detectada:
COCINA

Nivel de riesgo:
ALTO
```

## Ubicación no determinada

Si la confianza del modelo es inferior al umbral establecido:

```text
Ubicación detectada:
NO DETERMINADA

Estado:
IDENTIFICANDO NIVEL DE RIESGO...
```

---
# Demostración visual del sistema

A continuación se presentan imágenes que ilustran el funcionamiento del prototipo durante la detección en tiempo real.

---

## Maqueta utilizada para las pruebas

La maqueta representa un entorno doméstico simplificado compuesto por tres zonas:

- Habitación
- Sala
- Cocina

docs/images/maqueta-general.jpg

---

## Escenario 1: Riesgo Bajo (Habitación)

El modelo identifica que el muñeco se encuentra en la habitación.

**Resultado esperado:**

```text
Ubicación detectada:
HABITACIÓN

Nivel de riesgo:
BAJO
```

docs/images/habitacion.jpg

---

## Escenario 2: Riesgo Medio (Sala)

El modelo identifica que el muñeco se encuentra en la sala.

**Resultado esperado:**

```text
Ubicación detectada:
SALA

Nivel de riesgo:
MEDIO
```

docs/images/sala.jpg

---

## Escenario 3: Riesgo Alto (Cocina)

El modelo identifica que el muñeco se encuentra en la cocina.

**Resultado esperado:**

```text
Ubicación detectada:
COCINA

Nivel de riesgo:
ALTO
```

docs/images/cocina.jpg

---

## Escenario 4: Identificación en proceso

Cuando la confianza del modelo es insuficiente o la ubicación no puede determinarse claramente.

**Resultado esperado:**

```text
Ubicación detectada:
NO DETERMINADA

Estado:
IDENTIFICANDO NIVEL DE RIESGO...
```

docs/images/identificando.jpg

---

## Detección en tiempo real

Captura de la interfaz ejecutándose en vivo desde la cámara del celular.

docs/images/live-detection.jpg

---

## Flujo completo del sistema

```text
Cámara del celular
          ↓
Captura de video
          ↓
YOLO analiza la imagen
          ↓
Clasifica la habitación
          ↓
Determina el nivel de riesgo
          ↓
Genera la alerta correspondiente
```

docs/images/arquitectura-sistema.png

---

## Resultados esperados

| Ubicación detectada | Nivel de riesgo |
|--------------------|----------------|
| Habitación | Bajo |
| Sala | Medio |
| Cocina | Alto |
| No determinada | Identificando nivel de riesgo |


---
# Estructura del proyecto

```text
VigIA-MVP/
│
├── README.md
├── requirements.txt
├── .gitignore
│
├── dataset/
│   ├── train/
│   │   ├── habitacion/
│   │   ├── sala/
│   │   └── cocina/
│   │
│   └── val/
│       ├── habitacion/
│       ├── sala/
│       └── cocina/
│
├── models/
│   ├── yolov8n.pt
│   └── best.pt
│
├── src/
│   ├── train.py
│   ├── live_detection.py
│   ├── risk_engine.py
│   └── alerts.py
│
├── assets/
│   ├── images/
│   └── sounds/
│
└── docs/
     └── images/
      ├── maqueta-general.jpg
      ├── habitacion.jpg
      ├── sala.jpg
      ├── cocina.jpg
      ├── identificando.jpg
      ├── live-detection.jpg
      └── arquitectura-sistema.png


```

---

# Tecnologías

```text
Python
YOLOv8
Ultralytics
OpenCV
NumPy
Mobile Camera Streaming
```

---

# Motor de riesgo

El sistema utiliza una clasificación simple basada en ubicación.

```python
RISK_LEVELS = {
    "habitacion": "BAJO",
    "sala": "MEDIO",
    "cocina": "ALTO"
}
```

---

# Criterio de decisión

```python
if confidence < 0.70:
    resultado = "IDENTIFICANDO NIVEL DE RIESGO..."

elif clase == "habitacion":
    resultado = "RIESGO BAJO"

elif clase == "sala":
    resultado = "RIESGO MEDIO"

elif clase == "cocina":
    resultado = "RIESGO ALTO"
```

---

# Fases futuras

## Fase 1

Detección de ubicación mediante visión artificial.

## Fase 2

Integración de zonas seguras (Safe Zones).

## Fase 3

Seguimiento de individuos.

## Fase 4

Integración con sistema robótico móvil.

## Fase 5

Intervención automática mediante robot de asistencia.

---

# Ejecución

```bash
pip install -r requirements.txt

python src/live_detection.py
```

---

# Autores

Proyecto académico de investigación y desarrollo de Vig.IA para la materia Gestión de proyectos, universidad Pontificia Bolivariana, 2026.

- Laura Martínez Correa 
- Fernando Castro Portilla
- Santiago Gómez Galeano
- Maria Fernanda Toro Torres 

Sistema inteligente de supervisión, evaluación espacial de riesgo e intervención asistida mediante inteligencia artificial.
