# Acceptance Tests - App movil multiplataforma

## Requisitos de entorno

- Backend corriendo en `http://localhost:8000`.
- Usuarios iniciales creados desde `.env`.
- App Flutter ejecutada con `API_BASE_URL` correcto.

Ejemplo Android emulator:

```bash
flutter run --dart-define=API_BASE_URL=http://10.0.2.2:8000/
```

Ejemplo iPhone simulator:

```bash
flutter run --dart-define=API_BASE_URL=http://localhost:8000/
```

## Pruebas de autenticacion

- Login con admin valido devuelve token y abre pantalla admin.
- Login con camarero valido abre pantalla de camarero.
- Login con cocinero valido abre pantalla de cocina.
- Login con password incorrecto muestra error.
- Logout borra token y vuelve a login.
- Token expirado o invalido redirige a login.

## Pruebas por rol

### Admin

- Puede ver mesas.
- Puede ver productos y categorias.
- Puede crear mesa.
- Puede crear producto.
- Puede gestionar usuarios.
- Puede ver cuentas.

### Camarero

- Puede ver mesas.
- Puede crear pedido.
- Puede gestionar reservas.
- Puede generar cuenta.
- No debe acceder a gestion de usuarios.

### Cocinero

- Puede ver pedidos activos.
- Puede actualizar estado de preparacion.
- No debe acceder a usuarios ni cuentas administrativas.

## Pruebas de API

- `GET /usuarios/me` carga datos del usuario autenticado.
- `GET /mesas/` muestra lista o estado vacio.
- `GET /productos/` muestra lista o estado vacio.
- `POST /pedidos/` crea pedido valido.
- `GET /pedidos/` refleja el pedido creado.
- `GET /cuentas/generar/mesa/{mesaId}` genera resumen cuando la mesa tiene pedidos entregados.

## Pruebas de error

- Backend apagado muestra mensaje de conexion.
- Sin internet muestra mensaje de red.
- `401` fuerza logout.
- `403` muestra acceso denegado.
- `422` muestra errores de formulario.
- `500` muestra error del servidor sin cerrar la app.

## Criterios de aceptacion v1

- La misma base de codigo compila para Android e iOS.
- La app inicia sesion contra FastAPI.
- El token no esta hardcodeado.
- La URL del backend se configura por entorno.
- Los tres roles tienen pantallas iniciales diferenciadas.
- Mesas, productos, pedidos, reservas y cuentas consumen datos reales del backend.
- La app no se bloquea ante errores de red.
- El backend no necesita cambios estructurales para soportar la app.

