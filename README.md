# IPTV En Español - Android TV & Google TV (ARM)

Aplicación nativa completa desarrollada en **Kotlin** con arquitectura moderna para **Android TV / Google TV** (optimizada para dispositivos con arquitectura ARM como Onn TV FHD / 4K, Chromecast con Google TV y Smart TVs).

La aplicación reproduce y categoriza los canales en español del proyecto abierto [iptv-org](https://github.com/iptv-org/iptv).

---

## 🌟 Características Principales

1. **Navegación 100% Adaptada a D-Pad / Control Remoto**:
   - Soporte nativo para flechas direccionales (`DPAD_UP`, `DPAD_DOWN`, `DPAD_LEFT`, `DPAD_RIGHT`), botón de selección (`DPAD_CENTER`, `ENTER`) y botón `BACK`.
   - Efecto visual de foco de alto contraste con escala elástica animada y borde neón cian (`#00E5FF`).
   - Pestañas horizontales de categorías con selección automática al posar el cursor o pulsar Enter.
2. **Motor de Reproducción AndroidX Media3 (ExoPlayer)**:
   - Configurado con `DefaultTrackSelector` y `TrackSelectionParameters` para forzar automáticamente la resolución máxima disponible (1080p FHD / 720p HD) en transmisiones HLS (`.m3u8`).
   - `setForceHighestSupportedBitrate(true)` para priorizar la mejor tasa de bits de video.
   - Búfer optimizado para emisiones en vivo por internet.
3. **Resiliencia de Red y Modo Offline**:
   - Descarga en tiempo de ejecución la lista remota actualizada: `https://iptv-org.github.io/iptv/languages/spa.m3u`.
   - Si la conexión falla o el dispositivo está fuera de línea en el primer arranque, activa automáticamente el **fallback offline** empaquetado en `assets/spa.m3u`.
   - Soporte para tráfico Cleartext HTTP mediante `network_security_config.xml` (necesario para señales públicas que aún no usan TLS).
   - Manejo de enlaces rotos o streams caídos sin cierres forzados (*crash-free*).
4. **Carga y Caché de Logos con Coil**:
   - Renderizado asíncrono de logos (`tvg-logo`) con placeholder vectorial temático y soporte crossfade.

---

## 🏗️ Estructura del Proyecto

```text
iptv_player_tv/
├── build.gradle.kts
├── settings.gradle.kts
├── gradle.properties
├── app/
│   ├── build.gradle.kts
│   ├── proguard-rules.pro
│   └── src/main/
│       ├── AndroidManifest.xml
│       ├── assets/
│       │   └── spa.m3u                  <-- Fallback offline empaquetado
│       ├── java/com/antigravity/iptvplayer/
│       │   ├── data/
│       │   │   ├── model/
│       │   │   │   └── Channel.kt       <-- Modelo de datos del canal
│       │   │   ├── parser/
│       │   │   │   └── M3uParser.kt     <-- Parser de directivas #EXTINF y URLs
│       │   │   └── repository/
│       │   │       └── ChannelRepository.kt <-- Lógica de red, caché y fallback
│       │   └── ui/
│       │       ├── MainActivity.kt      <-- Catálogo y navegación TV
│       │       ├── PlayerActivity.kt    <-- Reproductor a pantalla completa
│       │       ├── components/
│       │       │   └── ChannelCard.kt   <-- Tarjeta con foco D-Pad y badges
│       │       ├── theme/
│       │       │   ├── Color.kt
│       │       │   └── Theme.kt
│       │       └── viewmodel/
│       │           └── MainViewModel.kt
│       └── res/
│           ├── drawable/
│           │   ├── ic_banner.xml        <-- Banner para Leanback Launcher TV
│           │   ├── ic_tv_placeholder.xml
│           │   ├── ic_launcher_background.xml
│           │   └── ic_launcher_foreground.xml
│           ├── values/
│           │   ├── colors.xml
│           │   ├── strings.xml
│           │   └── themes.xml
│           └── xml/
│               └── network_security_config.xml
```

---

## 🚀 Compilación e Instalación

### Requisitos
- Android Studio Hedgehog (2023.1.1) o superior / Android SDK 34.
- JDK 17.

### Pasos para compilar el APK:
1. Abrir Android Studio y seleccionar **Open an Existing Project**, eligiendo el directorio:
   `C:\Users\WinterOS\.gemini\antigravity\scratch\iptv_player_tv`
2. Compilar mediante la terminal de Android Studio o Gradle:
   ```bash
   ./gradlew assembleDebug
   ```
3. El APK generado se ubicará en:
   `app/build/outputs/apk/debug/app-debug.apk`

### Instalación en dispositivo Onn TV / Google TV vía ADB:
1. En el dispositivo TV, habilitar **Opciones de desarrollador** y activar **Depuración por red (ADB)**.
2. Conectar tu PC al dispositivo TV:
   ```bash
   adb connect <IP_DEL_DISPOSITIVO_TV>:5555
   ```
3. Instalar el APK directamente:
   ```bash
   adb install -r app/build/outputs/apk/debug/app-debug.apk
   ```
