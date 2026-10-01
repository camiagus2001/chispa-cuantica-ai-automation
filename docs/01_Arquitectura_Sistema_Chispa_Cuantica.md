# 01 — Arquitectura del sistema

## Objetivo

Automatizar solicitudes de reserva recibidas por Gmail, interpretar lenguaje natural con IA, validar disponibilidad real en Airtable y mantener una aprobación humana antes de la confirmación final.

## Flujo

```text
Gmail Trigger
  ↓
OpenAI — intención + extracción
  ↓
Normalización
  ↓
Switch
  ├─ Consulta → Reply
  ├─ Irrelevante → Fin
  └─ Reserva
      ↓
    Validación cámara / fecha / hora
      ↓
    Buscar o crear cliente
      ↓
    Consultar disponibilidad
      ├─ No disponible → alternativas → Reply
      └─ Disponible → Reserva pendiente
                          ↓
                    Human-in-the-loop
                    ├─ Approve → update + reply + log
                    └─ Decline → update + reply + log
```

## Componentes

- **Trigger:** Gmail.
- **Motor IA:** OpenAI.
- **Router:** Switch de n8n.
- **Base:** Airtable.
- **HITL:** Gmail Send and Wait.
- **Salida:** Gmail Thread Reply.
- **Observabilidad:** tabla Logs + Airtable Interface.

La arquitectura visual completa también está incluida en el documento maestro de la entrega.
