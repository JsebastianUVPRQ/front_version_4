# API Contract - Flutter app y FastAPI

## Base URL

La app debe recibir la URL base por configuracion:

```text
API_BASE_URL=http://10.0.2.2:8000/
```

Todas las rutas se resuelven contra esa base.

## Autenticacion

### Login

```http
POST /login
Content-Type: application/json
```

Body:

```json
{
  "username": "admin",
  "password": "password"
}
```

Respuesta esperada:

```json
{
  "access_token": "jwt",
  "token_type": "bearer"
}
```

### Usuario actual

```http
GET /usuarios/me
Authorization: Bearer <token>
```

Respuesta esperada:

```json
{
  "id": 1,
  "username": "admin",
  "email": "admin@example.com",
  "nombre": "Admin",
  "apellido": "Principal",
  "rol": "admin",
  "activo": true,
  "fecha_creacion": "2026-05-12T00:00:00"
}
```

## Endpoints protegidos principales

| Recurso | Listar | Crear | Detalle | Actualizar | Eliminar |
| --- | --- | --- | --- | --- | --- |
| Mesas | `GET /mesas/` | `POST /mesas/` | `GET /mesas/{id}` | `PUT /mesas/{id}` | `DELETE /mesas/{id}` |
| Pedidos | `GET /pedidos/` | `POST /pedidos/` | `GET /pedidos/{id}` | `PUT /pedidos/{id}` | `DELETE /pedidos/{id}` |
| Productos | `GET /productos/` | `POST /productos/` | `GET /productos/{id}` | `PUT /productos/{id}` | `DELETE /productos/{id}` |
| Categorias | `GET /categorias/` | `POST /categorias/` | `GET /categorias/{id}` | `PUT /categorias/{id}` | `DELETE /categorias/{id}` |
| Reservas | `GET /reservas/` | `POST /reservas/` | `GET /reservas/{id}` | `PUT /reservas/{id}` | `DELETE /reservas/{id}` |
| Cuentas | `GET /cuentas/` | `POST /cuentas/` | `GET /cuentas/{id}` | `PUT /cuentas/{id}` | `DELETE /cuentas/{id}` |

## Endpoints especiales

```http
GET /mesas/{id}/reserva-activa
GET /cuentas/resumen
GET /cuentas/generar/mesa/{mesaId}
POST /pedidos/{pedidoId}/detalles/
PUT /pedidos/{pedidoId}/detalles/{detalleId}
DELETE /pedidos/{pedidoId}/detalles/{detalleId}
```

## Convenciones

- Usar slash final en endpoints de coleccion: `/mesas/`, `/pedidos/`, etc.
- Enviar token en todos los endpoints protegidos.
- Tratar `401` como sesion invalida.
- Tratar `403` como permiso insuficiente.
- Tratar `422` como error de validacion de formulario.
- Mostrar errores `500` como error del servidor y registrar detalle tecnico en logs de desarrollo.

## Datos que la app debe modelar

- `Usuario`
- `Mesa`
- `Pedido`
- `DetallePedido`
- `Producto`
- `Categoria`
- `Reserva`
- `Cuenta`
- `DetalleCuenta`

Los nombres de campos deben respetar el JSON del backend, que usa `snake_case` en varios atributos como `mesa_id`, `categoria_id`, `fecha_creacion`, `num_personas`.

