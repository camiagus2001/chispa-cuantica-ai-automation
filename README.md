# Chispa Cuántica — Sistema de Reservas IA

> Proyecto final — Ecosistema de Automatización IA Autónomo para Negocios

Sistema de reservas construido con **n8n + Gmail + OpenAI + Airtable**, pensado para interpretar mensajes en lenguaje natural, consultar disponibilidad real, registrar trazabilidad y detenerse en un punto **Human-in-the-loop** antes de confirmar una reserva.

## Idea principal

El proyecto parte de una regla simple:

- **La IA interpreta el mensaje.**
- **Las reglas del sistema deciden qué se puede hacer.**

OpenAI clasifica la intención y extrae datos. n8n valida, enruta y coordina. Airtable mantiene el estado del sistema. La disponibilidad nunca se inventa con IA y una reserva disponible no se confirma sin aprobación humana.

## Stack

| Capa | Herramienta | Uso |
|---|---|---|
| Orquestación | n8n | Trigger, validaciones, routing, integraciones y Error Handling |
| Entrada / salida | Gmail | Recepción y respuestas en el mismo hilo |
| IA | OpenAI | Clasificación de intención, extracción y redacción |
| Base de datos | Airtable | Clientes, cámaras, disponibilidad, reservas y logs |
| HITL | Gmail + n8n | Aprobación / rechazo antes de confirmar |
| Observabilidad | Airtable Interface | KPIs y seguimiento operativo |

## Arquitectura

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

La base de Airtable está organizada en cinco tablas relacionadas:

- **Clientes**
- **Cámaras**
- **Disponibilidad**
- **Reservas**
- **Logs**

Los estados permiten seguir el proceso desde la solicitud inicial hasta la aprobación, rechazo, falta de disponibilidad o error.

## Human-in-the-loop

Cuando existe disponibilidad, el sistema crea una reserva en estado **Esperando aprobación** y envía una solicitud de aprobación humana. Solo después se actualiza Airtable y se responde al cliente.

## Resiliencia

El flujo incorpora:

- `Retry On Fail` en nodos críticos;
- rutas de `Error Output`;
- logs de error en Airtable;
- validación determinista de cámara, fecha y hora;
- respuestas con `Thread ID`;
- clasificación de mensajes irrelevantes;
- registro de errores como `OPENAI_API_ERROR`.

## Test de estrés

Se probaron al menos estos escenarios:

1. reserva disponible + aprobación;
2. reserva disponible + rechazo;
3. sin disponibilidad;
4. datos incompletos;
5. consulta general;
6. mensaje irrelevante;
7. error forzado de OpenAI.

Ver: [`tests/test-stress.md`](tests/test-stress.md)

## Documentación por criterio

| Criterio de la rúbrica | Documento |
|---|---|
| Arquitectura | [`docs/01_Arquitectura_Sistema_Chispa_Cuantica.md`](docs/01_Arquitectura_Sistema_Chispa_Cuantica.md) |
| Datos + JSON | [`docs/02_Manual_Operativo_Datos_Chispa_Cuantica.md`](docs/02_Manual_Operativo_Datos_Chispa_Cuantica.md) |
| Optimización de costos | [`docs/03_Matriz_Costos_Chispa_Cuantica.md`](docs/03_Matriz_Costos_Chispa_Cuantica.md) |
| Seguridad + resiliencia | [`docs/04_Seguridad_Resiliencia_Chispa_Cuantica.md`](docs/04_Seguridad_Resiliencia_Chispa_Cuantica.md) |
| Dashboard | [`docs/05_Dashboard_Control.md`](docs/05_Dashboard_Control.md) |

## Dashboard de Control

Shared View pública de Airtable:

https://airtable.com/apprsQXDGVFQ4kDSl/shrXFSFUiGxymWrOZ

La rúbrica pide como mínimo:

- **Tasa de aprobación**
- **Volumen de salida**
- **Tasa de error**

## Workflow n8n

El export técnico está incluido en:

[`workflow/chispa-cuantica-reservas-n8n.json`](workflow/chispa-cuantica-reservas-n8n.json)

Antes de publicarlo se sanitizaron:

- referencias a credenciales;
- webhook IDs;
- email personal del aprobador.

Las credenciales deben volver a configurarse localmente al importar el workflow.

## Documento maestro de entrega

Se preparó un Google Doc único con:

- propuesta;
- manual operativo;
- esquemas JSON pegados como texto;
- evidencias visuales;
- seguridad y resiliencia;
- test de estrés;
- dashboard;
- respaldo técnico.

**Documento maestro:**  
https://docs.google.com/document/d/1Xq3L-vjXL0tR0LvyBKvJiLFV-GUOwNxv7UghuRvL09I/edit

> Antes de enviar al profesor, activar **“Cualquiera con el enlace puede ver”** y probarlo en incógnito.

## Estructura del repositorio

```text
chispa-cuantica-ai-automation/
├── README.md
├── .gitignore
├── docs/
│   ├── 01_Arquitectura_Sistema_Chispa_Cuantica.md
│   ├── 02_Manual_Operativo_Datos_Chispa_Cuantica.md
│   ├── 03_Matriz_Costos_Chispa_Cuantica.md
│   ├── 04_Seguridad_Resiliencia_Chispa_Cuantica.md
│   └── 05_Dashboard_Control.md
├── workflow/
│   └── chispa-cuantica-reservas-n8n.json
├── tests/
│   └── test-stress.md
└── submission/
    ├── CHECKLIST_ENTREGA.md
    └── LINKS_PUBLICOS.md
```

## Seguridad antes de publicar

No subir nunca:

- API Keys;
- tokens OAuth;
- contraseñas;
- `.env`;
- cookies;
- capturas con emails privados sin anonimizar.

### Pendiente antes de la entrega final

El export actual confirma que:

- la validación de cámara, fecha y hora está configurada con tres condiciones `notEmpty` y AND;
- las respuestas de consulta y datos faltantes usan `Thread ID`;
- el Gmail Trigger todavía aparece con `filters: {}`.

Por eso falta agregar al Trigger una exclusión de la cuenta emisora, por ejemplo:

```text
in:inbox -from:TU_CUENTA_DEL_SISTEMA
```

y volver a exportar el JSON final si querés que esa mejora quede reflejada en el respaldo técnico.

## Autora

**Camila Liendro**  
Proyecto final — AI Automation
