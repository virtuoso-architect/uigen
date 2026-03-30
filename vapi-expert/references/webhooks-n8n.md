# Webhooks VAPI + Automatización en n8n

## Evento principal: end-of-call-report
- Se dispara cuando la llamada termina (por cualquier razón).
- Contiene: `summary`, `transcript`, `recordingUrl`, `analysis` (del analysisPlan), `call.id`, `customer.number`.

## Validación de seguridad
```javascript
// En el nodo Code de n8n
const secret = $input.first().json.headers['x-vapi-secret'];
if (secret !== process.env.VAPI_WEBHOOK_SECRET) {
  throw new Error('Unauthorized webhook');
}
```

## Flujo típico en n8n post-llamada
```
[Webhook: POST /vapi-call-ended]
    → [IF: event = "end-of-call-report"]
    → [Extract: analysis.interes, analysis.cita_agendada, summary, recordingUrl]
    → [GHL: Update Contact + Add Note]
    → [IF: cita_agendada = true]
         → [GHL/Cal.com: Create Appointment]
    → [IF: interes = "alto"]
         → [Notify Vendedor: Slack/WhatsApp]
```

## analysisPlan — Extracción estructurada
```json
{
  "analysisPlan": {
    "structuredDataPrompt": "Extrae del resumen: interes (alto/medio/bajo), cita_agendada (true/false), nombre_contacto, telefono",
    "structuredDataSchema": {
      "type": "object",
      "properties": {
        "interes": { "type": "string", "enum": ["alto", "medio", "bajo"] },
        "cita_agendada": { "type": "boolean" },
        "nombre_contacto": { "type": "string" }
      }
    }
  }
}
```
