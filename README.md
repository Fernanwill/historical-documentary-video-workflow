# Workflow de Video: El Imperio Económico (Netflix Style Documentary)

Sistema automatizado y modular para la producción de videos documentales de alto impacto para el canal de YouTube **"El Imperio Económico"**, con estética cinematográfica estilo Netflix en español, enfocado en corporaciones, magnates, monopolios, finanzas y escándalos históricos.

Diseñado para operar mediante agentes de automatización de navegador en **[VideoExpress](https://app.videoexpress.ai/)** y plataformas de generación de voz como **CloneVoice AI** o el sintetizador TTS de VideoExpress.

---

## 📁 Estructura del Proyecto

```text
├── README.md                      # Documentación y guía de uso general
├── SYSTEM_PROMPT.md               # Prompt maestro de producción para el agente de automatización
├── docs/                          # Guías técnicas y arquitectura de automatización
│   └── videoexpress-automation-guide.md # Mapa de selectores, eventos Backbone/Marionette y timeline
├── scripts/                       # Guiones completos y paquetes de producción
│   └── rockefeller-documentary/   # Documental de formato largo (10 minutos en 2 partes de 5 min)
│       ├── README.md              # Visión general, estructura y notas de producción
│       ├── parte-1-ascenso-y-monopolio-5min.md # Parte 1 (00:00 - 05:00) con prompts sincronizados
│       └── parte-2-guerra-juicio-y-legado-5min.md # Parte 2 (05:00 - 10:00) con prompts sincronizados
├── styles/                        # Catálogo de estilos visuales cinematográficos
│   ├── cinematic-netflix.md       # Drama financiero moderno/clásico (claroscuro, 8k, anamórfico)
│   ├── vintage-archival.md        # Metraje de archivo 35mm (Wall Street histórico, magnates, 1870-1970)
│   ├── historic-engraving.md      # Grabado y litografía bancaria (burbujas clásicas, papel moneda)
│   └── dark-corporate.md          # Estética corporativa contemporánea oscura (fondos, crisis, trading)
├── formats/                       # Parámetros por formato de video y plataforma
│   ├── landscape-16-9.md          # Horizontal 16:9 (YouTube Estándar / Documental Largo)
│   └── vertical-9-16.md           # Vertical 9:16 (YouTube Shorts / TikTok / Reels)
└── templates/                     # Plantillas de estructura y guiones en español
    ├── external-audio-clonevoice.md # Protocolo de integración con CloneVoice AI (audio externo)
    ├── voiceover-only.md          # Estructura 100% voz en off inmersiva estilo Netflix
    ├── presenter-intro.md         # Estructura con Presentador a cámara (Mateo Silva) + narración
    └── script-examples-es.md      # Ejemplos de guiones reales listos para producción
```

---

## 🚀 Parámetros Configurables al Iniciar

Al ejecutar el workflow con tu agente, este te solicitará 5 decisiones clave en un solo mensaje:

| Parámetro | Opciones Disponibles | Valor Predeterminado |
|---|---|---|
| **1. Idea o Guion** | Tema histórico/financiero libre o guion propio completo | Requiere especificación |
| **2. Estilo Visual** | • `Cinematic Netflix / Drama Financiero`<br>• `Vintage Archival 35mm`<br>• `Grabado Histórico Bancario`<br>• `Dark Corporate 3D / Cyber Finanzas` | `Cinematic Netflix` |
| **3. Modo de Producción** | • `Solo Narrador en Off (Voiceover Inmersivo Netflix)`<br>• `Presentador a Cámara + Narración` | `Solo Narrador en Off` |
| **4. Formato / Ratio** | • `Landscape (16:9)` (YouTube Estándar / Documentales Largos)<br>• `Vertical (9:16)` (YouTube Shorts, Reels, TikTok) | `Landscape (16:9)` |
| **5. Duración** | • `60 segundos` (Shorts de alto impacto)<br>• `120 segundos` (Mini-documental)<br>• `Formato Largo: 10 min (2 partes de 5 min)` | `120 segundos` |

---

## 🎙️ Dos Modalidades de Audio

1. **Voz Clonada Externa (CloneVoice AI - Recomendada):**
   - El usuario genera la narración a partir de los guiones cronometrados con la voz oficial de su canal.
   - Sube el archivo (`.mp3` o `.wav`) en `Import Media` → `Upload Media` de VideoExpress.
   - El agente detecta el archivo en `My Media -> Audio` (ID `72280`), lo suelta en la Pista 2 (`#track32`) y monta los clips de video en la Pista 1 (`#track30`) ajustados a su duración exacta.
   - Ver `templates/external-audio-clonevoice.md`.
2. **TTS Integrado de VideoExpress:**
   - Selección de idioma `Spanish (Mexico)`.
   - Generación directa dentro de VideoExpress, guardado en `My Media -> My AI Audio` (ID `363795`).

---

## 🎬 Flujo de Trabajo en VideoExpress

1. **Conexión:** Chrome en modo depuración remota (`--remote-debugging-port=9222 --remote-allow-origins=*`).
2. **Generación de Keyframes:** Prompt visual con iluminación claroscuro, sujeto y vestimenta de época. Clic en **Save to Library** en la tarjeta de la imagen para que quede registrada.
3. **Generación Multi-Shot:** Activar **Advanced Mode**, desmarcar auto-enhancers, activar **Video Only (No Sound)** (B-roll sin ruido de fondo), duración 10s.
4. **Montaje en Línea de Tiempo:**
   - Pista 2 (`#track32`): Audio de la narración en español.
   - Pista 1 (`#track30`): Secuencia cronológica de clips de video de 10s.
5. **Guardado y Exportación:** Guardar el proyecto con nombre canónico (`.button-save-project-as`) y exportar en `FullHD mp4` (`.button-render-project`).

---

## 📚 Documentación de Referencia
- **[Guía de Automatización y Arquitectura Técnica de VideoExpress](docs/videoexpress-automation-guide.md)**: Selectores DOM, APIs Backbone/Marionette y manipulación programática de droppables.
- **[Documental de John D. Rockefeller (10 min en 2 partes de 5 min)](scripts/rockefeller-documentary/README.md)**:
  - [Parte 1: El Ascenso y el Monopolio](scripts/rockefeller-documentary/parte-1-ascenso-y-monopolio-5min.md)
  - [Parte 2: La Guerra, el Juicio y el Legado](scripts/rockefeller-documentary/parte-2-guerra-juicio-y-legado-5min.md)
- **[Protocolo de Audio Externo CloneVoice AI](templates/external-audio-clonevoice.md)**
