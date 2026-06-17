# Modulo tipo tramite_nuevo

Usar cuando la ficha describa un tramite ciudadano con wizard y flujo de backoffice.

## Revisar primero

- `CLAUDE.md`
- `docs/tramites.md`
- un tramite similar en `app/Tramites/Tipos`

## Estructura esperada

- `app/Tramites/Tipos/MiTipo/MiTipoDefinicion.php`
- `resources/views/tramites/tipos/mi-tipo/pasos/paso-N.blade.php`
- `resources/views/tramites/tipos/mi-tipo/pdf.blade.php`
- registro en `app/Providers/AppServiceProvider.php`

## Confirmar en la ficha

- pasos del wizard
- reglas de validacion
- flujo backoffice
- roles de cada etapa
- acciones de cada etapa
- si muestra datos del ciudadano
- si genera PDF
- si inicia pagos

## No olvidar

- implementar metodos abstractos de la definicion
- revisar `camposPersona(int $paso)` si hay datos del ciudadano
- respetar `estado`, `etapa_actual` y `etapa_retorno`
- buscar una definicion similar antes de crear una nueva
