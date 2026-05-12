# SDD - Nueva app movil multiplataforma

## Objetivo

Definir, planear y acotar una nueva aplicacion movil multiplataforma para el sistema de restaurante, usando una sola base de codigo para:

- Android
- iPhone
- Web en una fase posterior

El backend existente `FastAPI-Restaurant-Server-main` se mantiene como API central. La app nueva consumira los endpoints ya disponibles:

- `/login`
- `/usuarios/me`
- `/mesas/`
- `/pedidos/`
- `/productos/`
- `/categorias/`
- `/reservas/`
- `/cuentas/`

## Decision tecnica recomendada

Usar **Flutter** para la nueva app.

Motivos:

- Permite una sola base de codigo para Android, iOS y web.
- Tiene buen soporte para interfaces tipo dashboard, listas, formularios y estados de carga.
- Reduce el mantenimiento frente a tener Android Kotlin e iOS Swift separados.
- Facilita construir una version inicial funcional sin depender de componentes nativos complejos.

## Archivos SDD

- `01_PRODUCT_SPEC.md`: alcance funcional, usuarios, roles y flujos principales.
- `02_TECHNICAL_SPEC.md`: arquitectura propuesta, dependencias, estado, navegacion y almacenamiento.
- `03_API_CONTRACT.md`: contrato esperado entre Flutter y FastAPI.
- `04_IMPLEMENTATION_PLAN.md`: fases de implementacion y orden de trabajo.
- `05_ACCEPTANCE_TESTS.md`: pruebas, criterios de aceptacion y checklist de validacion.

## Fuera de alcance inicial

- Reescribir el backend.
- Publicar en App Store o Play Store.
- Pagos reales.
- Modo offline completo.
- Notificaciones push nativas.
- Panel web administrativo avanzado.

