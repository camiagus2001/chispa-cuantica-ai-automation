# Chispa Cuántica — Sistema de Reservas IA

> Proyecto final — Ecosistema de Automatización IA Autónomo para Negocios

Sistema de reservas construido con **n8n + Gmail + OpenAI + Airtable** para interpretar solicitudes escritas en lenguaje natural, validar disponibilidad real, registrar el proceso y mantener una aprobación humana antes de confirmar una reserva.

## Concepto

El flujo separa dos responsabilidades:

- **La IA interpreta el mensaje.**
- **Las reglas del sistema deciden qué se puede hacer.**

OpenAI clasifica la intención y extrae datos. n8n valida, enruta y coordina. Airtable mantiene el estado del sistema. La disponibilidad se consulta en la base y la confirmación final pasa por **Human-in-the-loop**.

## Stack

| Capa | Herramienta | Uso |
|---|---|---|
| Orquestación | n8n | Trigger, validaciones, routing, integraciones y Error Handling |
| Entrada / salida | Gmail | Recepción y respuestas en el mismo hilo |
| IA | OpenAI | Clasificación de intención, extracción y redacción |
| Base de datos | Airtable | Clientes, cámaras, disponibilidad, reservas y logs |
| HITL | Gmail + n8n | Aprobación o rechazo antes de confirmar |
| Control | Airtable Interface | KPIs y seguimiento operativo |

## Flujo principal

```text
Gmail Trigger
    ↓
OpenAI — intención + extracción
    ↓
Normalización
    ↓
Switch
    ├── Consulta → Reply
    ├── Irrelevante → Fin
    └── Reserva
          ↓
       Validación
          ↓
       Buscar / crear cliente
          ↓
       Consultar disponibilidad
          ├── No disponible → alternativas → Reply
          └── Disponible → reserva pendiente
                               ↓
                         Human-in-the-loop
                         ├── Approve → update + reply + log
                         └── Decline → update + reply + log
```

## Modelo de datos

Airtable funciona como memoria persistente con cinco tablas relacionadas:

- **Clientes**
- **Cámaras**
- **Disponibilidad**
- **Reservas**
- **Logs**

## Pruebas realizadas

Se probaron más de cinco escenarios:

1. reserva disponible + aprobación humana;
2. reserva disponible + rechazo humano;
3. horario sin disponibilidad;
4. datos incompletos;
5. consulta general;
6. mensaje irrelevante;
7. error forzado de OpenAI.

Ver: [`tests/test-stress.md`](tests/test-stress.md)

## Dashboard de Control

**Datos y tablas — Airtable:**  
https://airtable.com/apprsQXDGVFQ4kDSl/shrXFSFUiGxymWrOZ

**Interface / Dashboard:**  
https://airtable.com/apprsQXDGVFQ4kDSl/pagMM1FktETvVJ4gH

Los KPIs principales documentados son:

- **Tasa de aprobación**
- **Volumen de salida**
- **Tasa de error**

## Documentación

| Tema | Archivo |
|---|---|
| Arquitectura | [`docs/01_Arquitectura_Sistema_Chispa_Cuantica.md`](docs/01_Arquitectura_Sistema_Chispa_Cuantica.md) |
| Datos + JSON | [`docs/02_Manual_Operativo_Datos_Chispa_Cuantica.md`](docs/02_Manual_Operativo_Datos_Chispa_Cuantica.md) |
| Costos | [`docs/03_Matriz_Costos_Chispa_Cuantica.md`](docs/03_Matriz_Costos_Chispa_Cuantica.md) |
| Seguridad + resiliencia | [`docs/04_Seguridad_Resiliencia_Chispa_Cuantica.md`](docs/04_Seguridad_Resiliencia_Chispa_Cuantica.md) |
| Dashboard | [`docs/05_Dashboard_Control.md`](docs/05_Dashboard_Control.md) |

## Respaldo técnico

- **Workflow n8n sanitizado:** [`workflow/chispa-cuantica-reservas-n8n.json`](workflow/chispa-cuantica-reservas-n8n.json)
- **Matriz de pruebas:** [`tests/test-stress.md`](tests/test-stress.md)
- **Evidencias:** [`screenshots/`](screenshots/)

El export público de n8n no contiene API Keys ni tokens. Las referencias sensibles fueron sanitizadas para el repositorio.

## Video demo

[Ver archivo del video final](video/Video_Demo_Chispa_Cuantica_FINAL.mp4)

## Autora

**Camila Liendro**  
Proyecto final — AI Automation
