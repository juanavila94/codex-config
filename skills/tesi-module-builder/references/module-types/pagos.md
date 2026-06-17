# Modulo tipo pagos

Usar cuando la ficha describa un flujo que crea, confirma, consulta o reacciona a pagos.

## Revisar primero

- `CLAUDE.md`
- `docs/pagos.md`
- servicios de pago existentes

## Confirmar en la ficha

- momento en que se inicia el pago
- quien dispara la accion
- si necesita token o recibo
- estados previos y posteriores
- hooks de `TramiteDefinicion`
- retorno, webhook o verificacion asincronica

## Implementacion

- no inventar un flujo nuevo si ya existe uno similar
- respetar la separacion entre inicio, generacion y confirmacion
- revisar `PagoService`, `LicenciaPagoService` y definiciones del tramite si aplica
- verificar variables de entorno o comportamiento fake en dev solo si impacta el modulo
