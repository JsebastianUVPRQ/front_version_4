# Guia de cambios para convertir el proyecto en NUESTRA VERSION

Este documento describe los cambios de codigo y configuracion que deben aplicarse para transformar este proyecto Android en una version propia. La guia no incluye secretos reales: todos los valores sensibles deben reemplazarse por placeholders o configurarse fuera del repositorio.

> Importante: esta guia es operativa. No hardcodear passwords, tokens JWT, claves privadas ni credenciales reales en el codigo fuente.

## 1. Valores actuales detectados

| Elemento | Valor actual | Donde aparece |
| --- | --- | --- |
| `namespace` | `rjm.frontrestaurante` | `app/build.gradle.kts` |
| `applicationId` | `rjm.frontrestaurante` | `app/build.gradle.kts` |
| Paquete Kotlin base | `rjm.frontrestaurante` | `app/src/main/java/rjm/frontrestaurante/` |
| Nombre visible app | `Barauto` | `app/src/main/res/values/strings.xml` |
| URL backend REST | `http://10.0.2.2:8000/` | `RetrofitClient.kt`, `ApiClient.kt` |
| Preferencias de sesion | `RestaurantePrefs` | `SessionManager.kt` |
| Preferencias app | `restaurante_preferences` | `AppPreferences.kt` |
| Claves de token | `token`, `auth_token` | `SessionManager.kt`, `AppPreferences.kt` |
| Claves de usuario | `user_id`, `user_name`, `user_role` | `SessionManager.kt`, `AppPreferences.kt` |
| FileProvider authority | `${applicationId}.fileprovider` | `AndroidManifest.xml` |
| Tema base | `Theme.FrontRestaurante` | `AndroidManifest.xml`, `themes.xml` |

## 2. Sustituciones recomendadas para NUESTRA VERSION

Definir estos valores antes de editar el proyecto:

| Concepto | Actual | Nuevo valor recomendado |
| --- | --- | --- |
| Paquete / App ID | `rjm.frontrestaurante` | `com.nuestraempresa.restaurante` |
| Nombre app | `Barauto` | `NuestraVersion` |
| URL backend local emulador | `http://10.0.2.2:8000/` | `http://10.0.2.2:8000/` si el backend corre local |
| URL backend dispositivo fisico | `http://10.0.2.2:8000/` | `http://192.168.X.X:8000/` |
| URL backend produccion | `http://10.0.2.2:8000/` | `https://api.nuestro-dominio.com/` |
| Preferencias sesion | `RestaurantePrefs` | `NuestraVersionSessionPrefs` |
| Preferencias app | `restaurante_preferences` | `nuestra_version_preferences` |
| Icono / logo | `@drawable/logo` | recurso propio, por ejemplo `@drawable/logo_nuestra_version` |
| Tema | `Theme.FrontRestaurante` | `Theme.NuestraVersion` |

## 3. Cambios de codigo necesarios

### Identidad Android

Actualizar `app/build.gradle.kts`:

```kotlin
namespace = "com.nuestraempresa.restaurante"
applicationId = "com.nuestraempresa.restaurante"
```

Mantener `versionCode` y `versionName` segun la politica de versionado propia. Si la app se publicara en Google Play como una aplicacion nueva, el `applicationId` nuevo hara que Android la trate como otra app.

### Paquetes Kotlin e imports

Mover fisicamente:

```text
app/src/main/java/rjm/frontrestaurante/
```

a:

```text
app/src/main/java/com/nuestraempresa/restaurante/
```

Actualizar todas las declaraciones `package` e imports que empiecen por:

```kotlin
rjm.frontrestaurante
```

para que empiecen por:

```kotlin
com.nuestraempresa.restaurante
```

Tambien actualizar las referencias completamente cualificadas dentro del codigo, por ejemplo usos directos de `rjm.frontrestaurante.model.*` o `rjm.frontrestaurante.databinding.*`.

Recomendacion: hacer este cambio con `Refactor > Rename Package` en Android Studio para evitar errores en imports y rutas.

### Manifest y FileProvider

Actualizar `app/src/main/AndroidManifest.xml`:

```xml
<application
    android:name=".RestauranteApp"
    android:label="@string/app_name"
    android:icon="@drawable/logo"
    android:roundIcon="@drawable/logo"
    android:theme="@style/Theme.FrontRestaurante"
    android:usesCleartextTraffic="true">
```

Cambios esperados:

- Si se mantiene `RestauranteApp`, la referencia relativa `android:name=".RestauranteApp"` puede conservarse despues de cambiar el paquete.
- Si se renombra la clase, cambiar `android:name` al nuevo nombre.
- Cambiar `android:icon` y `android:roundIcon` al logo propio.
- Cambiar `android:theme` si se renombra el tema.
- Revisar `android:usesCleartextTraffic="true"`: usar `false` en produccion si el backend usa HTTPS.

El provider actual usa:

```xml
android:authorities="${applicationId}.fileprovider"
```

Esto es correcto porque se adapta al nuevo `applicationId`. Si se reemplaza por un string fijo, debe coincidir con el nuevo paquete.

### Navegacion

Actualizar `app/src/main/res/navigation/nav_graph.xml`.

Todas las entradas `android:name` que ahora apuntan a:

```xml
android:name="rjm.frontrestaurante..."
```

deben apuntar al paquete nuevo:

```xml
android:name="com.nuestraempresa.restaurante..."
```

Esto afecta fragments de login, mesas, pedidos, productos, categorias, reservas, cuentas, usuarios y detail.

### Branding visible

Actualizar `app/src/main/res/values/strings.xml`:

```xml
<string name="app_name">NuestraVersion</string>
```

Revisar textos visibles si deben cambiar de marca, tono o dominio funcional. El proyecto actual usa textos genericos de restaurante, pero el nombre visible actual es `Barauto`.

Actualizar colores de marca en `app/src/main/res/values/colors.xml`. Actualmente hay nombres asociados a Barauto, por ejemplo:

```xml
barauto_brown
barauto_orange
barauto_red
```

Se pueden mantener los nombres internos si solo cambia el color, pero para una version limpia conviene renombrarlos y actualizar referencias.

Actualizar `app/src/main/res/values/themes.xml` y `app/src/main/res/values-night/themes.xml` si se cambia `Theme.FrontRestaurante` a un tema propio.

### Recursos visuales

Reemplazar recursos de marca:

```text
app/src/main/res/drawable/logo.png
app/src/main/res/drawable/logo_restaurante.xml
app/src/main/res/mipmap-*/logo.png
app/src/main/res/mipmap-*/ic_launcher.png
app/src/main/res/mipmap-anydpi-v26/ic_launcher.xml
app/src/main/res/mipmap-anydpi-v26/ic_launcher_round.xml
```

Si se cambian nombres de archivos, actualizar todas las referencias XML y Kotlin relacionadas.

### Backend, links y endpoints

Actualizar las URLs base en:

```text
app/src/main/java/.../api/RetrofitClient.kt
app/src/main/java/.../api/ApiClient.kt
```

Valor actual:

```kotlin
private const val BASE_URL = "http://10.0.2.2:8000/"
```

Opciones:

```kotlin
// Emulador Android apuntando al localhost del PC
private const val BASE_URL = "http://10.0.2.2:8000/"

// Dispositivo fisico en la misma red
private const val BASE_URL = "http://192.168.X.X:8000/"

// Produccion
private const val BASE_URL = "https://api.nuestro-dominio.com/"
```

Reglas importantes:

- La URL de Retrofit debe terminar en `/`.
- No usar `localhost` desde un dispositivo fisico Android.
- Si se usa HTTP en desarrollo, `android:usesCleartextTraffic="true"` permite trafico sin TLS.
- En produccion usar HTTPS y revisar Network Security Config si se requiere politica especifica.

Los endpoints estan declarados en `RestauranteApi.kt` y `ApiService.kt`. Si el backend propio cambia rutas, ajustar ahi las anotaciones `@GET`, `@POST`, `@PUT`, `@DELETE`, etc.

### Tokens, JWT y credenciales

No hardcodear tokens JWT ni credenciales en el repositorio.

Flujo correcto actual:

1. El usuario inicia sesion contra `/login`.
2. El backend devuelve un token.
3. La app guarda el token en `SessionManager` y/o `AppPreferences`.
4. Las llamadas protegidas envian:

```kotlin
"Bearer $token"
```

en el header:

```kotlin
@Header("Authorization") token: String
```

Cambios recomendados para version propia:

- Mantener el token como dato de sesion, nunca como constante.
- Cambiar nombres de preferencias para evitar colision con la version original:

```kotlin
private const val PREF_NAME = "NuestraVersionSessionPrefs"
private const val KEY_TOKEN = "token"
```

```kotlin
private const val PREF_NAME = "nuestra_version_preferences"
private const val KEY_AUTH_TOKEN = "auth_token"
```

- Si se necesita mayor seguridad, migrar el almacenamiento del token a `EncryptedSharedPreferences`.
- No guardar usuario, password, API keys, tokens personales de GitHub ni claves del backend en XML, Kotlin, Gradle o README.

### Preferencias y datos locales

Actualizar estos archivos si se quiere aislar la nueva app de instalaciones previas:

```text
app/src/main/java/.../util/SessionManager.kt
app/src/main/java/.../util/AppPreferences.kt
```

Valores actuales:

```kotlin
private const val PREF_NAME = "RestaurantePrefs"
private const val PREF_NAME = "restaurante_preferences"
```

Valores sugeridos:

```kotlin
private const val PREF_NAME = "NuestraVersionSessionPrefs"
private const val PREF_NAME = "nuestra_version_preferences"
```

Si existe base de datos Room, revisar `AppDatabase.kt` y definir un nombre propio para la base de datos si actualmente usa un nombre heredado.

### Tests

Actualizar paquetes en:

```text
app/src/test/java/rjm/frontrestaurante/
app/src/androidTest/java/rjm/frontrestaurante/
```

Moverlos a:

```text
app/src/test/java/com/nuestraempresa/restaurante/
app/src/androidTest/java/com/nuestraempresa/restaurante/
```

Actualizar el test instrumentado que comprueba el package name:

```kotlin
assertEquals("com.nuestraempresa.restaurante", appContext.packageName)
```

## 4. Checklist de conversion

- [ ] Definir paquete final, por ejemplo `com.nuestraempresa.restaurante`.
- [ ] Cambiar `namespace` y `applicationId`.
- [ ] Renombrar/mover carpeta Kotlin base.
- [ ] Actualizar declaraciones `package`.
- [ ] Actualizar imports y referencias completamente cualificadas.
- [ ] Actualizar `nav_graph.xml`.
- [ ] Actualizar `AndroidManifest.xml`.
- [ ] Actualizar `strings.xml` con el nombre visible propio.
- [ ] Cambiar logos, iconos y recursos visuales.
- [ ] Actualizar colores y temas si aplica.
- [ ] Cambiar `BASE_URL` en `RetrofitClient.kt` y `ApiClient.kt`.
- [ ] Confirmar que ningun token, password o API key real queda en el repositorio.
- [ ] Cambiar nombres de preferencias si se quiere aislar la app.
- [ ] Actualizar tests unitarios e instrumentados.
- [ ] Compilar y probar.

## 5. Comandos de validacion

Ejecutar desde la raiz del proyecto:

```bash
./gradlew clean
./gradlew test
./gradlew assembleDebug
```

Si hay emulador o dispositivo conectado:

```bash
./gradlew connectedAndroidTest
```

Comprobaciones utiles:

```bash
git status --short
rg -n "rjm\\.frontrestaurante|Barauto|10\\.0\\.2\\.2|RestaurantePrefs|restaurante_preferences" app
rg -n "password|secret|api_key|apikey|token|Bearer" app README.md *.md
```

El primer comando `rg` debe devolver solo referencias que intencionalmente se hayan conservado. El segundo debe revisarse manualmente: puede mostrar claves de nombres internos como `KEY_TOKEN`, pero no debe mostrar secretos reales.

## 6. Notas de seguridad

- No subir credenciales reales al repositorio.
- No usar passwords personales como mecanismo de despliegue.
- Para GitHub usar SSH key, GitHub CLI autenticado o Personal Access Token guardado en el credential manager, nunca escrito en archivos del proyecto.
- Para backend de produccion usar HTTPS.
- Considerar `EncryptedSharedPreferences` para tokens si la app manejará datos sensibles reales.
- Revisar logs antes de publicar: actualmente hay logging HTTP en nivel `BODY`, util para desarrollo pero riesgoso en produccion porque puede exponer datos de requests/responses.

