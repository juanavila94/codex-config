---
name: laravel-livewire-investigation
description: >
  Usar para pantallas, formularios, botones, acciones, inputs,
  `wire:model`, `wire:click`, wizards o componentes Livewire.
---

# Livewire Laravel

Objetivo: ubicar pantalla, componente, accion o formulario sin recorrer todo.

Reglas:

- No abrir todos los componentes.
- Buscar primero por texto visible, ruta, boton, `wire:*`, metodo probable, campo o modelo.
- Usar `rg` antes de abrir archivos completos.
- Responder con `Encontrado`, `Componente`, `Vista` y `Accion` si aplica.
