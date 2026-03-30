# Tools y Functions en VAPI

## Tools nativas (sin servidor externo)
- **transferCall**: Transfiere a número de teléfono o SIP. Parámetros: `destination.type` ("number" o "sip"), `destination.number`.
- **endCall**: Cuelga la llamada. Usar con `endCallPhrases` para cierre natural.
- **dtmf**: Envía tonos DTMF (para navegar IVRs externos).

## Custom Tools (Functions con servidor)
```json
{
  "type": "function",
  "function": {
    "name": "checkAvailability",
    "description": "Verifica disponibilidad de citas. Llamar cuando el usuario quiera agendar.",
    "parameters": {
      "type": "object",
      "properties": {
        "fecha": { "type": "string", "description": "Fecha solicitada en formato YYYY-MM-DD" },
        "hora": { "type": "string", "description": "Hora solicitada en formato HH:MM" }
      },
      "required": ["fecha"]
    }
  },
  "server": { "url": "https://tu-servidor.com/availability" }
}
```

## Chaining de Tools
- Una tool puede retornar datos que el LLM usa para invocar otra tool.
- Ejemplo: `checkAvailability` → retorna slots → LLM invoca `bookAppointment` con el slot elegido.

## Buenas prácticas
- La `description` es crítica — el LLM decide cuándo llamar la tool basándose en ella.
- Ser específico en cuándo llamar y cuándo NO llamar la tool.
- Mantener el schema de parámetros simple — máximo 3-4 campos requeridos.

