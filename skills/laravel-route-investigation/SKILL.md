---
name: laravel-route-investigation
description: >
  Usar cuando el usuario dé el texto de un botón, una URL, un módulo o una pantalla
  y quiera saber qué ruta usa, qué acción ejecuta y qué archivos están asociados.
  Ejemplos: qué hace el botón Aprobar, qué ruta usa este botón,
  desde esta URL qué componente carga, qué servicio/modelo toca esta acción.
---

# Trazar acción desde botón/ruta en Laravel

Objetivo: encontrar rápido qué ruta, componente, método, service y modelo usa una acción.

## Regla principal

No recorrer todo el proyecto.

Buscar primero por el texto del botón, URL o módulo dado por el usuario.

No buscar ni abrir tests salvo pedido explícito.

## Estilo de respuesta

Responder corto.

Formato:

```text
Botón:
- X.

Ruta:
- GET/POST /...

Destino:
- Livewire/Controller.

Acción:
- método X.

Toca:
- Service Y.
- Modelo Z.

Archivo:
- path/archivo
```

## Flujo

1. Buscar texto del botón o label visible.
2. Encontrar `wire:click`, `wire:submit`, `href`, `route()` o `action`.
3. Ir al componente Livewire o controller.
4. Buscar el método ejecutado.
5. Desde ese método, seguir solo llamadas directas a services/modelos.
6. Cortar cuando la acción queda clara.

## Buscar botón o texto visible

```bash
rg "Texto del botón|Label|placeholder" resources/views app/Livewire
```

## Buscar URL o módulo

```bash
rg "url-o-modulo|segmento-url|nombre-modulo" routes app resources
```

## Buscar acción del botón

```bash
rg "wire:click|wire:submit|href=|route\(|action=" resources/views app/Livewire
```

Si aparece `wire:click="metodo"`:

```bash
rg "function metodo|metodo\(" app/Livewire app/Services app/Models
```

Si aparece `route('nombre.ruta')`:

```bash
rg "name\('nombre.ruta|nombre.ruta|Route::" routes app resources
```

Si aparece URL directa:

```bash
rg "segmento-url" routes app resources
```

## Seguir Livewire

Cuando encuentres componente Livewire:

```bash
rg "class NombreComponente|function render|function metodo" app/Livewire
```

Buscar vista asociada solo si hace falta:

```bash
rg "nombre-componente|livewire.nombre-componente" resources/views app/Livewire
```

## Seguir Controller

Cuando encuentres controller:

```bash
rg "NombreController|function metodo" app/Http/Controllers routes
```

## Seguir services y modelos

Desde el método accionado, seguir solo llamadas directas:

- `app(...)`
- `new Service`
- inyección por constructor
- `Modelo::`
- `$modelo->`
- relaciones usadas ahí

Buscar:

```bash
rg "Service|::class|app\(|new |Modelo::|->" app/Livewire app/Http/Controllers app/Services app/Models
```

No investigar servicios/modelos no llamados por esa acción.

## Permisos

Si el botón está condicionado o no aparece:

```bash
rg "@can|can\(|Gate::|hasRole|hasAnyRole|middleware|admin-tesi|es-algun-admin" resources/views app routes
```

Recordar:

```text
@can / Gate / can() => slug.
hasRole / hasAnyRole => normalmente name DB.
EtapaDefinicion::rol => normalmente name DB.
```

## Regla de corte

Detener cuando esté claro:

- texto del botón,
- ruta o evento Livewire,
- componente/controller,
- método ejecutado,
- service/modelo tocado.

No seguir más profundo salvo que el usuario pida detalle.

## Respuesta esperada

```text
Encontrado:
- Botón "Aprobar" en resources/views/...

Ruta:
- No usa ruta directa. Usa Livewire.

Acción:
- wire:click="aprobar"
- método aprobar() en app/Livewire/...

Toca:
- TramiteService::ejecutarAccion()
- Modelo Tramite

Corte:
- No seguí más porque la acción ya queda clara.
```

