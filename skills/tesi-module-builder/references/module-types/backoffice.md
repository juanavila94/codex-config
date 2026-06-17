# Modulo tipo backoffice

Usar cuando la ficha describa una pantalla operativa, administracion, accion interna o gestion de estados.

## Revisar primero

- `CLAUDE.md`
- componente Livewire o controller similar
- permisos y middleware relacionados

## Confirmar en la ficha

- ruta o punto de entrada
- actor o rol
- listado, detalle, filtros y acciones
- cambios de estado
- servicios o modelos tocados
- si la pantalla depende de `@can`, Gates o roles por nombre

## Implementacion

- buscar patron similar en `app/Livewire`, `resources/views/livewire` o controllers
- seguir convenciones reales de nombres y renderizado
- revisar permisos de visibilidad y ejecucion
- seguir solo llamadas directas a servicios y modelos relevantes
