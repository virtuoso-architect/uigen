# Parámetros del Objeto Assistant — VAPI

## Parámetros de comportamiento
- **silenceTimeoutSeconds**: Tiempo antes de colgar por silencio (default 30s). Reducir a 10-15s para outbound agresivo.
- **maxDurationSeconds**: Duración máxima de la llamada. Establecer límite para evitar costos por llamadas infinitas.
- **firstMessageMode**: `"assistant-speaks-first"` para outbound; `"assistant-waits-for-user"` para inbound.
- **backgroundSound**: `"office"` añade realismo a llamadas de ventas. `"off"` para soporte técnico.

## Parámetros de latencia
- **responseDelaySeconds**: Añade una pausa natural antes de responder (0.5-1s recomendado para naturalidad).
- **llmRequestDelaySeconds**: Tiempo antes de hacer la petición al LLM tras detectar fin de turno.

## Configuración de interrupciones
- **interruptionsEnabled**: `true` para conversaciones naturales; `false` para scripts lineales.
- **numWordsToInterruptAssistant**: Número de palabras del usuario para interrumpir al asistente (2-3 palabras).

## Ejemplo de configuración base
```json
{
  "model": { "provider": "openai", "model": "gpt-4o-mini", "maxTokens": 150 },
  "voice": { "provider": "elevenlabs", "voiceId": "VOICE_ID" },
  "transcriber": { "provider": "deepgram", "model": "nova-2", "language": "es" },
  "firstMessageMode": "assistant-speaks-first",
  "silenceTimeoutSeconds": 15,
  "backgroundSound": "office",
  "interruptionsEnabled": true
}
```
