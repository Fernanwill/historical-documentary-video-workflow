# Protocolo de Integración de Voz Externa (CloneVoice AI) en VideoExpress

Este documento define el flujo de trabajo estándar cuando la locución del documental para **"El Imperio Económico"** no se genera mediante el sintetizador integrado de VideoExpress, sino a través de un modelo de clonación de voz profesional externo (**CloneVoice AI**).

---

## 1. Justificación y Ventajas del Modelo Híbrido

- **Consistencia de Marca:** Mantiene la misma voz icónica del canal en todos los documentales, sin variaciones de acento o tesitura.
- **Calidad de Actuación:** Permite modular pausas dramáticas, silencios tensos y cambios de entonación documental difíciles de lograr con TTS básicos.
- **Ahorro de API:** No requiere credenciales ni suscripciones API complejas en VideoExpress; se utiliza la interfaz web estándar (Plan Básico / All Access).

---

## 2. Preparación y Exportación del Audio en CloneVoice AI

1. **Selección del Guión:** Tomar los bloques de narración cronometrados en:
   - `scripts/rockefeller-documentary/parte-1-ascenso-y-monopolio-5min.md`
   - `scripts/rockefeller-documentary/parte-2-guerra-juicio-y-legado-5min.md`
2. **Configuración de la Voz:**
   - **Tono:** Grave, analítico, pausado y con autoridad periodística.
   - **Ritmo de lectura:** 130 a 140 palabras por minuto.
   - **Idioma:** Español Neutro / Latinoamericano.
3. **Formato de Exportación:**
   - Archivo: `.mp3` (320 kbps) o `.wav` (48 kHz / 24-bit).
   - Nomenclatura recomendada:
     - `Imperio_Rockefeller_Parte1_CloneVoice.mp3`
     - `Imperio_Rockefeller_Parte2_CloneVoice.mp3`

---

## 3. Subida del Audio a VideoExpress

El usuario (o el agente vía interfaz) sube el audio a VideoExpress siguiendo estos pasos:

1. Abrir VideoExpress (`https://app.videoexpress.ai/`).
2. En la barra lateral derecha, hacer clic en el botón **`Import Media / Text to Speech`** (`.button-open-panel-import`).
3. Hacer clic en la tarjeta **`Upload Media`**.
4. Seleccionar el archivo `Imperio_Rockefeller_Parte1_CloneVoice.mp3`.
5. Una vez terminada la carga, VideoExpress almacena automáticamente el archivo en:
   - **Pestaña:** `Media Library` (`.button-open-panel-medialibrary`)
   - **Subcarpeta:** `My Media -> Audio` (ID de carpeta en dataset: `72280`).

---

## 4. Protocolo de Automatización del Agente (CDP)

Una vez que el usuario sube el audio a la plataforma, el agente ejecuta las siguientes tareas de manera 100% autónoma:

1. **Localizar el Audio Subido:**
   ```javascript
   // Navegar a My Media -> Audio (ID 72280)
   const audioItem = window.$('.library-item[data-type="audio"]').filter(function() {
     return $(this).text().includes('Rockefeller') || $(this).text().includes('CloneVoice');
   });
   ```
2. **Insertar el Audio en la Pista 2 del Timeline (`#track32`):**
   - Ejecutar el droppable programático en offset `0px`:
     ```javascript
     const $t2 = window.$('#track32');
     const droppable = $t2.data('ui-droppable');
     droppable.options.drop.call(droppable, {}, {
       draggable: audioItem,
       offset: { left: 0 }
     });
     ```
3. **Medir la Duración Real del Audio:**
   - Leer `window.$('#track32 .brick.audio').outerWidth()`.
   - Ejemplo: ~6000px para 5 minutos (~300 segundos).
4. **Generar los Clips de Video en VideoExpress (`Create with AI`):**
   - Generar cada una de las 8 escenas descritas en el guión correspondiente (10s por clip, 16:9, B-roll sin sonido).
   - Guardar cada imagen clave y clip de video en la biblioteca.
5. **Ensamblar la Pista 1 de Video (`#track30`):**
   - Insertar los clips de video secuencialmente en el orden cronológico del guión hasta cubrir exactamente la duración total de la pista de audio.
6. **Guardar y Exportar:**
   - Guardar el proyecto con el nombre canónico (ej. `Imperio_Rockefeller_Parte1_FullHD`).
   - Enviar a renderizar en formato `FullHD mp4` con calidad `High`.
