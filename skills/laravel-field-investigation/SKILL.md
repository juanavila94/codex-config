---
name: laravel-field-investigation
description: >
  Usar cuando el usuario pregunte dónde se carga, usa, valida, modifica,
  muestra o impacta un campo en un proyecto Laravel.
  Ejemplos: dónde se carga el dni, qué usa el campo email,
  qué impacto tiene cambiar estado, dónde validan teléfono,
  qué pasa si cambio este campo.
---

# Investigación de campos Laravel con bajo consumo de tokens

Objetivo: encontrar el uso real de un campo Laravel evitando exploración masiva del proyecto.

## Regla principal

No abrir archivos completos al inicio.

Primero buscar ocurrencias exactas y variantes probables con `rg`.  
Abrir solo los archivos donde haya coincidencias relevantes.

## Restricción importante

No buscar ni abrir tests.

El proyecto no utiliza tests como fuente útil de investigación funcional.

Excluir siempre:

- `tests/`
- Pest
- PHPUnit

Salvo que el usuario lo pida explícitamente.

## Flujo de trabajo

1. Identificar el campo inicial.
2. Buscar variantes del nombre del campo.
3. Detectar en qué tabla/modelo vive.
4. Revisar validaciones, formularios y pantallas.
5. Si el campo se propaga por relaciones, seguir la cadena relacional.
6. Cortar cuando el campo deje de propagarse.

## Búsqueda inicial de campo

Ejemplo para DNI:

```bash
rg "dni|DNI|documento|numero_documento|nro_documento|cuil|cuit" app database resources
```

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
```
