# 🚀 Guía de Cambios para Convertir a Nuestra Versión

Este documento describe **todos los cambios necesarios** para adaptar el proyecto **FrontRestaurante** a **nuestra propia versión** (cambiar nombres de paquete, aplicación, servidor, etc.).

> ⚠️ **IMPORTANTE**: NO modifiques ningún archivo aún. Sigue esta guía paso a paso cuando estés listo.

---

## 1. 🔧 Cambiar el nombre del paquete (Package / Application ID)

### Archivo: `app/build.gradle.kts`
| Línea | Cambio |
|-------|--------|
| `namespace = "rjm.frontrestaurante"` | → Cambiar a `"tunuevopaquete.app"` |
| `applicationId = "rjm.frontrestaurante"` | → Cambiar a `"tunuevopaquete.app"` |

**Nota**: Ambos valores deben ser el mismo string (ej. `"micafe.app"`, `"com.mirestaurante.app"`, etc.).

---

## 2. 📁 Renombrar la estructura de directorios del código fuente

### Mover de:
```
app/src/main/java/rjm/frontrestaurante/
```

### A:
```
app/src/main/java/tunuevopaquete/app/
```

Esto implica **mover físicamente** la carpeta `rjm/frontrestaurante` a `tunuevopaquete/app` dentro de `app/src/main/java/`.

Además, en Android Studio puedes hacer **Refactor → Rename Package** para que Kotlin actualice automáticamente todos los imports y declaraciones `package`.

---

## 3. 📝 Actualizar todas las declaraciones `package` en archivos Kotlin

Cada archivo `.kt` dentro de `app/src/main/java/rjm/frontrestaurante/` tiene una línea al inicio como:

```kotlin
package rjm.frontrestaurante.api       // RetrofitClient.kt, ApiClient.kt, etc.
package rjm.frontrestaurante.model      // Mesa.kt, Producto.kt, etc.
package rjm.frontrestaurante.adapter    // ProductoAdapter.kt, etc.
package rjm.frontrestaurante.ui.login   // LoginActivity.kt, etc.
package rjm.frontrestaurante.ui.main    // MainActivity.kt, etc.
package rjm.frontrestaurante.ui.mesas   // DetalleMesaFragment.kt, etc.
package rjm.frontrestaurante.ui.pedidos // DetallePedidoFragment.kt, etc.
package rjm.frontrestaurante.ui.productos // AgregarProductoFragment.kt, etc.
package rjm.frontrestaurante.ui.categorias // CategoriasFragment.kt, etc.
package rjm.frontrestaurante.ui.reservas // ReservasFragment.kt, etc.
package rjm.frontrestaurante.ui.cuentas  // CuentasFragment.kt, etc.
package rjm.frontrestaurante.ui.detail   // DetailFragment.kt, etc.
package rjm.frontrestaurante.ui.usuarios // UsuariosFragment.kt, etc.
package rjm.frontrestaurante.util        // SessionManager.kt, etc.
package rjm.frontrestaurante.util.db     // AppDatabase.kt, etc.
```

**Todas** deben cambiarse a:

```kotlin
package tunuevopaquete.app.api
package tunuevopaquete.app.model
// etc.
```

---

## 4. 🔄 Actualizar imports en todos los archivos Kotlin

Cada import como:
```kotlin
import rjm.frontrestaurante.*
```

Debe cambiarse a:
```kotlin
import tunuevopaquete.app.*
```

Esto afecta a **decenas de archivos**. Usa **Refactor → Rename Package** en Android Studio para hacerlo automáticamente.

---

## 5. 📱 `AndroidManifest.xml` - Cambiar referencias a clases

### Archivo: `app/src/main/AndroidManifest.xml`

| Línea actual | Cambiar a |
|-------------|-----------|
| `android:name=".RestauranteApp"` | `android:name="tunuevopaquete.app.RestauranteApp"` |
| `android:name=".ui.login.LoginActivity"` | `android:name="tunuevopaquete.app.ui.login.LoginActivity"` |
| `android:name=".ui.main.MainActivity"` | `android:name="tunuevopaquete.app.ui.main.MainActivity"` |

---

## 6. 🏪 Cambiar el nombre de la aplicación

### Archivo: `app/src/main/res/values/strings.xml`
```xml
<string name="app_name">Barauto</string>
```
→ Cambiar a:
```xml
<string name="app_name">MiRestauranteApp</string>
```
(El nombre que quieras que aparezca en el lanzador de Android)

---

## 7. 🌐 Configurar la URL del servidor backend

### Archivo: `app/src/main/java/.../api/RetrofitClient.kt`
```kotlin
private const val BASE_URL = "http://10.0.2.2:8000/"
```
→ Cambiar a la IP/puerto de **tu servidor real**:

- **Emulador Android** → `http://10.0.2.2:8000/` (OK, apunta al localhost del host)
- **Dispositivo físico (misma red WiFi)** → `http://192.168.X.X:8000/`
- **Servidor remoto** → `https://tuservidor.com/`
- **localhost desde dispositivo físico** → `http://10.0.2.2:8000/` **NO funciona**, necesitas la IP real

### Archivo: `app/src/main/java/.../api/ApiClient.kt`
```kotlin
private const val BASE_URL = "http://10.0.2.2:8000/"
```
→ Cambiar al mismo valor que arriba.

---

## 8. 🔐 Nombre de preferencias compartidas (opcional)

### Archivo: `app/src/main/java/.../util/SessionManager.kt`
```kotlin
private const val PREF_NAME = "RestaurantePrefs"
```
→ Cambiar a (opcional):
```kotlin
private const val PREF_NAME = "MiAppPrefs"
```

---

## 9. 🎨 Cambiar iconos y recursos visuales (opcional)

### Ubicación:
```
app/src/main/res/mipmap-hdpi/
app/src/main/res/mipmap-mdpi/
app/src/main/res/mipmap-xhdpi/
app/src/main/res/mipmap-xxhdpi/
app/src/main/res/mipmap-xxxhdpi/
app/src/main/res/mipmap-anydpi-v26/
app/src/main/res/drawable/
```

Reemplaza estos recursos con los iconos/drawables de tu propia marca.

En `AndroidManifest.xml`:
```xml
android:icon="@drawable/logo"
android:roundIcon="@drawable/logo"
```
Puedes cambiar `@drawable/logo` por tu propio recurso.

---

## 10. 🏗️ Cambiar el nombre de la clase `RestauranteApp`

### Archivo: `app/src/main/java/.../RestauranteApp.kt`
Si quieres renombrar la clase de aplicación:
```kotlin
class RestauranteApp : Application()
```
→ Cambiar a:
```kotlin
class MiApp : Application()
```

**IMPORTANTE**: Si renombras esta clase, actualiza su referencia en:
- `AndroidManifest.xml` → `android:name`
- Cualquier otro archivo que la importe/usen

---

## ✅ Lista de verificación final

- [ ] `build.gradle.kts` → `namespace` y `applicationId` cambiados
- [ ] Estructura de directorios renombrada a `tunuevopaquete/app/`
- [ ] Todas las declaraciones `package` actualizadas en cada `.kt`
- [ ] Todos los `import` actualizados (usar refactor automático)
- [ ] `AndroidManifest.xml` → clases y `applicationId` actualizados
- [ ] `strings.xml` → `app_name` cambiado
- [ ] `RetrofitClient.kt` → `BASE_URL` apuntando a tu servidor
- [ ] `ApiClient.kt` → `BASE_URL` apuntando a tu servidor
- [ ] `SessionManager.kt` → `PREF_NAME` (opcional)
- [ ] Recursos visuales (iconos, drawables) reemplazados (opcional)
- [ ] Compilar y probar: `./gradlew clean assembleDebug`

---

## 🔄 Comandos útiles

```bash
# Limpiar build anterior
./gradlew clean

# Compilar DEBUG (genera APK)
./gradlew assembleDebug

# Probar unitarias
./gradlew test

# Probar instrumentadas (requiere emulador/dispositivo)
./gradlew connectedAndroidTest

# Instalar APK en dispositivo
adb install app/build/outputs/apk/debug/app-debug.apk
```

---

## 📌 Notas importantes

1. **Siempre haz un `git commit` del proyecto original** antes de hacer estos cambios, por si necesitas revertir.
2. El cambio de paquete es **delicado**: usa la herramienta **Refactor → Rename Package** de Android Studio para evitar errores manuales.
3. Si el servidor FastAPI usa HTTPS, recuerda que `BASE_URL` debe empezar con `https://` en lugar de `http://`.
4. Si cambias el `applicationId`, la Google Play Store (si la usas) lo tratará como una app diferente.