# 03 — Matriz de costos y decisión de modelo

| Tarea | Estrategia | Justificación |
|---|---|---|
| Clasificación de intención | Modelo liviano | Salida corta y estructurada |
| Extracción de cámara/fecha/hora | Modelo liviano | No requiere razonamiento extenso |
| Redacción breve | Modelo liviano | Mensajes simples y acotados |
| Lectura extensa / casos complejos | Modelo de mayor capacidad | Solo cuando la dificultad lo justifique |
| Procesos masivos no interactivos | Batch | Reduce el costo relativo frente a llamadas síncronas individuales |
| Validaciones de negocio | Sin LLM | Las resuelve n8n/Airtable |

## Decisión aplicada

Para este flujo se utilizó **GPT-4o-mini** en tareas simples de clasificación, extracción y redacción. La disponibilidad y las decisiones críticas no dependen del modelo.

## Optimización

- limitar el contexto enviado;
- pedir JSON estructurado;
- evitar llamadas de IA para validaciones deterministas;
- usar un modelo de mayor capacidad solo por excepción;
- considerar Batch para volumen alto no interactivo.

> Los valores exactos de precios pueden cambiar. La matriz se enfoca en la decisión arquitectónica y el ahorro relativo por no escalar todas las tareas al modelo más costoso.
