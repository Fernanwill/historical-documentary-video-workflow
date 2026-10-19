# Guía de Automatización y Arquitectura Técnica de VideoExpress.ai

Esta guía documenta la arquitectura técnica inversa, selectores DOM, eventos Backbone.js/Marionette.js, estructura de directorios y funciones clave de **VideoExpress 3.5 (`https://app.videoexpress.ai/`)** aprendidas durante la automatización de documentales para el canal **"El Imperio Económico"**.

---

## 1. Conexión y Protocolo Chrome DevTools (CDP)

### 1.1 Configuración de Inicio de Chrome
Para permitir que scripts de automatización interactúen con VideoExpress sin que Chrome muestre ventanas emergentes de confirmación repetidas en cada conexión:
```powershell
chrome.exe --remote-debugging-port=9222 --remote-allow-origins=*
```
El archivo de puerto activo y WebSocket se lee desde:
`$env:LOCALAPPDATA\Google\Chrome\User Data\DevToolsActivePort` (Línea 1 = puerto, Línea 2 = ruta WS).

---

## 2. Mapa de Selectores y Componentes DOM

### 2.1 Barra Superior y Proyectos
- **Nuevo Proyecto:** `.button-new-project` (`button[title="New"]`)
- **Abrir Proyecto:** `.button-open-project`
- **Guardar Proyecto:** `.button-save-project`
- **Menú Guardar Como:** `.button-save-project + .dropdown-toggle` -> `.button-save-project-as`
  - Abre modal con campo `input` y botón `Save` (`.modal button`).
- **Exportar Video:** `.button-render-project` (`button.btn-primary.button-render-project`)
  - Modal de exportación: Formato (`mp4`), Resolución (`FullHD`), Calidad (`High`), Nombre de archivo.
  - Botón de confirmación: `.button-submit` (`btn btn-raised btn-primary ladda-button button-submit`).
- **Mis Videos (Renders Finales):** `.button-open-movies` (`btn[title="My Videos"]`)

### 2.2 Relación de Aspecto (Canvas)
- **Horizontal 16:9 (Documental YouTube / TV):** Botón flotante superior izquierdo con texto `Landscape 16:9`.
- **Vertical 9:16 (YouTube Shorts / TikTok):** Botón flotante superior izquierdo con texto `Vertical 9:16`.

### 2.3 Barra Lateral Derecha (Navegación Principal)
- **Biblioteca de Medios:** `.button-open-panel-medialibrary` (`[text()="Media Library"]`)
- **Generador de IA:** `.button-generate-video` (`[text()="Create with AI"]`)
- **Importar Medios / Text to Speech:** `.button-open-panel-import` (`[text()="Import Media / Text to Speech"]`)
- **Animaciones de Texto:** `.button-open-panel-animation`
- **Filtros de Color / Estilo:** `.button-open-panel-filters`
- **Fast Cut:** `.button-open-panel-fast-cut-effect`
- **Subtítulos Automáticos:** `a[text()="Automatic Captions"]`

---

## 3. Generador de Contenido con IA (`Create with AI`)

### 3.1 Apertura y Opciones
1. Hacer clic en `.button-open-panel-filters` o cerrar paneles previos, luego `.button-generate-video`.
2. En la vista de bienvenida hacer clic en la tarjeta `Create Video with AI` (`.button-generate-from-prompt`).

### 3.2 Campos y Checkboxes Clave
| Función | Selector / Identificador | Valor Requerido |
| :--- | :--- | :--- |
| **Prompt de Imagen Clave** | `textarea[placeholder*="image"]` | Prompt cinematográfico detallado |
| **Creative Mode** | `#use_creative_mode` | `CHECKED` |
| **Auto-enhance Image Prompt** | Input checkbox con label "Automatically enhance" | **`UNCHECKED`** (conservar fidelidad de prompt) |
| **Public Gallery** | Checkbox "Share this in the public gallery" | **`UNCHECKED`** |
| **Advanced Mode** | Checkbox "Advanced Mode" | **`CHECKED`** (habilita multi-shot & control de cámara) |
| **Prompt Multi-Shot / Video** | `.textarea-video-prompt` | Movimiento de cámara, duración y detalles cinemáticos |
| **Auto-enhance Video Prompt** | Checkbox video enhance | **`UNCHECKED`** |
| **Video Only (No Sound)** | Checkbox video only | **`CHECKED`** (vital para B-roll documental sin ruidos) |
| **Duración de Clip** | Select / Dropdown de duración | `10s` (máxima continuidad para edición) |
| **Relación de Aspecto** | Radio / Botones en generador | `Landscape 16:9` o `Vertical 9:16` |

### 3.3 Flujo Crítico de Selección y Creación de Video
1. **Generar Imagen:** Clic en `.button-generate-image-submit`.
2. **Esperar y Persistir:** Aparece carrusel Swiper (`.swiper-slide`). Clic obligatorio en el botón flotante **`Save to Library` (`.button-save-image`)** en la tarjeta de la imagen para que se registre en la cuenta.
3. **Seleccionar para Video:** Hacer clic en la imagen dentro del carrusel para activarla (elimina la advertencia *"Please select or create an image first to proceed"*).
4. **Generar Video:** Clic en `.button-generate-video-submit`.
5. **Completado:** Aparece la notificación flotante: *"The generation of your AI Video has been successfully completed [Show me]"*.

---

## 4. Estructura de Carpetas en la Biblioteca de Medios (`My Media`)

La pestaña `My Media` organiza todos los recursos bajo identificadores de carpeta específicos:

| Carpeta | ID en Dataset | Contenido |
| :--- | :--- | :--- |
| **`My AI Videos`** | `72289` | Todos los clips de video generados con IA (identificadores `data-ident`). |
| **`My AI Audio`** | `363795` | Audios generados mediante el TTS nativo de VideoExpress. |
| **`Audio`** | `72280` | Audios subidos manualmente (ej. locuciones de **CloneVoice AI**). |
| **`My AI Images`** | `72292` | Todas las imágenes clave guardadas. |
| **`Videos`** | `72283` | Videos importados o de stock propio. |
| **`Images`** | `72286` | Archivos gráficos importados. |

> [!TIP]
> **Reinicio de Navegación de Carpetas:** Si la biblioteca se queda atascada dentro de una subcarpeta mostrando "Empty." con botón de retorno morado, hacer clic en `.button-open-panel-filters` y luego de vuelta en `.button-open-panel-medialibrary`. Esto restablece la vista al directorio raíz de `My Media`.

---

## 5. Arquitectura del Timeline y Drag-and-Drop Programático

VideoExpress implementa un timeline basado en jQuery UI Droppable con pistas numeradas:
- **Pista 1 (Video):** Elemento DOM `#track30` con clase `.track-row.ui-droppable`.
- **Pista 2 (Audio):** Elemento DOM `#track32` con clase `.track-row.ui-droppable`.

### 5.1 Función Interna de Droppable
El drop lee los datos del elemento arrastrado:
- `data-type`: `"video"`, `"audio"`, `"image"`.
- `data-ident`: ID del recurso en `mediaLibraryModule.itemsCollection`.
- Desencadena internamente:
  ```javascript
  App.vent.trigger("onMediaTimelineDropped", trackModel, mediaItemModel, offsetPixels);
  ```

### 5.2 Inserción Programática Directa (Sin Arrastre Físico)
Para insertar clips de video o pistas de audio en posiciones exactas:
```javascript
const $track = window.$('#track30'); // o #track32 para audio
const $item = window.$('[data-ident="ID_DEL_RECURSO"]');
const droppable = $track.data('ui-droppable');

// Calcular offset en píxeles:
const tracksContainer = window.$('#tracks-container');
const l = tracksContainer.scrollLeft();
const c = window.$('.left-panel').width();
const wrapper = window.$('.tracks-wrapper');
const n = wrapper.offset() ? wrapper.offset().left : 0;
const fakeOffsetLeft = targetPositionPx + n + c - l;

droppable.options.drop.call(droppable, {}, {
  draggable: $item,
  offset: { left: fakeOffsetLeft }
});
```

### 5.3 Escala Temporal y Unidades del Timeline
- Factor de escala interno: **`zoom = 0.02`** (equivale a 20 px/s o 0.02 px/ms).
- Unidades de modelo (`BrickItem`): `left` y `duration` están en **milisegundos**.
- Offset en píxeles para eventos de drop: `offsetPx = Math.floor(targetTimeMs * zoom)`.
- **10 segundos de video:** 10,041 ms (~201 px).
- **60 segundos:** 60,000 ms (1,200 px).
- **5 minutos (Parte 1):** ~304,535 ms (6,091 px).

### 5.4 Arquitectura Interna Nativa (Backbone.js / Marionette.js)
El editor expone la instancia global `window.app` con acceso directo a colecciones y eventos sin depender de emulación física de UI:
- **Pistas del Timeline:** `window.app.timelineModule.trackItemsCollection`
  - Pista 0 (Video): `window.app.timelineModule.trackItemsCollection.at(0)`
  - Pista 1 (Audio): `window.app.timelineModule.trackItemsCollection.at(1)`
- **Elementos en Timeline (Bricks):** `window.app.timelineModule.brickItemsCollection`
- **Inserción Directa de Medios:**
  ```javascript
  const trackModel = window.app.timelineModule.trackItemsCollection.at(0); // Video
  const mediaData = await window.$.ajax({ url: '/library/get_media_item/' + mediaId, type: 'GET' });
  const itemModel = new window.app.mediaItemsModule.Item(mediaData);
  const offsetPx = Math.floor(targetTimeMs * window.app.project.get("zoom"));
  window.app.vent.trigger("onMediaTimelineDropped", trackModel, itemModel, offsetPx);
  ```
- **Exportación Directa a Cola de Render (FullHD mp4):**
  ```javascript
  const n = window.$("#formExportProject").serializeObject() || {
    name: "Nombre_Proyecto",
    quality: "high",
    size: "1080",
    format: "mp4"
  };
  n.aspect = window.app.project.get("aspect") || "16:9";
  n.project_id = window.app.project.get("id");
  n.project_title = window.app.project.get("title");
  
  const payload = {
    options: n,
    data: window.app.getProjectData().data
  };
  
  window.$.ajax({
    type: "POST",
    url: window.app.options.baseUrl + "render_project/" + (n.project_id || "tmp"),
    dataType: "json",
    contentType: "application/json",
    data: JSON.stringify(payload)
  });
  ```

---

## 6. Flujo de Trabajo Híbrido: Audio Externo (CloneVoice AI) + VideoExpress

Cuando la locución no se genera con el TTS integrado de VideoExpress sino con una voz clonada de alta calidad (ej. **CloneVoice AI**):

1. **Generación Externa:**
   - Se toma el guión cronometrado (ej. 5 minutos por parte).
   - Se genera el archivo de audio en CloneVoice AI (`.mp3` o `.wav` a 320 kbps / 48 kHz).
2. **Subida a VideoExpress:**
   - El usuario o agente abre `Import Media / Text to Speech` -> Clic en tarjeta `Upload Media`.
   - Se sube el archivo de audio, el cual se aloja automáticamente en `My Media -> Audio` (ID `72280`).
3. **Ensamblaje Automatizado por el Agente:**
   - El agente detecta los audios en la carpeta `Audio` (ID `72280`).
   - Los inserta ordenadamente en la **Pista de Audio (`trackItemsCollection.at(1)`)**.
   - Genera los clips visuales de B-roll (10s Multi-Shot Cinematic Netflix, Silent/Video Only) en VideoExpress.
   - Ensambla y ajusta la duración de los clips en la **Pista de Video (`trackItemsCollection.at(0)`)** sincronizados con los cortes de la historia.
   - Guarda el proyecto (`Rockefeller_Parte_1_5min`) y lanza la exportación FullHD en la cola de renderizado.

