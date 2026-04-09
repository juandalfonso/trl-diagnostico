# 🔬 Diagnóstico TRL — Herramienta de Evaluación de Madurez Tecnológica

> Herramienta web interactiva para evaluar el **Technology Readiness Level (TRL 1–9)** de creaciones intelectuales a partir de la información declarada en un *Disclosure de Identificación y Evaluación*.

[![Licencia: MIT](https://img.shields.io/badge/Licencia-MIT-blue.svg)](LICENSE)
[![HTML5](https://img.shields.io/badge/HTML5-Sin_dependencias_externas-orange.svg)](#)
[![TRL](https://img.shields.io/badge/Est%C3%A1ndar-NASA%2FComisi%C3%B3n_Europea-green.svg)](#)

---

## 📋 Tabla de Contenidos

1. [¿Qué es esta herramienta?](#qué-es)
2. [Flujo de usuario](#flujo)
3. [Arquitectura técnica](#arquitectura)
4. [Motor de reglas TRL](#motor-de-reglas)
5. [Preguntas por sector](#preguntas)
6. [Cómo ejecutar localmente](#local)
7. [Estructura de archivos](#estructura)
8. [Capturas de pantalla](#capturas)
9. [Licencia](#licencia)

---

## ¿Qué es esta herramienta? <a name="qué-es"></a>

Esta aplicación permite a inventores, evaluadores de propiedad intelectual y gestores de innovación obtener un **diagnóstico objetivo del nivel TRL** de una tecnología, basándose en el estándar NASA / Comisión Europea (TRL 1–9) y adaptado al instrumento *Formato de Identificación y Evaluación de Creaciones Intelectuales*.

### Características principales

| Característica | Detalle |
|---|---|
| **Sin dependencias** | 100% HTML + CSS + JS vanilla. Sin Node.js, sin bundler, sin backend |
| **Evaluación guiada** | Flujo de 5 pasos: carga → datos → matriz → diagnóstico → guía |
| **Multi-sector** | Preguntas adaptadas a Software, Hardware, IoT, Biotecnología y Química |
| **Motor de reglas** | Consolidación progresiva por umbral ≥60% desde TRL 1 |
| **Modo demo** | Caso real pre-cargado: sistema IoT de calidad vial (TRL 5) |
| **Exportación** | Impresión / exportación PDF via print nativo del navegador |
| **Responsive** | Funciona en móvil (375px) y escritorio (1280px+) |
| **Modo oscuro** | Detecta preferencia del sistema |

---

## Flujo de usuario <a name="flujo"></a>

```
┌─────────────────────────────────────────────────────────────┐
│  PASO 1: Cargar Disclosure                                   │
│  → El usuario sube PDF/DOCX del disclosure, o usa el demo   │
└──────────────────────┬──────────────────────────────────────┘
                       ▼
┌─────────────────────────────────────────────────────────────┐
│  PASO 2: Datos Generales                                     │
│  → Nombre, sector tecnológico, tipo, inventores, prototipo  │
│  → El sector activa las preguntas sectoriales relevantes    │
└──────────────────────┬──────────────────────────────────────┘
                       ▼
┌─────────────────────────────────────────────────────────────┐
│  PASO 3: Matriz de Evaluación TRL                           │
│  → 70 preguntas organizadas en TRL 1–9                      │
│  → Respuesta: ✅ Sí / ✗ No / ⟳ En ejecución               │
│  → Campo de evidencia por cada pregunta                     │
└──────────────────────┬──────────────────────────────────────┘
                       ▼
┌─────────────────────────────────────────────────────────────┐
│  PASO 4: Diagnóstico                                        │
│  → TRL consolidado + comparación con TRL auto-declarado     │
│  → Tabla de cumplimiento por nivel con barras de progreso   │
│  → Lista de brechas y acciones para avanzar al nivel        │
└──────────────────────┬──────────────────────────────────────┘
                       ▼
┌─────────────────────────────────────────────────────────────┐
│  PASO 5: Guía TRL (referencia siempre disponible)          │
│  → Descripción de los 9 niveles con criterios orientadores  │
└─────────────────────────────────────────────────────────────┘
```

---

## Arquitectura técnica <a name="arquitectura"></a>

La aplicación es un archivo HTML autocontenido que integra HTML, CSS y JavaScript sin dependencias externas de runtime (solo Google Fonts como CDN de tipografía).

```
trl-diagnostico.html
│
├── 📄 HTML  — Estructura semántica de 5 secciones (pasos)
│   ├── <header>       → Branding y badge institucional
│   ├── <nav>          → Navegación por pasos (sticky)
│   ├── #sec-0         → Paso 1: Carga de archivo
│   ├── #sec-1         → Paso 2: Formulario de datos generales
│   ├── #sec-2         → Paso 3: Matriz TRL (generada dinámicamente)
│   ├── #sec-3         → Paso 4: Resultado del diagnóstico
│   └── #sec-4         → Paso 5: Guía TRL
│
├── 🎨 CSS  — Sistema de diseño en variables CSS (:root)
│   ├── Paleta institucional (azul #003366, dorado #C8A951)
│   ├── 9 colores de gradiente para los niveles TRL 1–9
│   ├── Componentes: cards, formularios, badges, barras de progreso
│   └── Responsive: media queries en 640px y print
│
└── ⚙️ JavaScript  — Lógica de aplicación (vanilla, sin frameworks)
    ├── TRL_LEVELS[]   → Definición de los 9 niveles
    ├── PREGUNTAS[]    → 70 preguntas con sector y TRL asignado
    ├── state{}        → Estado mutable: respuestas y evidencias
    ├── buildMatrix()  → Genera la matriz filtrada por sector
    ├── setAnswer()    → Captura respuesta y actualiza indicadores
    ├── calcDiagnostico() → Motor de reglas TRL
    ├── buildGuia()    → Construye la guía de referencia
    └── loadDemo()     → Carga el caso de ejemplo IoT vial
```

### Modelo de datos

```javascript
// Pregunta
{ trl: 5, sector: 'software', txt: '¿El software ha sido desplegado...?' }

// Estado de respuestas
state.answers['5_31'] = 'si'   // TRL 5, pregunta global índice 31
state.evidences['5_31'] = 'Prueba en entorno staging con 3 usuarios'

// Resultado por nivel
{ n: 5, total: 6, si: 5, no: 1, ej: 0, pctSi: 83, consolidated: true }
```

---

## Motor de reglas TRL <a name="motor-de-reglas"></a>

El diagnóstico aplica un algoritmo de **consolidación progresiva** coherente con la naturaleza acumulativa de la escala TRL:

```
PARA cada nivel TRL n (de 1 a 9):
  preguntas_aplicables = preguntas donde sector ∈ sectores_activos Y trl = n
  pct_cumplimiento = (respuestas_SI / total_aplicables) × 100

  SI pct_cumplimiento ≥ 60%:
    nivel n → CONSOLIDADO
    TRL_final = n
  SI NO:
    DETENER (no se evalúan niveles superiores)

RESULTADO: TRL_final = último nivel consolidado en la cadena consecutiva
```

### Criterio de umbral: ≥ 60%

El umbral del 60% permite que un nivel esté mayoritariamente demostrado sin exigir el 100%, reconociendo que algunos criterios sectoriales pueden ser genuinamente no aplicables o estar en ejecución. Respuestas "En ejecución" **no cuentan** como cumplidas para el cálculo.

### Regla de degradación

Si el nivel *n* no alcanza el umbral, el TRL consolidado es *n-1*, independientemente del desempeño en niveles superiores. Esto es fiel al estándar: no se puede afirmar TRL 6 si TRL 5 no está plenamente demostrado.

### Comparación con TRL auto-declarado

Cuando el evaluador ingresa el TRL auto-declarado en el disclosure, el sistema calcula la diferencia:

| Resultado | Interpretación |
|---|---|
| Diagnosticado = Declarado | ✅ Coherente — evidencia respaldada |
| Diagnosticado > Declarado | 📈 Potencial subvaloración — revisar evidencias no documentadas |
| Diagnosticado < Declarado | ⚠️ Brecha — evidencia insuficiente para el TRL declarado |

---

## Preguntas por sector <a name="preguntas"></a>

La matriz contiene **70 preguntas** distribuidas en 9 niveles TRL y 5 categorías sectoriales:

| Sector | Código | Descripción |
|---|---|---|
| General | `general` | Criterios transversales, aplica a toda tecnología |
| Software / TI | `software` | Arquitectura, algoritmos, despliegue, ciclo de vida |
| Hardware / Dispositivos | `hardware` | Fabricación, integración, calificación física |
| Software + Hardware (IoT) | `software,hardware` | Ambos tracks activados simultáneamente |
| Biotecnología | `biotecnologia` | In vitro/vivo, biorreactor, regulatorio |
| Química / Materiales | `quimica` | Síntesis, escalado, caracterización |

Distribución por nivel:

| TRL | Preguntas generales | Preguntas sectoriales | Total (IoT) |
|---|---|---|---|
| 1 | 3 | 4 SW + 2 HW + 1 BIO + 1 QCA | 9 |
| 2 | 3 | 2 SW + 2 HW + 1 BIO + 1 QCA | 7 |
| 3 | 3 | 2 SW + 2 HW + 1 BIO + 1 QCA | 7 |
| 4 | 3 | 2 SW + 2 HW + 1 BIO + 1 QCA | 7 |
| 5 | 3 | 2 SW + 2 HW + 1 BIO + 1 QCA | 7 |
| 6 | 3 | 2 SW + 1 HW + 1 BIO + 1 QCA | 6 |
| 7 | 3 | 1 SW + 1 HW | 5 |
| 8 | 3 | 1 SW + 1 HW | 5 |
| 9 | 3 | 1 SW + 1 HW | 5 |

---

## Cómo ejecutar localmente <a name="local"></a>

No requiere instalación. Solo descarga y abre:

```bash
# Clonar el repositorio
git clone https://github.com/juandalfonso/trl-diagnostico.git
cd trl-diagnostico/trl-diagnostico

# Abrir en el navegador (cualquiera de estas opciones)
open trl-diagnostico.html               # macOS
start trl-diagnostico.html              # Windows
xdg-open trl-diagnostico.html          # Linux

# O servir con Python (opcional, para evitar restricciones de file://)
python3 -m http.server 8080
# Luego abrir http://localhost:8080/trl-diagnostico.html
```

> **Nota:** La carga de archivos PDF/DOCX usa la API `FileReader` del navegador. El archivo **nunca sale del dispositivo** — todo el procesamiento es local.

---

## Estructura de archivos <a name="estructura"></a>

```
trl-diagnostico/
├── README.md                    ← Esta documentación
└── trl-diagnostico/
    └── trl-diagnostico.html     ← Aplicación completa (autocontenida)
```

---

## Caso de prueba — Demo IoT Vial <a name="demo"></a>

El modo demo pre-carga el caso real descrito en el disclosure de ejemplo:

- **Tecnología:** Sistema de medición de calidad vial mediante IoT móvil
- **Inventores:** Daniel Jaramillo Ramírez, Edwar Jacinto Gomez, Fredy Hernán Martínez Sarmiento
- **Institución:** Universidad Distrital Francisco José de Caldas
- **TRL auto-declarado:** 5
- **TRL diagnosticado por la herramienta:** 5 ✅ Coherente
- **Evidencias:** Artículo IEEE Latin America Transactions, prototipo Android, servidor con BD, prueba piloto en Bogotá

---

## Licencia <a name="licencia"></a>

MIT License — libre uso, modificación y distribución con atribución.

---

*Herramienta desarrollada como solución a medida para evaluación de creaciones intelectuales · Estándar TRL basado en NASA Technology Readiness Level y Comisión Europea Horizon.*
