---
name: tesi-module-builder
description: Crear o planificar modulos nuevos para tesi-v3 a partir de una ficha tecnica en Markdown. Usar cuando Codex deba leer una especificacion funcional, respetar el proyecto padre, seguir CLAUDE.md y referencias del repo, detectar faltantes, buscar un patron similar y ejecutar por etapas cambios en Laravel, Livewire, tramites, backoffice, pagos o colas.
---

# TESI Module Builder

Objetivo: convertir una ficha tecnica `.md` en un plan de ejecucion corto y, si corresponde, en una implementacion alineada con `tesi-v3`.

## Bootstrap obligatorio

Antes de planificar o crear archivos:

- Leer `AGENTS.md` del proyecto padre.
- Leer cada archivo listado ahi.
- Leer `CLAUDE.md` completo.
- Si el modulo toca tramites, leer `docs/tramites.md`.
- Si toca pagos, leer `docs/pagos.md`.
- Si toca permisos o admins, buscar documentacion local relevante antes de decidir.

Si una referencia del proyecto contradice la ficha tecnica, priorizar el proyecto padre y marcar la diferencia.

## Entrada esperada

Esperar una ficha tecnica en Markdown. Si la ficha es pobre o incompleta, usar [references/module-spec-template.md](references/module-spec-template.md) como estructura objetivo.

Campos minimos para ejecutar sin friccion:

- objetivo
- tipo de modulo
- actores o roles
- entidades o datos
- pantallas o pasos
- acciones
- estados si aplica
- permisos
- archivos afectados o punto de registro si ya se conoce
- criterios de aceptacion

## Clasificar el modulo

Clasificar rapido:

- `tramite_nuevo`
- `backoffice`
- `pagos`
- `cola_presencial`
- `catalogo_crud`
- `integracion_transversal`

Usar la referencia adecuada:

- [references/module-types/tramite.md](references/module-types/tramite.md)
- [references/module-types/backoffice.md](references/module-types/backoffice.md)
- [references/module-types/pagos.md](references/module-types/pagos.md)

## Flujo de trabajo

1. Leer ficha tecnica.
2. Normalizarla en `requerido`, `opcional`, `faltante`, `dudas`.
3. Leer contexto del proyecto padre.
4. Buscar un patron similar en el repo antes de diseñar nada nuevo.
5. Armar plan corto con archivos a crear, archivos a tocar y registros necesarios.
6. Ejecutar por bloques chicos.
7. Verificar sintaxis, registros y puntos de integracion.

## Buscar patron real

Antes de implementar:

- Buscar modulos parecidos con `rg`.
- Priorizar estructura, nombres y convenciones ya existentes.
- No inventar una arquitectura nueva si el repo ya resuelve algo similar.
- Cortar la investigacion cuando el patron suficiente ya este claro.

## Reglas de implementacion

- Implementar primero estructura y registros, despues logica y vistas.
- Tocar solo archivos relacionados al modulo.
- No recorrer todo el proyecto.
- No abrir tests salvo pedido explicito o necesidad real.
- Si faltan definiciones criticas, frenar antes de crear codigo incierto.
- Si la ficha pide algo que rompe el contrato del proyecto padre, marcarlo y proponer el ajuste minimo.

## Respuesta esperada

Responder corto. Formato preferido:

```text
Contexto:
- tipo de modulo
- referencias leidas

Faltantes:
- X

Patron:
- modulo similar

Plan:
- crear A
- tocar B

Riesgo:
- X

Siguiente:
- paso 1
- paso 2
```

## Regla de corte

Detener y pedir confirmacion corta solo si:

- faltan definiciones criticas,
- hay dos patrones validos con consecuencias distintas,
- o el cambio puede romper una convencion fuerte del proyecto.

## Referencias

- [references/module-spec-template.md](references/module-spec-template.md)
- [references/module-checklist.md](references/module-checklist.md)
- [references/module-types/tramite.md](references/module-types/tramite.md)
- [references/module-types/backoffice.md](references/module-types/backoffice.md)
- [references/module-types/pagos.md](references/module-types/pagos.md)
