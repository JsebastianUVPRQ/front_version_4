# Implementation Plan - Flutter multiplataforma

## Fase 0 - Preparacion

- Crear nuevo repositorio o carpeta para `restaurant_mobile_app`.
- Instalar Flutter SDK.
- Verificar `flutter doctor`.
- Confirmar backend funcionando en `http://localhost:8000/docs`.
- Crear usuarios iniciales con `crear_usuarios_prueba.py`.

## Fase 1 - Scaffold base

- Crear proyecto Flutter.
- Configurar tema visual inicial.
- Configurar `go_router`.
- Configurar `dio` con `API_BASE_URL`.
- Crear interceptor para `Authorization: Bearer <token>`.
- Crear storage seguro para token.

## Fase 2 - Autenticacion

- Pantalla de login.
- Servicio `AuthApi`.
- Guardado de token.
- Carga de `GET /usuarios/me`.
- Redireccion segun rol.
- Logout.

## Fase 3 - Navegacion por rol

- Shell principal con tabs o drawer adaptado a movil.
- Admin: acceso completo.
- Camarero: mesas, pedidos, reservas, cuentas.
- Cocinero: pedidos activos.
- Bloqueo visual de pantallas sin permiso.

## Fase 4 - Modulos operativos

- Mesas: lista, filtros por estado, detalle.
- Productos/categorias: lista, crear, editar, eliminar segun rol.
- Pedidos: crear pedido, listar pedidos, cambiar estado, gestionar detalles.
- Reservas: listar, crear, editar, cancelar/eliminar.
- Cuentas: generar cuenta, detalle, historial.
- Usuarios: CRUD basico para admin.

## Fase 5 - Calidad y compatibilidad

- Manejo unificado de loading/error/empty.
- Validaciones de formularios.
- Pruebas unitarias de servicios.
- Pruebas de widgets para login y flujos criticos.
- Pruebas manuales en Android emulator.
- Pruebas manuales en iOS simulator o iPhone.

## Fase 6 - Web y mejoras posteriores

- Evaluar Flutter Web.
- Ajustar layouts para escritorio/tablet.
- Integrar WebSockets.
- Agregar notificaciones push si el producto lo requiere.
- Mejorar almacenamiento seguro y expiracion de sesion.

## Orden recomendado de entrega

1. Login funcional.
2. Carga de usuario y rol.
3. Mesas.
4. Productos y categorias.
5. Pedidos.
6. Reservas.
7. Cuentas.
8. Usuarios admin.
9. WebSockets.
10. Web.

