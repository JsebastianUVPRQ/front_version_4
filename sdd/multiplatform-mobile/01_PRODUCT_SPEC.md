# Product Spec - App movil multiplataforma restaurante

## Resumen

Construir una nueva app movil multiplataforma para operar el restaurante desde Android e iPhone usando el backend FastAPI existente.

La app debe cubrir los flujos ya presentes en la version Android actual:

- Login con JWT.
- Navegacion segun rol.
- Gestion de mesas.
- Gestion de pedidos.
- Gestion de productos y categorias.
- Gestion de reservas.
- Gestion de cuentas.
- Gestion de usuarios para administradores.

## Usuarios y roles

### Administrador

Puede:

- Ver y gestionar mesas.
- Crear, editar o eliminar productos.
- Crear, editar o eliminar categorias.
- Ver y gestionar pedidos.
- Crear y gestionar usuarios.
- Ver historial y resumen de cuentas.
- Gestionar reservas.

### Camarero

Puede:

- Iniciar sesion.
- Ver mesas disponibles, ocupadas y reservadas.
- Crear pedidos para una mesa.
- Agregar o eliminar productos de un pedido.
- Marcar pedidos como entregados cuando corresponda.
- Crear y gestionar reservas.
- Generar cuenta/cierre de mesa.

### Cocinero

Puede:

- Iniciar sesion.
- Ver pedidos activos.
- Cambiar estado de pedidos o detalles segun permisos.
- Marcar preparaciones como listas.

## Flujos principales

### Login

1. Usuario ingresa usuario y contrasena.
2. App llama `POST /login`.
3. Backend devuelve `access_token`.
4. App guarda token de forma local segura.
5. App llama `GET /usuarios/me`.
6. App redirige a la pantalla inicial segun rol.

### Mesas

1. App llama `GET /mesas/`.
2. Muestra mesas agrupadas o filtradas por estado.
3. Usuario selecciona una mesa.
4. App muestra detalle de mesa, pedidos activos y reserva activa si existe.

### Pedidos

1. Camarero selecciona mesa.
2. App lista productos desde `GET /productos/`.
3. Usuario selecciona productos y cantidades.
4. App crea pedido con `POST /pedidos/`.
5. Cocina ve pedidos activos.
6. Cocinero actualiza estados con `PUT /pedidos/{id}` o endpoints de detalles.

### Reservas

1. Usuario consulta reservas con `GET /reservas/`.
2. Crea reserva con `POST /reservas/`.
3. Actualiza o elimina reservas segun permisos.

### Cuentas

1. Camarero genera cuenta con `GET /cuentas/generar/mesa/{mesaId}`.
2. App muestra resumen.
3. Usuario registra metodo de pago con `PUT /cuentas/{id}` si aplica.
4. App muestra historial con `GET /cuentas/`.

## Requisitos no funcionales

- La app debe mostrar errores comprensibles cuando el backend no este disponible.
- El token no debe hardcodearse.
- La URL del backend debe configurarse por entorno.
- Debe haber estados de carga, error y vacio en listas.
- La UI debe ser usable en telefono pequeno y pantalla grande.
- La app debe evitar duplicar acciones criticas por doble toque.

