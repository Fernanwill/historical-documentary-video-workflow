# Workflow de Video: El Imperio Económico (Netflix Style Documentary)

Sistema automatizado y modular para la producción de videos documentales de alto impacto para el canal de YouTube **"El Imperio Económico"**, con estética cinematográfica estilo Netflix en español, enfocado en corporaciones, magnates, monopolios, finanzas y escándalos históricos.

Diseñado para operar mediante agentes de automatización de navegador en **[VideoExpress](https://app.videoexpress.ai/)** y plataformas de generación de voz como CloneVoice / ElevenLabs / VideoExpress Audio.

---

## 📁 Nueva Jerarquía del Proyecto

```text
├── README.md                      # Documentación y guía de uso
├── SYSTEM_PROMPT.md               # Prompt maestro de producción para el agente de automatización
├── styles/                        # Catálogo de estilos visuales cinematográficos
│   ├── cinematic-netflix.md       # Drama financiero moderno/clásico (iluminación claroscuro, 8k, anamórfico)
│   ├── vintage-archival.md        # Metraje de archivo 35mm (Wall Street histórico, magnates, 1870-1970)
│   ├── historic-engraving.md      # Grabado y litografía bancaria (burbujas clásicas, papel moneda)
│   └── dark-corporate.md          # Estética corporativa contemporánea oscura (Silicon Valley, fondos, crisis)
├── formats/                       # Parámetros por formato de video y plataforma
│   ├── landscape-16-9.md          # Horizontal 16:9 (YouTube Estándar / TV)
│   └── vertical-9-16.md           # Vertical 9:16 (YouTube Shorts / TikTok / Reels)
└── templates/                     # Plantillas de estructura y guiones en español
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
| **4. Formato / Ratio** | • `Landscape (16:9)` (YouTube Estándar)<br>• `Vertical (9:16)` (Shorts, Reels, TikTok) | `Landscape (16:9)` |
| **5. Duración** | • `60 segundos` (Shorts / cápsulas rápidas)<br>• `120 segundos` (Mini-documental)<br>• `Hasta 300 segundos (5 min)` (Documental extendido) | `120 segundos` |

---

## 🎬 Flujo de Trabajo en VideoExpress

1. **Ingreso a la Plataforma:** El agente accede a [VideoExpress](https://app.videoexpress.ai/) mediante el navegador visible y abre `Create with AI → Create Video From Prompt`.
2. **Generación de Keyframes:** A partir de los bloques de estilo en `styles/`, genera y aprueba las imágenes base para cada escena con *Creative Mode*.
3. **Generación Multi-Shot:** Redacta prompts multi-toma (`SHOT 1 / SHOT 2 / SHOT 3`) con acciones tangibles (firmas de acuerdos, parquets bursátiles en pánico, maquinaria industrial, despachos con lluvia).
4. **Voz y Audio en Español:** Integra la narración en español neutro (voz seria, profunda y misteriosa).
5. **Montaje en Línea de Tiempo:**
   - Alinea los clips de video en la pista superior y el audio en la inferior.
   - Silencia el canal de audio del metraje de video para garantizar máxima claridad en la narración.
   - Posiciona el cabezal de reproducción en el final exacto del audio y corta (**Cut**) el video sobrante para una sincronización perfecta.
6. **Exportación:** Guarda el proyecto y pulsa **Export Video**.

---

## 💡 Cómo Usar este Repositorio

1. Carga `SYSTEM_PROMPT.md` en tu agente de automatización de navegador.
2. Responde las 5 preguntas de configuración indicando el tema del episodio de **El Imperio Económico**.
3. El agente orquestará la creación en VideoExpress y te notificará cuando el video esté listo y exportado en la plataforma.
