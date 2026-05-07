---
name: laravel-permission-investigation
description: >
  Usar cuando el usuario pregunte por permisos, roles, Gates, @can,
  visibilidad de botones, acceso a pantallas, acciones protegidas,
  roles_catalogo, roles_bloques, RolService, hasRole, hasAnyRole
  o errores donde se mezcle slug con name.
  Ejemplos: por qué no veo este botón, qué rol necesita esta pantalla,
  por qué no pasa el permiso, revisar permisos, este rol no matchea,
  admin-tesi, admin-bloque, @can no funciona.
---

# Investigación de permisos Laravel con bajo consumo de tokens

Objetivo: detectar rápido errores de permisos, roles y Gates sin recorrer todo el proyecto.

## Regla principal

No abrir todo el proyecto.

Primero buscar el permiso, rol, slug, Gate o pantalla exacta con `rg`.

No buscar ni abrir tests salvo que el usuario lo pida explícitamente.

## Contexto crítico del proyecto

En este proyecto hay dos valores distintos:

```text
roles_catalogo.name = nombre humano del rol guardado/asignado en DB
roles_catalogo.slug = slug usado para Gates, @can y permisos Laravel
```

Regla:

```text
@can / Gate / user()->can() => usar slug
hasRole / hasAnyRole por nombre DB => usar name
EtapaDefinicion::rol => normalmente usa name exacto del rol
```

Ejemplo:

```php
@can('tramites-verificador')
```

usa slug.

```php
$user->hasRole('Tramites Verificador')
```

usa name.

No mezclar:

```php
$user->hasRole('tramites-verificador')
@can('Tramites Verificador')
```

## Estilo de respuesta

Responder corto.

Formato:

```text
Encontrado:
- X.

Problema:
- Usa slug donde espera name.

Arreglo:
- Cambiar X por Y.

Duda:
- ¿Aplicar? sí/no
```

No explicar de más salvo que el usuario pida detalle.

## Búsqueda inicial

Si el usuario menciona un permiso/slug:

```bash
rg "permiso|slug|admin-tesi|admin-|tramites-|loys-|servicios-" app resources routes config database
```

Si menciona un rol por nombre:

```bash
rg "Nombre Rol|hasRole|hasAnyRole|EtapaDefinicion|rol:" app resources routes config database
```

Si menciona botón o pantalla que no aparece:

```bash
rg "@can|can\(|cannot\(|Gate::|hasRole|hasAnyRole|middleware|role|roles_catalogo|roles_bloques|RolService" app resources routes config database
```

## Prioridad de revisión

Priorizar:

1. Blade donde se oculta/muestra el botón.
2. Ruta o middleware que protege la pantalla.
3. Livewire component que ejecuta la acción.
4. Controller si aplica.
5. `EtapaDefinicion` si es trámite/backoffice.
6. `RolService` / provider solo si el error apunta al registro de Gates.
7. Migraciones/seeders/config de roles solo si hay duda sobre `name` vs `slug`.

## Casos típicos a detectar

### 1. `@can` usando name

Incorrecto:

```php
@can('Tramites Verificador')
```

Correcto:

```php
@can('tramites-verificador')
```

Porque `@can` usa Gate slug.

### 2. `user()->can()` usando name

Incorrecto:

```php
auth()->user()->can('Tramites Verificador')
```

Correcto:

```php
auth()->user()->can('tramites-verificador')
```

### 3. `hasRole()` usando slug

Sospechoso:

```php
$user->hasRole('tramites-verificador')
```

Probablemente correcto:

```php
$user->hasRole('Tramites Verificador')
```

### 4. `hasAnyRole()` mezclando slug/name

Sospechoso:

```php
$user->hasAnyRole(['tramites-inspector', 'Tramites Director'])
```

Debe usar formato consistente según implementación real del modelo.

Revisar método antes de cambiar:

```bash
rg "function hasRole|function hasAnyRole" app/Models app
```

### 5. `EtapaDefinicion::rol`

Si la etapa define rol:

```php
rol: 'Tramites Verificador'
```

Debe coincidir con el name esperado por el flujo de etapas.

No cambiar a slug salvo verificar que `TramiteService` o la lógica de permisos lo espere.

Buscar:

```bash
rg "EtapaDefinicion|rol:" app/Tramites app
```

## Búsquedas útiles

Permisos en Blade:

```bash
rg "@can|@cannot|can\(|cannot\(" resources/views app/Livewire
```

Permisos en PHP:

```bash
rg "Gate::|->can\(|hasRole|hasAnyRole|authorize|middleware" app routes
```

Roles del sistema:

```bash
rg "RolService|roles_catalogo|roles_bloques|admin-tesi|es-algun-admin|admin-" app config database routes resources
```

Etapas de trámites:

```bash
rg "EtapaDefinicion|rol:" app/Tramites
```

Rutas protegidas:

```bash
rg "middleware|can:|auth|role|admin-tesi|es-algun-admin" routes app
```

## Regla name vs slug

Antes de corregir, clasificar el uso:

```text
Si es Gate / @can / can() / middleware can:
- espera slug.

Si es hasRole / hasAnyRole:
- verificar implementación.
- normalmente espera name DB.

Si es EtapaDefinicion::rol:
- normalmente espera name DB exacto.

Si es roles_catalogo.slug:
- slug para Gate.

Si es roles_catalogo.name:
- name humano para asignación/relación DB.
```

## Cache de roles

Si el código parece correcto pero el permiso no aparece, revisar cache.

Buscar:

```bash
rg "roles_catalogo_bloques|cache|RolService" app config
```

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
