# Hero Sports — App Android
## Proyecto WebView listo para APK directa y Play Store

---

## ⚡ CAMBIOS RÁPIDOS (sin tocar código Java)

### Cambiar la URL de la web
Archivo: `app/src/main/java/com/herostream/sportsmob/MainActivity.java`
Línea: `private static final String HOME_URL = "https://sportsmob.herostream.org";`
→ Cambia la URL y vuelve a compilar.

### Cambiar el nombre de la app
Archivo: `app/src/main/res/values/strings.xml`
→ Cambia `<string name="app_name">Hero Sports</string>`

### Cambiar colores
Archivo: `app/src/main/res/values/colors.xml`
→ Paleta Grupo TC ya aplicada. Cambia los hex si quieres otro color.

### Añadir un dominio al WebView (que abra dentro de la app)
Archivo: `app/src/main/java/com/herostream/sportsmob/MainActivity.java`
→ Añade el dominio a `INTERNAL_HOSTS`

### Cambiar versión de la app
Archivo: `app/build.gradle`
→ `versionCode` (número, +1 en cada update)
→ `versionName` (texto visible, ej: "1.1")

---

## 🏗️ COMPILAR — Paso a paso

### Requisitos
- Android Studio Hedgehog 2023.1 o más nuevo (gratis: https://developer.android.com/studio)
- JDK 17 (viene incluido con Android Studio)
- Android SDK 36 (se instala automático al abrir el proyecto)

### Pasos
1. Abre Android Studio → "Open" → selecciona esta carpeta `HeroStream/`
2. Espera a que Gradle sincronice (puede tardar 3-5 min la primera vez)
3. **Para APK directa (instalar sin Play Store):**
   - Menú: `Build → Build Bundle(s) / APK(s) → Build APK(s)`
   - El APK se guarda en: `app/build/outputs/apk/release/HeroStream-1.0-release.apk`
   - Envía ese APK por WhatsApp, Telegram o descarga desde tu web

4. **Para Play Store (cuando estés listo):**
   - Sigue la sección "Preparar para Play Store" más abajo

---

## 📱 INSTALAR EL APK EN EL TELÉFONO

### Método 1: WhatsApp / Telegram
Envía el archivo `.apk` por WhatsApp al teléfono del cliente.
El cliente abre el archivo y acepta "Instalar de fuente desconocida" una sola vez.

### Método 2: Descargar desde tu web
Sube el APK a tu servidor y crea un enlace:
```
https://sportsmob.herostream.org/app/HeroStream.apk
```
El cliente entra al enlace desde Chrome en su Android → descarga → instala.

### ⚠️ Aviso de "fuente desconocida"
Android mostrará: *"Esta app no fue instalada desde Play Store"*
Esto es NORMAL para APKs directas. El usuario toca "Instalar de todas formas".
No es un error de seguridad — es una advertencia estándar de Android.

---

## 🎨 CAMBIAR EL ÍCONO (IMPORTANTE)

El ícono actual es un placeholder vectorial (letras HS).
Para usar tu logo real:

1. Prepara tu logo en PNG con fondo transparente, tamaño 1024x1024
2. En Android Studio: click derecho en `app/src/main/res` → `New → Image Asset`
3. Tipo: "Launcher Icons (Adaptive and Legacy)"
4. Foreground: tu imagen PNG
5. Background: color `#C0172B` (Rojo Jester) o transparente
6. Android Studio genera automáticamente todos los tamaños

O bien, copia tu PNG en estas rutas manualmente:
- `mipmap-mdpi/ic_launcher.png`     → 48×48 px
- `mipmap-hdpi/ic_launcher.png`     → 72×72 px
- `mipmap-xhdpi/ic_launcher.png`    → 96×96 px
- `mipmap-xxhdpi/ic_launcher.png`   → 144×144 px
- `mipmap-xxxhdpi/ic_launcher.png`  → 192×192 px

---

## 🏪 PREPARAR PARA PLAY STORE (cuando estés listo)

### 1. Crear cuenta de desarrollador
- Ve a https://play.google.com/console
- Paga $25 USD una sola vez (sin renovación)
- Verifica tu identidad

### 2. Crear keystore (firma digital — GUÁRDALA BIEN, no se puede perder)
En Android Studio: `Build → Generate Signed Bundle/APK`
→ "Create new" → guarda el archivo `.keystore` y las contraseñas en un lugar seguro.

### 3. Configurar firma en build.gradle
Descomenta el bloque `signingConfigs` en `app/build.gradle` y llena los datos del keystore.

### 4. Generar AAB (formato requerido por Play Store)
`Build → Generate Signed Bundle/APK → Android App Bundle`
→ Sube el `.aab` a Play Console

### 5. Completar ficha en Play Console
- Ícono 512×512 PNG
- Screenshots de la app
- Descripción corta y larga
- Clasificación de contenido (marcar como "deportes")

### 6. Política de privacidad (obligatoria)
Play Store requiere una URL con tu política de privacidad.
Ejemplo simple que puedes alojar en tu web:
```
https://sportsmob.herostream.org/privacidad
```

---

## 🔧 SOLUCIÓN DE PROBLEMAS

### Gradle no sincroniza
→ `File → Invalidate Caches → Invalidate and Restart`

### Error "minSdk too low"
→ En `app/build.gradle` cambia `minSdk 24` a `minSdk 26` si es necesario

### El video no reproduce
→ Verifica que `setMediaPlaybackRequiresUserGesture(false)` esté en el código (ya está)
→ Si el stream usa HTTP (no HTTPS), edita `network_security_config.xml`

### La app cierra sola
→ Habilita logs: `adb logcat -s HeroStream`

---

## 📦 ESTRUCTURA DEL PROYECTO

```
HeroStream/
├── app/
│   ├── build.gradle              ← versión, minSdk, targetSdk
│   └── src/main/
│       ├── AndroidManifest.xml   ← permisos, deep links
│       ├── java/com/herostream/sportsmob/
│       │   └── MainActivity.java ← LÓGICA PRINCIPAL — editar aquí
│       └── res/
│           ├── values/
│           │   ├── strings.xml   ← nombre de la app
│           │   ├── colors.xml    ← paleta de colores
│           │   └── styles.xml    ← tema visual
│           ├── xml/
│           │   └── network_security_config.xml  ← seguridad de red
│           └── mipmap-*/
│               └── ic_launcher* ← íconos (reemplazar con tu logo)
├── build.gradle                  ← versión del plugin Android
├── settings.gradle               ← nombre del proyecto
└── gradle.properties             ← config de Gradle
```
