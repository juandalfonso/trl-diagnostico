# 🔬 TRL Diagnóstico — Herramienta de Madurez Tecnológica

**Herramienta web interactiva, 100% offline, para diagnosticar el nivel TRL (Technology Readiness Level) de una tecnología a partir de su Disclosure de creación intelectual.**

---

## ✨ ¿Qué hace?

1. **Carga el Disclosure** (PDF o DOCX) — el archivo **nunca sale del computador** del usuario. Todo corre en el navegador.
2. **Extrae automáticamente** la información de identificación de la tecnología: título, inventores, institución, sector, tipo de creación, TRL declarado, estado del prototipo, evidencias disponibles, descripción del problema y solución.
3. **Prellenado con marca visual** — cada campo auto-completado queda resaltado en amarillo con etiqueta `AUTO` para que el evaluador lo verifique.
4. **Matriz TRL interactiva** — el evaluador diligencia Sí / No / En ejecución por cada criterio, organizado por niveles TRL 1–9 y por sector tecnológico (general + software + hardware/IoT + biotech + química).
5. **Diagnóstico con contraste** — el sistema calcula el TRL consolidado y lo compara contra el TRL auto-declarado en el Disclosure, mostrando brechas, criterios faltantes y recomendaciones para avanzar.

---

## 📁 Estructura del Repositorio

```
trl-diagnostico/
├── README.md                          ← Este archivo
├── index.html                         ← Redirección de raíz
└── trl-diagnostico/
    └── trl-diagnostico.html           ← Aplicación completa (autocontenida)
```

---

## 🚀 Cómo usar

### Opción A — Directamente desde el repositorio
1. Descarga `trl-diagnostico/trl-diagnostico.html`
2. Ábrelo con doble clic en Chrome, Edge o Firefox
3. ¡Listo!

### Opción B — GitHub Pages *(si está habilitado)*
Accede a: `https://juandalfonso.github.io/trl-diagnostico/`

---

## 🔒 Privacidad y seguridad

- **El archivo PDF/DOCX nunca se transmite a ningún servidor.** Toda la lectura, extracción y análisis ocurre localmente en el navegador.
- No hay backend, no hay base de datos, no hay cookies.
- Las únicas conexiones externas son la carga de fuentes tipográficas (Google Fonts) y las librerías CDN (pdf.js, mammoth.js) — ambas solo en la primera carga.

---

## 🧰 Tecnologías utilizadas

| Componente | Tecnología |
|---|---|
| Lectura de PDF | [pdf.js 3.11](https://mozilla.github.io/pdf.js/) |
| Lectura de DOCX | [mammoth.js 1.6](https://github.com/mwilliamson/mammoth.js) |
| Interfaz | HTML5 + CSS3 + Vanilla JavaScript |
| Tipografía | Inter (Google Fonts) |
| Despliegue | Archivo HTML único autocontenido |

---

## 📋 Flujo de evaluación

```
Cargar Disclosure (PDF/DOCX)
        │
        ▼
Extracción automática de campos
(título, inventores, TRL declarado, evidencias...)
        │
        ▼
Revisar y completar Identificación de la Tecnología
        │
        ▼
Diligenciar Matriz TRL por nivel y sector
(Sí / No / En ejecución + evidencia textual)
        │
        ▼
Calcular diagnóstico
        │
        ▼
Contraste: TRL calculado vs TRL declarado
+ Brechas por nivel + Recomendaciones
```

---

## 📊 Niveles TRL cubiertos

| TRL | Descripción |
|-----|-------------|
| TRL 1 | Principios básicos observados |
| TRL 2 | Concepto tecnológico formulado |
| TRL 3 | Prueba de concepto experimental |
| TRL 4 | Validación en laboratorio |
| TRL 5 | Validación en entorno relevante |
| TRL 6 | Demostración en entorno relevante |
| TRL 7 | Demostración en entorno operativo |
| TRL 8 | Sistema completo calificado |
| TRL 9 | Sistema probado en entorno operativo real |

---

## 🗂 Sectores tecnológicos soportados

- **General** (aplica a todas las tecnologías)
- **Software / Sistemas de Información**
- **Hardware / Electrónica / IoT**
- **Biotecnología / Ciencias de la Vida**
- **Química / Materiales**

---

## 📝 Changelog

### v2.0.0 — Abril 2026
- ✅ Lectura local de PDF (pdf.js) y DOCX (mammoth.js) — sin servidor
- ✅ Extracción automática de campos de identificación desde el texto del Disclosure
- ✅ Prellenado visual con marca `AUTO` para revisión del evaluador
- ✅ Preview de campos extraídos antes de avanzar al formulario
- ✅ Log de procesamiento en tiempo real
- ✅ Contraste TRL declarado vs TRL calculado en el diagnóstico
- ✅ Recomendaciones por nivel para avanzar al siguiente TRL

### v1.0.0 — Abril 2026
- ✅ Estructura base de la aplicación
- ✅ Formulario de Identificación de la Tecnología
- ✅ Matriz TRL interactiva por niveles y sectores
- ✅ Cálculo de TRL consolidado con reglas de umbral

---

## 👤 Autor

Desarrollado para el proceso de **gestión de creaciones intelectuales** y evaluación de madurez tecnológica.
