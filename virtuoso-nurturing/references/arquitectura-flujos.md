# Lógica n8n + GHL para Nurturing

## Wait Nodes
- Configura tiempos de espera relativos a la última interacción, no a la fecha de entrada al flujo
- Usa "Wait until condition" para esperar la ventana horaria óptima antes de enviar
- Nunca encadenes más de 3 mensajes sin un Wait de al menos 24h entre ellos

## Webhook GHL — Pausa Automática
```
Evento a escuchar: "Customer Replied" (Inbound Message)
Acción:
  1. Remover tag de secuencia activa (ej. "nurturing-postdemo")
  2. Añadir tag "respondió-[fecha]"
  3. Notificar al vendedor asignado (Slack/SMS/GHL interno)
  4. Detener cualquier workflow activo para ese contacto
```

## Checklist de Validación (Nodos IF en n8n)
Antes de cada mensaje, validar en secuencia:
```
IF tag = "no-contactar" → END
IF tag = "cliente-activo" → END
IF last_message_received < 48h → END + notify
IF conversation_status = "open" → END + notify
IF hour NOT IN [9-11, 15-17] → Wait until next window
IF day IN [Saturday, Sunday] → Wait until Monday
→ SEND MESSAGE
```

## Retry Logic para WhatsApp API
```
En el nodo HTTP Request (Meta/GHL API):
  - retryOnFail: true
  - maxTries: 3
  - waitBetweenTries: 60000ms (1 min)
  - Si falla 3 veces: Add tag "whatsapp-fallo" + notificar vendedor
```

## Tags de Segmentación en GHL
| Tag | Significado | Acción |
|---|---|---|
| `nurturing-nuevo` | Lead 0-48h activo | Activar Segmento A |
| `nurturing-postdemo` | Post-demo activo | Activar Segmento B |
| `nurturing-frio` | Lead 22-90 días | Activar Segmento C |
| `nurturing-propuesta` | Propuesta enviada | Activar Segmento E |
| `nurturing-pausado` | Respondió — humano tomó el control | Detener todas las secuencias |
| `no-contactar` | Opted-out o cliente perdido | Bloquear entrada a cualquier flujo |
