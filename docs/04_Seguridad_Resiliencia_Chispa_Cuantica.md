# 04 — Seguridad y resiliencia

## Minimización de datos

El flujo procesa solo los datos necesarios para la reserva: remitente, cámara, fecha, hora, mensaje y estado operativo.

## Credenciales

No se publican API Keys, tokens OAuth ni passwords. El JSON incluido en este repositorio fue sanitizado: las referencias a credenciales y el email de aprobación fueron reemplazados.

## Error Handling

Los nodos críticos contemplan:
- `Retry On Fail`;
- `Error Output`;
- registro de errores en Airtable Logs;
- códigos de error identificables.

Se probó un fallo de OpenAI y fue registrado como `OPENAI_API_ERROR`.

## Human-in-the-loop

Una reserva disponible no se confirma automáticamente. Se crea en estado `Esperando aprobación` y se requiere una decisión humana antes de contactar al cliente con el resultado final.

## Gmail

Las respuestas de consulta y datos faltantes utilizan `Thread ID` para mantener el hilo.

## Validación determinista

Cámara, fecha y hora se validan con un IF en n8n mediante tres condiciones `notEmpty` combinadas con AND.

## Anti-loop

El export actual del Gmail Trigger no contiene todavía un filtro `-from:`. Antes de la entrega debe agregarse una exclusión de la cuenta emisora del sistema para evitar bucles de auto-respuesta.
