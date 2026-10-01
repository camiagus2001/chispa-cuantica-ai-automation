# 04 — Seguridad y resiliencia

## Minimización de datos

El flujo procesa solamente la información necesaria para gestionar una reserva: remitente, cámara, fecha, hora, mensaje y estado operativo.

## Credenciales

Las API Keys, tokens OAuth y contraseñas se administran desde las credenciales de n8n y no se publican dentro de prompts, expresiones, capturas ni documentación.

El workflow incluido en este repositorio está sanitizado: las referencias sensibles de credenciales, webhook y correo del aprobador fueron reemplazadas por valores neutros.

## Error Handling

Los nodos críticos contemplan:

- `Retry On Fail`;
- `Error Output`;
- registro de errores en Airtable Logs;
- códigos de error identificables.

Durante las pruebas se forzó un fallo de OpenAI y se registró como `OPENAI_API_ERROR`.

## Human-in-the-loop

Una reserva disponible se crea en estado `Esperando aprobación`. La confirmación al cliente se ejecuta únicamente después de recibir una decisión humana.

## Respuestas organizadas

Los nodos de consulta y datos faltantes responden sobre el mismo hilo mediante `Thread ID` y `Message ID`.

## Validación determinista

Cámara, fecha y hora se validan con tres condiciones `notEmpty` combinadas mediante `AND`, evitando que una salida incompleta del modelo continúe hacia la búsqueda de disponibilidad.

## Prevención de bucles

El Gmail Trigger incorpora un filtro de exclusión para los mensajes emitidos por la cuenta del propio sistema:

```text
in:inbox -from:approver@example.com
```

En la versión pública se usa un correo neutro para mantener anonimizada la cuenta real de operación.
