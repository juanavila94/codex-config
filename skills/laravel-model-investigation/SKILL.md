---
name: laravel-model-investigation
description: Usar cuando el usuario pregunte qué hace un modelo Laravel, dónde se usa, qué pantallas/flujos dependen de él, o cómo impacta en otros modelos relacionados. Ejemplos: "qué hace el modelo User", "cómo afecta User a Personas", "qué usa este modelo", "qué pantallas dependen de esto".
---

# Investigación de modelos Laravel con bajo consumo de tokens

Objetivo: entender el uso e impacto de un modelo Laravel leyendo la menor cantidad posible de archivos.

## Regla principal

No recorrer todo el proyecto.

Primero usar `rg` para encontrar referencias concretas.  
Abrir archivos solo cuando las coincidencias indiquen impacto real.

## Restricción importante

No buscar ni abrir tests.

El proyecto no utiliza tests como fuente útil de investigación funcional.

Excluir siempre:

- `tests/`
- Pest
- PHPUnit

Salvo que el usuario lo pida explícitamente.

## Flujo de trabajo

1. Identificar el modelo inicial.
2. Buscar referencias directas del modelo.
3. Detectar tabla, foreign keys y variables comunes.
4. Revisar relaciones directas.
5. Seguir cadenas relacionales solo si son relevantes.
6. Cortar la exploración cuando el impacto ya esté claro.

## Búsqueda inicial de modelo

Ejemplo para `User`:

```bash
rg "User|users|user_id|\$user" app database resources

## Estilo de respuesta

Responder corto y directo.

Formato preferido:

```text
Encontrado:
- X.

Impacto:
- Y.

Hacer:
- Z.

Duda:
- ¿A sí/no?