---
name: laravel-model-investigation
description: >
  Usar cuando el usuario pregunte que hace un modelo Laravel,
  donde se usa o como impacta en otros modelos relacionados.
---

# Modelo Laravel

Objetivo: entender uso e impacto leyendo lo minimo.

Reglas:

- Buscar primero con `rg`; abrir archivos solo si muestran impacto real.
- Excluir `tests/` salvo pedido explicito.
- Priorizar `app/Models`, migraciones, Livewire, controllers y views.
- Seguir relaciones solo con evidencia concreta: Eloquent, foreign keys, `with()`, `whereHas()`, joins o acceso real.
- Cortar cuando el impacto ya este claro.
