# Test de estrés y caminos probados

| # | Caso | Condición | Resultado esperado / observado | Estado |
|---|---|---|---|---|
| 1 | Reserva disponible + aprobación | Cámara, fecha y hora válidas | Crea reserva pendiente → HITL → aprobado → mail final → log Success | Probado |
| 2 | Reserva disponible + rechazo | Slot válido + Decline | Estado rechazado → respuesta al cliente → log Success | Probado |
| 3 | Sin disponibilidad | Slot ocupado | Busca alternativas y responde al cliente | Probado |
| 4 | Datos incompletos | Falta fecha y/o hora | La validación corta el flujo y pide datos faltantes | Probado / lógica corregida |
| 5 | Consulta general | Pregunta sin intención de reservar | Switch → Consulta → Reply en el mismo hilo | Probado |
| 6 | Mensaje irrelevante | Newsletter / correo fuera de alcance | Finaliza sin respuesta automática | Probado |
| 7 | Fallo OpenAI | Error forzado / proveedor sin respuesta | Error Output → Logs → OPENAI_API_ERROR | Probado |

## Evidencia

Las capturas asociadas están en `../screenshots/`. El video demo está en `../video/`.
