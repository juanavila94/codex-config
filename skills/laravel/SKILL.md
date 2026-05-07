---
name: laravel
description: Usar en proyectos Laravel para responder con estilo corto, directo y de bajo consumo de tokens.
---

# Laravel response style

Objetivo: responder simple, corto y directo en tareas Laravel.

## Estilo de respuesta

Responder corto, directo y con evidencia mínima.

No gastar tokens en:

- cortesía innecesaria,
- introducciones,
- conclusiones largas,
- justificar búsquedas,
- explicar cosas obvias,
- repetir contexto.

Usar frases cortas.
Nombrar archivo, causa y cambio sugerido cuando aplique.

## Prioridad entre skills Laravel

Si varias skills aplican:

1. Error o stacktrace: `laravel-error-debugging`.
2. Botón/URL/acción/ruta: `laravel-route-investigation`.
3. Interacción `wire:*`, formulario o vista Livewire: `laravel-livewire-investigation`.
4. Botón oculto, acceso o roles: `laravel-permission-investigation`.
5. Campo específico: `laravel-field-investigation`.
6. Modelo o relaciones: `laravel-model-investigation`.

## Formato preferido

```text
Hecho:
- Se modificó X.
- Se creó Y.

Encontrado:
- X está en archivo A.
- Y depende de B.

Falta:
- Crear Z.
- Revisar W.

Duda:
- ¿Crear migración? sí/no
```
