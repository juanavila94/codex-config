---
name: laravel-route-investigation
description: >
  Usar cuando el usuario de un boton, URL, modulo o pantalla
  y quiera saber que ruta usa, que accion ejecuta y que archivos toca.
---

# Rutas Laravel

Objetivo: trazar rapido boton, ruta o accion hasta metodo y dependencias directas.

Reglas:

- Buscar primero por texto visible, URL, `wire:click`, `wire:submit`, `href`, `route()` o `action`.
- Excluir `tests/` salvo pedido explicito.
- Seguir solo llamadas directas del metodo: Livewire o controller -> service -> modelo.
- Si hay condicion de visibilidad, revisar permisos.
- Cortar cuando ya este claro boton, destino, metodo y dependencia principal.
