# 🎬 RV Player

Reproductor multimedia para Android (Kotlin + Jetpack Compose) con estética oscura
Material 3, motor **VLC/LibVLC**, catálogo de YouTube, visor de imágenes y descargas
con **yt-dlp**.

![Android](https://img.shields.io/badge/Android-7.0%2B-3DDC84?logo=android&logoColor=white)
![Kotlin](https://img.shields.io/badge/Kotlin-2.x-7F52FF?logo=kotlin&logoColor=white)
![Jetpack Compose](https://img.shields.io/badge/Jetpack%20Compose-Material%203-4285F4?logo=jetpackcompose&logoColor=white)
![minSdk](https://img.shields.io/badge/minSdk-24-00D9DB)

<p align="center">
  <img src="logo.png" alt="RV Player" width="220">
</p>

RV Player reúne en una sola app la biblioteca local (vídeo, música, carpetas, artistas
y álbumes), la galería de fotos, YouTube y un gestor de descargas, todo con
reproducción mediante LibVLC y una interfaz inspirada en SimpMusic, Gallery y OpenTune.

## Funcionalidades

### 🎬 Biblioteca unificada
Una sola pantalla al estilo de los reproductores modernos, con pestañas en forma de
píldora: **Videos · Canciones · Playlists · Carpetas · Artistas · Álbumes**.
- **Videos** con miniatura real extraída del propio archivo, duración y resolución.
- **Selección múltiple** (mantener pulsado) para eliminar varios vídeos o canciones.
- **Canciones** en lista plana con carátula.
- **Carpetas / Artistas / Álbumes** con tarjetas y vista previa de canciones.
- Mini reproductor flotante que puede ocultarse tocando el logo/título de cualquier sección.

### 🎬 Reproductor de vídeo
- **LibVLC** (MKV, MP4, AVI, WebM, TS, FLV…).
- Play/Pausa, ±10 s, anterior/siguiente y barra de progreso.
- **Botón para rotar** la pantalla.
- **Deslizar en la mitad izquierda** = brillo; **mitad derecha** = volumen,
  con indicador en pantalla.
- Al salir de la pantalla, el vídeo **se detiene**.

### 🎵 Reproductor de audio
- Play/Pausa, anterior/siguiente y **retroceder/adelantar 10 s**.
- Aleatorio, repetir y repetir una.
- **Letras** vía LRCLib.
- **Ecualizador** con preajustes aplicados a VLC.
- **Temporizador de sueño**.
- Notificación con controles y MediaSession (pantalla de bloqueo).

### 🖼️ Galería
- Estilo **Material 3** inspirado en la app Gallery: barra con logo, pestañas Fotos / Álbumes / Favoritos.
- **Fotos** en cuadrícula de 4 columnas; **Álbumes** en tarjetas de 2 columnas con portada y conteo.
- Visor con zoom, compartir, información y eliminar; al borrar pasa a la imagen siguiente.

### ▶️ YouTube (estilo OpenTune)
- Búsqueda con filtros **Todo / Canciones / Vídeos / Álbumes / Artistas / Playlists** y resultados por secciones.
- **Música** con categorías: Éxitos 2026, Pop, Rock, Electro, Trance y Clásicos.
- Interruptor **«Seguir con la siguiente»** para decidir si continúa la lista o solo suena la canción pulsada.
- **Playlists y álbumes** con ficha propia (portada, autor, canciones y Reproducir).
- **Artistas**: al pulsarlos se muestran sus canciones.
- Miniaturas directas de `i.ytimg.com`.
- Reproducción mediante **NewPipeExtractor / yt-dlp** (más fiable que las APIs públicas).
- Menú **⋮** en cada fila: reproducir, descargar audio MP3 y favorito.

### ⬇️ Descargas con yt-dlp
- Pega la URL → elige **Audio (MP3)** o **Vídeo (MP4)**.
- Compatible con YouTube, TikTok, Instagram, Facebook, X y cientos de sitios.
- Progreso, velocidad, cancelar, reintentar, abrir y compartir.
- Botón **Actualizar** para actualizar el binario de yt-dlp.
- Audio en `Descargas/RVPlayer/audio` y vídeo en `Descargas/RVPlayer/video`.

### ⚙️ Ajustes
- Ocultar archivos ocultos y notas de voz (WhatsApp/Telegram).
- Notificaciones, batería y acceso a archivos.
- Actualizar/borrar biblioteca.

## Arquitectura

```
app/src/main/java/com/rvplayer/
├── core/
│   ├── playback/     PlaybackManager, VlcEngine, Playable
│   ├── download/     YtDlpEngine (yt-dlp + Python + FFmpeg)
│   └── util/         FileProviderUtils
├── data/
│   ├── model/        Room: MediaItem (path único), Download, Playlist…
│   ├── database/     AppDatabase v2, DAOs
│   └── repository/   Media, Playlist, Preferences, Lyrics, YouTube
└── ui/
    ├── navigation/   MainActivity + mini reproductor
    ├── library/      Biblioteca con pestañas
    ├── player/       Reproductor audio/vídeo, diálogos y servicio
    ├── gallery/      Fotos / Álbumes / Favoritos
    ├── youtube/      Catálogo y streaming
    ├── download/     Pantalla y servicio de descargas
    ├── settings/     Ajustes
    ├── components/   BottomBar, MiniPlayer…
    └── theme/        Paleta y tipografía
```

## Compilar

Requisitos: **JDK 17** y Android SDK 34 (platform + build-tools).

```bash
gradlew.bat assembleDebug     # Windows (desarrollo)
gradlew.bat assembleRelease   # Windows (producción, firmado)
./gradlew assembleRelease     # Linux / macOS
```

APK de desarrollo: `app/build/outputs/apk/debug/app-debug.apk`.
APK release firmado: `app/build/outputs/apk/release/app-release.apk`.

## Instalación

1. Descarga el APK release (`RVPlayer-*.apk`).
2. Activa «Instalar apps desconocidas» para tu explorador de archivos.
3. Instala el APK. Si tenías una versión **debug**, desinstálala primero:
   ```bash
   adb uninstall com.rvplayer
   ```
4. Al abrir, concede:
   - **Acceso a todos los archivos** (Android 11+) para descargar y borrar multimedia.
   - **Notificaciones** (Android 13+) para los controles del reproductor y las descargas.

> El release y el debug usan firmas distintas. Para actualizar entre versiones release
> conserva siempre el mismo keystore (`rvplayer-release.jks`).

## Novedades 2.1.1

- **Abrir desde el explorador de archivos**: se detecta el tipo por MIME, tipo del proveedor o extensión; se admiten también envíos por «Compartir» (`ACTION_SEND`) y, si el proveedor no expone ruta ni descriptor, el archivo se copia a la caché antes de reproducirlo. Se muestra un aviso con el nombre del archivo al abrirlo.
- **YouTube**: las portadas de playlists sin miniatura muestran un **🎵** en lugar de un cuadro vacío/negro.
- **Readme.md** preparado para GitHub.

## Novedades 2.1.0

- **Galería con zoom de cuadrícula**: pellizca con dos dedos sobre las fotos para cambiar el tamaño de las miniaturas (de 2 a 6 por fila). Por defecto se muestran **3 por fila**, más grandes que antes, y la elección se guarda.
- **Compilación release firmada**: APK de producción con R8, reducción de recursos y firma propia.

### Firma release

El APK release se firma con `rvplayer-release.jks` (alias `rvplayer`). Las contraseñas están en `app/build.gradle.kts`. Guarda ese archivo para poder publicar actualizaciones con la misma firma. Al instalar el release sobre un APK debug hay que desinstalar antes (`adb uninstall com.rvplayer`), porque las firmas son distintas.

## Novedades 2.0.1

- **Descargas**: al terminar, la notificación de progreso se sustituye por la de **«Descarga completada — 100 %»**; ya no queda la barra de descarga abierta.
- **Vídeo en segundo plano**: al minimizar la app el vídeo **sigue sonando** (se desactiva solo la salida de imagen) y, al volver, se recupera la imagen en la misma posición.
- **Música → vídeo**: al abrir un vídeo después de reproducir audio se fuerza una nueva salida de vídeo, evitando que quede en negro con solo sonido.

## Novedades 2.0.0

- **Módulo YouTube rehecho al estilo OpenTune**:
  - Buscador con filtros **Todo / Canciones / Vídeos / Álbumes / Artistas / Playlists**.
  - Resultados agrupados por tipo con encabezados, como en OpenTune.
  - Filas con miniatura, canal, visitas, duración y menú **⋮** (reproducir, descargar audio, favorito).
  - **Álbumes y playlists** se abren en una ficha con portada, autor, número de canciones y botón Reproducir.
  - **Artistas** abren directamente sus canciones.
  - Inicio con pestañas Música / Playlists / Favoritos, categorías por género y el interruptor de reproducción continua.
- **Vídeo: desincronización y pantalla negra**:
  - Eliminadas las opciones de VLC que impedían descartar fotogramas (`--no-drop-late-frames`, `--no-skip-frames`), causantes del desfase audio/vídeo.
  - Al minimizar y volver se recrea el decodificador de vídeo en la posición guardada, en lugar de reutilizar una superficie destruida. Esto corrige la pantalla negra y los tirones al reanudar.

## Novedades 1.9.2

- **Notificación**: los botones de reproducir/pausar ahora son acciones independientes; además la notificación solo se reconstruye cuando cambia el estado, no cada segundo por la posición, lo que provocaba que los botones fallaran.
- **Vídeo al minimizar**: al volver a la app se desengancha y vuelve a enganchar la superficie de vídeo, evitando la pantalla negra con audio de fondo.

## Novedades 1.9.1

- **YouTube**: las pestañas Música / Playlists / Favoritos ocupan el ancho completo, repartidas en partes iguales.
- **Abrir con RV Player**: los archivos recibidos desde Descargas u otras apps se abren por descriptor (`AssetFileDescriptor`) o por ruta real, lo que corrige que el reproductor se abriera sin cargar el audio o el vídeo.
- **Visualizador de imágenes**: RV Player se registra para abrir imágenes (`image/*`) desde otras apps; si la imagen pertenece a la galería local se abre con todas sus vecinas para deslizar.
- **YouTube → Música**: interruptor **«Seguir con la siguiente»** antes de reproducir; si está activo continúa con la lista del género elegido, si está apagado reproduce solo la canción seleccionada. La elección se guarda.

## Novedades 1.9.0

- **Galería rediseñada** siguiendo el proyecto de referencia: barra superior con logo y acceso a Ajustes, pestañas Material 3 **Fotos / Álbumes / Favoritos**, cuadrícula de fotos de 4 columnas y álbumes en tarjetas de 2 columnas con portada y número de elementos.
- **Eliminar mientras se ve una imagen**: ahora pasa a la siguiente (o a la anterior si era la última) sin cerrar el visor.
- **Abrir descargas con RV Player**: se añadió el manejo de `ACTION_VIEW`; si eliges RV Player para abrir un vídeo o audio, se reproduce. El botón Abrir de Descargas usa el tipo correcto (audio o vídeo).
- **YouTube**: eliminada la pestaña **En vivo**.
- **Ajustes**: eliminada la sección de **Estadísticas**.
- **Reproductor**: resplandor ambiental Material 3 detrás de la carátula.

## Novedades 1.8.0

- **Descargas organizadas**: el audio va a `Descargas/RVPlayer/audio` y el vídeo a `Descargas/RVPlayer/video`.
- **Extraer audio de un vídeo**: menú de 3 puntos en cada vídeo de la biblioteca → «Extraer audio (MP3)». Usa el FFmpeg incluido y guarda en la carpeta de audio.
- **Miniaturas de música descargada**: se guarda la carátula junto al archivo y la biblioteca la muestra; también se reparan las descargas anteriores.
- **YouTube → En vivo**: nueva pestaña con música en directo.
- **Mini reproductor**: se puede ocultar/mostrar tocando el título de la sección (Biblioteca, Galería, YouTube, Descargar o Ajustes).
- **Notificación**: sin animación de carga; muestra pausa en cuanto empieza la reproducción.
- **Galería**: la pestaña inicial ahora es **Fotos** y después **Álbum**.
- **Descargar**: botón de pegar URL desde el portapapeles.

## Novedades 1.7.0

- **YouTube → Playlists**: miniaturas reales en cada lista (NewPipe) y reproducción continua: al acabar una canción sigue la siguiente de la lista.
- Eliminada la pestaña **Tendencias** (mostraba noticias). Ahora **Música** incluye filtros: Éxitos 2026, Pop, Rock, Electro, Trance y Clásicos.
- **Descargas**: al pulsar el botón se elige **Audio (MP3)** o **Vídeo (MP4)**.
- **YouTube → descargar**: siempre guarda el **audio en MP3**.
- **Selección múltiple** en Videos y Canciones (mantener pulsado) para eliminar varios archivos de una vez; también dentro de carpetas, artistas y álbumes.
- **Mini reproductor ocultable**: toca el logo o el título «RV Player» para esconderlo y navegar sin estorbos; vuelve a tocar para mostrarlo.
- Ajustes → Reproducción: eliminado «Tiempo de reproducción».
- Ajustes → Estadísticas: «Música local reproducida» ahora suma solo el tiempo de audio local; eliminado el tiempo de uso de la app.

## Novedades 1.6.0

- Descargas más rápidas: **4 conexiones en paralelo** por rangos para YouTube y `-N 4` en yt-dlp.
- La notificación de descarga abre directamente la sección **Descargar**.
- Eliminado el botón "Probar"; el botón **Actualizar** informa la versión de yt-dlp resultante.
- YouTube con pestañas **Música** y **Playlists** (canciones de YouTube Music y listas separadas).
- Ajustes → **Estadísticas**: tiempo total de reproducción y tiempo de uso de la app.
- Todo el contenido multimedia se ordena por **fecha de modificación más reciente**. 


## Novedades 1.3.0

- **YouTube con NewPipeExtractor** (equivalente Java/Kotlin a YouTubeExplode): resolución de audio casi instantánea y descargas de YouTube sin Python.
- **Galería**: deslizamiento lateral entre imágenes arreglado (el gesto de zoom ya no bloquea el pager) y borrado con el diálogo oficial de MediaStore.
- **Ajustes → Excepciones**: elige carpetas manualmente para que no se escanee multimedia en ellas.
- **Notificación**: barra de progreso arrastrable (MediaSession) y al deslizarla se cierra la app por completo.
- **Botones play/pausa sin spinner**: solo muestran el estado real de reproducción.

## Novedades 1.2.0

- Galería con **deslizamiento entre imágenes** y zoom con doble toque.
- **Playlists**: menú "⋮" en cada canción para agregarla a una playlist (o crear una nueva).
- **Carpetas, artistas y álbumes** abren su lista de canciones; al elegir una, la cola sigue el orden del grupo.
- Animaciones reducidas (transiciones de 120–140 ms y splash más corto).
- Reproductor: vigilante anti-cuelgue (si VLC tarda más de 12 s, muestra error y permite reintentar) y botones ±10 s.
- Descargas: reintento automático en modo compatible, respaldo HTTP y diagnóstico visible del motor yt-dlp.

> `minSdk 24` (requerido por yt-dlp). Solo se empaquetan ABIs **arm64-v8a** y
> **armeabi-v7a** para reducir el tamaño. El APK ronda los 160 MB porque incluye
> LibVLC + Python + FFmpeg.

> `local.properties` contiene la ruta del SDK de este equipo; bórralo al abrir el
> proyecto en otro equipo o en Android Studio.

## Créditos

- [VLC for Android](https://github.com/videolan/vlc) — motor de reproducción.
- [yt-dlp](https://github.com/yt-dlp/yt-dlp) — extracción y descargas.
- [NewPipeExtractor](https://github.com/TeamNewPipe/NewPipeExtractor) — extracción de YouTube (GPLv3).
- [YoutubeDL-Boom](https://github.com/farimarwat/YoutubeDl-Boom) — empaquetado de yt-dlp para Android.
- [SimpMusic](https://github.com/maxrave-dev/SimpMusic) — inspiración de la interfaz.
- [OpenTune](https://github.com/therajatshahare/OpenTune-Music) — inspiración del módulo de YouTube.
- [Gallery](https://github.com/brandonhxrr/Gallery) y [SealPlus](https://github.com/MaheshTechnicals/SealPlus) — inspiración de galería y descargas.
- [LRCLib](https://lrclib.net) — letras.
- Instancias públicas de [Piped](https://github.com/TeamPiped/Piped) — catálogo de YouTube.

## Aviso legal

Descarga únicamente contenido sobre el que tengas derechos o permiso.
