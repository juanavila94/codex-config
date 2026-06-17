---
name: laravel-field-investigation
description: >
  Usar cuando el usuario pregunte donde se carga, usa, valida,
  modifica o impacta un campo en Laravel.
---

# Campo Laravel

Objetivo: seguir el uso real de un campo sin exploracion masiva.

Reglas:

- Buscar primero con `rg`; no abrir archivos completos al inicio.
- Excluir `tests/` salvo pedido explicito.
- Seguir: nombre del campo -> tabla/modelo -> validaciones -> formulario/pantalla -> relaciones si impactan.
- Cortar cuando el campo deje de propagarse.
