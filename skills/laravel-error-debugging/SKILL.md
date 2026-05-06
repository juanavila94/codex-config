---
name: laravel-error-debugging
description: >
  Usar cuando el usuario pegue un error, excepción, log, stacktrace
  o diga que algo falla en Laravel, Livewire, Blade, migraciones,
  uploads, roles, pagos, trámites, colas, PDF o backoffice.
  Ejemplos: me tira este error, rompió esto, SQLSTATE, ParseError,
  Property not found, Call to undefined method, Livewire no actualiza,
  falló el upload.
---

# Debug de errores Laravel con bajo consumo de tokens

Objetivo: encontrar la causa del error leyendo la menor cantidad posible de archivos.

## Regla principal

No explorar todo el proyecto.

Primero identificar:

1. mensaje exacto del error,
2. archivo y línea si existen,
3. clase, método, modelo, vista o componente involucrado,
4. módulo probable.

Usar `rg` antes de abrir archivos completos.

## Estilo de respuesta

Responder corto.

Formato:

```text
Causa:
- X.

Archivo:
- path/al/archivo.php

Arreglo:
- Hacer Y.

Duda:
- ¿Aplicar cambio? sí/no