---
name: laravel-permission-investigation
description: >
  Usar para permisos, roles, Gates, `@can`, visibilidad de botones,
  acceso a pantallas, `hasRole`, `hasAnyRole` o mezcla entre slug y name.
---

# Permisos Laravel

Objetivo: detectar rapido errores de roles, permisos y Gates.

Reglas:

- Buscar primero permiso, rol, slug, Gate o pantalla exacta con `rg`.
- Excluir `tests/` salvo pedido explicito.
- En este proyecto: `roles_catalogo.slug` se usa en `@can`, `Gate`, `can()` y middleware `can`.
- `hasRole`, `hasAnyRole` y `EtapaDefinicion::rol` suelen usar `name`; verificar implementacion antes de tocar.
- Priorizar Blade, rutas, middleware, Livewire y luego providers o seeders si hace falta.

Comando útil:

```bash
php artisan cache:forget roles_catalogo_bloques
```

No sugerir limpiar toda la cache salvo que haga falta.

## Admins

Recordar:

```text
admin-tesi = super-admin global.
admin-{bloque} = administra solo roles de su bloque.
es-algun-admin = true si es admin-tesi o cualquier admin-{bloque}.
```

Buscar:

```bash
rg "admin-tesi|admin-|es-algun-admin" app resources routes config database
```

## Regla de corte

Detener investigación cuando:

- se encontró el check de permiso exacto,
- se identificó si esperaba slug o name,
- el botón/ruta/acción ya queda explicado,
- o falta dato del usuario.

No seguir buscando roles no relacionados.

## Respuesta esperada

Responder así:

```text
Encontrado:
- resources/views/...
- app/Livewire/...

Problema:
- @can usa name, pero espera slug.

Arreglo:
- Cambiar 'Tramites Verificador' por 'tramites-verificador'.

Cache:
- Si no toma cambio: php artisan cache:forget roles_catalogo_bloques
```
