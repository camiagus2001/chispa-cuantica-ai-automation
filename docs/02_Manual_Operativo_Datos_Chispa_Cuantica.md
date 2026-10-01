# 02 — Manual operativo de datos

## Tablas

### Clientes
Identidad del cliente y vínculo con reservas.

### Cámaras
Catálogo de servicios/cámaras.

### Disponibilidad
Cámara, fecha, hora y estado de disponibilidad.

### Reservas
Cliente, cámara, fecha/hora, estado, confianza IA, mensaje, respuesta, aprobación y ejecución n8n.

### Logs
Tipo, nodo, código de error, descripción, reintento, ejecución y reserva asociada.

## Estados principales

`Pendiente → Procesando IA → Esperando aprobación → Aprobado por humano / Rechazado por humano`

También se contemplan `Sin disponibilidad`, `Error` y `Cancelado`.

## JSON — interpretación

```json
{
  "intencion": "crear_reserva",
  "camara": "Etereum",
  "fecha": "2026-10-03",
  "hora": "18:00",
  "email_cliente": "cliente@example.com",
  "datos_faltantes": [],
  "confianza": 0.98
}
```

## JSON — datos faltantes

```json
{
  "intencion": "crear_reserva",
  "camara": "Etereum",
  "fecha": null,
  "hora": null,
  "email_cliente": "cliente@example.com",
  "datos_faltantes": ["fecha", "hora"],
  "confianza": 0.95
}
```

La validación final de cámara, fecha y hora se realiza de forma determinista en n8n.
