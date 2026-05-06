---
name: laravel-model-investigation
description: >
  Usar cuando el usuario pregunte qué hace un modelo Laravel,
  dónde se usa, qué pantallas o flujos dependen de él,
  o cómo impacta en otros modelos relacionados.
  Ejemplos: qué hace el modelo User, cómo afecta User a Personas,
  qué usa este modelo, qué pantallas dependen de esto.
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
```

## Prioridad de revisión

Priorizar:

1. `app/Models`
2. `database/migrations`
3. `app/Livewire`
4. `app/Http/Controllers`
5. `resources/views`
6. `app/Http/Requests`
7. `app/Services`, solo si aparece en búsquedas.

## Investigación relacional en cadena

Cuando el impacto entre modelos no sea directo, seguir una cadena relacional acotada.

Ejemplo:

```text
User -> Persona -> Inscripcion -> Pago
```

No investigar todos los modelos del sistema.

Continuar la cadena solo si aparecen evidencias como:

- relaciones Eloquent,
- foreign keys,
- `with()`,
- `whereHas()`,
- `load()`,
- joins,
- accessors,
- validaciones,
- filtros,
- columnas,
- lógica condicional,
- acceso a propiedades relacionadas.

## Búsquedas para relaciones

Relaciones Eloquent:

```bash
rg "belongsTo|hasMany|hasOne|belongsToMany|morph" app/Models
```

Foreign keys:

```bash
rg "foreignId|constrained|references|_id" database/migrations
```

Uso relacional en código:

```bash
rg "with\(|whereHas\(|load\(|join\(|->" app resources
```

## Regla de corte

Detener la investigación cuando:

- el modelo deja de aparecer en la cadena,
- la relación no usa datos relevantes,
- el flujo deja de depender del modelo,
- no hay referencias concretas,
- o el impacto ya quedó claro.

No seguir relaciones “por las dudas”.

## Si el usuario pregunta por pantallas o flujos

Buscar de forma acotada en Livewire, controllers y views:

```bash
rg "User|users|user_id|\$user" app/Livewire app/Http/Controllers resources/views
```

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
```

No explicar de más salvo que el usuario pida detalle.

## Respuesta esperada

Responder con:

1. qué hace el modelo,
2. dónde se usa,
3. qué relaciones relevantes aparecen,
4. qué cadena relacional se siguió,
5. dónde se cortó la investigación,
6. qué archivos conviene tocar o revisar.