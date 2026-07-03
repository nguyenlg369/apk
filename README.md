# Flex Tracker — App Android nativa

Tracker personal de bloques de trabajo (millas, ingresos, gastos, paquetes, horas)
construido en **Kotlin + Jetpack Compose + Room** (100% local, sin conexión a internet, sin dependencias de Amazon Flex).

## Qué incluye

- **Panel**: totales de millas (totales y de entrega), ingresos brutos, gastos (gas + peajes),
  ganancia neta, horas, paquetes, y $/hora, $/milla y $/paquete — cada uno en **bruto y neto**.
  Incluye dos gráficas (tendencia de $/hora y comparación ingresos vs. gastos).
- **Nuevo bloque**: formulario para capturar cada turno.
- **Historial**: lista editable/eliminable de todos tus bloques.
- Datos guardados en una base de datos **Room** local en el propio dispositivo — nada sale del teléfono.

## Cómo abrir y compilar el proyecto

1. Abre **Android Studio** (versión Koala/2024.1 o más reciente recomendada).
2. `File > Open` y selecciona la carpeta `FlexTracker` (la que contiene `settings.gradle.kts`).
3. Si Android Studio pregunta por el **Gradle Wrapper**, acepta que lo genere automáticamente
   (o usa `File > Sync Project with Gradle Files`; Android Studio ofrecerá crear el wrapper si falta).
4. Espera a que sincronice las dependencias (necesita internet la primera vez para descargar
   Compose, Room, etc. desde Maven Central / Google — esto lo hace tu máquina, no yo).
5. Conecta tu celular Android (con "Depuración USB" activada) o usa un emulador, y presiona **Run ▶**.
6. Para generar el APK instalable: `Build > Build Bundle(s) / APK(s) > Build APK(s)`.
   El archivo quedará en `app/build/outputs/apk/debug/app-debug.apk`, listo para copiar a tu teléfono e instalar.

## Cómo obtener el APK SIN Android Studio (usando GitHub, gratis)

Este proyecto ya incluye un workflow de **GitHub Actions** (`.github/workflows/build-apk.yml`)
que compila el APK automáticamente en la nube. Solo necesitas un navegador:

1. Crea una cuenta gratuita en [github.com](https://github.com) si no tienes una.
2. Crea un repositorio nuevo (botón verde **New**), puede ser privado o público.
3. Descomprime `FlexTracker.zip` en tu computadora.
4. En la página del repositorio recién creado, haz clic en **"uploading an existing file"**
   (o `Add file > Upload files`), y arrastra **todo el contenido** de la carpeta `FlexTracker`
   (incluyendo la carpeta oculta `.github`) a la ventana del navegador. Confirma con **Commit changes**.
5. Ve a la pestaña **Actions** del repositorio. Debería aparecer una ejecución llamada
   "Compilar APK" corriendo automáticamente (tarda 3-5 minutos). Si no arrancó sola,
   entra al workflow y presiona **Run workflow**.
6. Cuando termine (círculo verde ✅), entra a esa ejecución y baja hasta la sección
   **Artifacts**: ahí encontrarás `FlexTracker-app-debug` — descárgalo (es un .zip que contiene el `.apk`).
7. Transfiere el `.apk` a tu celular (por correo, WhatsApp a ti mismo, Google Drive, o
   directamente abriendo GitHub desde el navegador del teléfono) e instálalo.
   Android te pedirá permitir "instalar apps de origen desconocido" la primera vez — actívalo
   solo para la app que estás usando para abrir el archivo (Archivos, Chrome, Gmail, etc.).

No necesitas instalar nada en tu computadora ni en tu teléfono aparte de esto.

### Alternativa: línea de comandos (sin la IDE, pero sí con el SDK de Android)

Si prefieres compilar en tu propia computadora sin la interfaz gráfica de Android Studio:
1. Instala el JDK 17.
2. Instala las "Android SDK Command-line Tools" (mucho más ligero que Android Studio completo).
3. Desde la carpeta del proyecto: `./gradlew assembleDebug` (o `gradlew.bat assembleDebug` en Windows).
4. El APK queda en `app/build/outputs/apk/debug/app-debug.apk`.



- minSdk 26 (Android 8.0+)
- targetSdk / compileSdk 34
- Kotlin 2.0.0, AGP 8.5.0

## Estructura

```
app/src/main/java/com/flextracker/app/
├── MainActivity.kt          → navegación por pestañas (Panel / Nuevo bloque / Historial)
├── data/                    → Entidad Room, DAO, base de datos, repositorio
├── viewmodel/                → TrackerViewModel + cálculo de totales
└── ui/
    ├── theme/                → colores, tipografía (paleta "tablero de ruta")
    ├── components/            → StatCard, gráficas, divisor
    └── screens/                → Dashboard, formulario, historial
```

## Notas

- Todos los cálculos de $/hora y $/milla se muestran en **bruto** (pago del bloque) y **neto**
  (después de restar gasolina y peajes).
- No hay ninguna funcionalidad de automatización o reserva de bloques de Amazon Flex —
  esta app es solo para llevar tu propio registro manual de estadísticas.
