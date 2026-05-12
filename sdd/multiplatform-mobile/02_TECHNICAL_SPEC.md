# Technical Spec - Flutter mobile app

## Stack recomendado

- Framework: Flutter.
- Lenguaje: Dart.
- HTTP client: `dio`.
- Estado: `riverpod` o `flutter_riverpod`.
- Modelos JSON: `freezed` + `json_serializable`, o modelos manuales en fase inicial.
- Almacenamiento seguro: `flutter_secure_storage`.
- Navegacion: `go_router`.
- Configuracion de entorno: `--dart-define=API_BASE_URL=...`.

## Estructura propuesta

```text
restaurant_mobile_app/
├── lib/
│   ├── main.dart
│   ├── app/
│   │   ├── app.dart
│   │   ├── router.dart
│   │   └── theme.dart
│   ├── core/
│   │   ├── config/
│   │   ├── http/
│   │   ├── storage/
│   │   └── errors/
│   ├── features/
│   │   ├── auth/
│   │   ├── mesas/
│   │   ├── pedidos/
│   │   ├── productos/
│   │   ├── categorias/
│   │   ├── reservas/
│   │   ├── cuentas/
│   │   └── usuarios/
│   └── shared/
│       ├── widgets/
│       └── models/
└── test/
```

## Configuracion de backend

La app debe leer la URL base desde entorno:

```bash
flutter run --dart-define=API_BASE_URL=http://10.0.2.2:8000/
```

Valores por plataforma:

- Android emulator: `http://10.0.2.2:8000/`
- Android fisico: `http://IP_DEL_PC:8000/`
- iPhone simulator: `http://localhost:8000/` si backend corre en el mismo Mac.
- iPhone fisico: `http://IP_DEL_PC_O_MAC:8000/`
- Produccion: `https://api.tu-dominio.com/`

## Autenticacion

La app debe:

1. Enviar usuario y contrasena a `POST /login`.
2. Guardar `access_token` en storage seguro.
3. Adjuntar `Authorization: Bearer <token>` en endpoints protegidos.
4. Borrar token en logout.
5. Redirigir a login si recibe `401`.

## Navegacion por rol

Despues de login:

- `admin`: dashboard con mesas, pedidos, productos, categorias, reservas, cuentas y usuarios.
- `camarero`: mesas, pedidos, reservas y cuentas.
- `cocinero`: pedidos activos.

La app no debe ocultar errores de permisos. Si el backend responde `403`, mostrar mensaje de acceso no autorizado.

## Manejo de errores

Casos obligatorios:

- Backend apagado o inaccesible.
- Credenciales invalidas.
- Token expirado.
- Error de permisos.
- Respuesta JSON inesperada.
- Lista vacia.

## WebSockets

Fase inicial: consumir REST y refrescar manualmente.

Fase posterior: integrar:

- `/ws/cocina`
- `/ws/camareros`
- `/ws/admin`

Los WebSockets deben autenticarse con token segun el contrato del backend.

