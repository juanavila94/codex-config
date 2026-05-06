---
name: laravel-livewire-investigation
description: Usar cuando el usuario pregunte por pantallas, formularios, botones, acciones, inputs, wire:model, wire:click, wizards, componentes Livewire, Blade asociado o flujos interactivos en Laravel Livewire. Ejemplos: "dónde está esta pantalla", "qué botón dispara esto", "dónde se carga este formulario", "por qué no actualiza el input", "dónde se guarda este campo", "qué componente usa esta vista".
---

# Investigación Livewire / Blade con bajo consumo de tokens

Objetivo: encontrar pantalla, componente, acción o formulario Livewire sin recorrer todo el proyecto.

## Regla principal

No abrir todos los componentes Livewire.

Primero buscar por:

1. texto visible,
2. ruta,
3. nombre de botón,
4. `wire:click`,
5. `wire:model`,
6. método probable,
7. campo,
8. modelo relacionado.

Usar `rg` antes de abrir archivos completos.

## Estilo de respuesta

Responder corto.

Formato:

```text
Encontrado:
- X.

Componente:
- app/Livewire/...

Vista:
- resources/views/...

Acción:
- método Y.

Hacer:
- Z.
