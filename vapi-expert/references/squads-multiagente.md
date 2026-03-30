# Arquitectura de Squads (Multi-Agente) — VAPI

## Cuándo usar Squads
- Flujos con etapas diferenciadas (Recepcionista → Calificador → Closer)
- Cuando un agente necesita especialización profunda en un dominio específico
- Para separar lógica de IVR de lógica de conversación

## Estructura de un Squad
```json
{
  "members": [
    {
      "assistant": { "name": "Recepcionista", "...": "config" },
      "assistantDestinations": [
        {
          "assistantName": "Vendedor",
          "description": "Transferir cuando el usuario confirme interés en conocer el producto",
          "message": "Un momento, te paso con nuestro especialista."
        }
      ]
    },
    {
      "assistant": { "name": "Vendedor", "...": "config" }
    }
  ]
}
```

## La clave: `description` de destino
- El LLM del asistente actual decide cuándo transferir basándose en la `description`.
- Ser muy específico: "Transferir SOLO cuando el usuario haya confirmado que tiene más de 50 leads/mes Y está interesado en una demo."
- Vago → transferencias prematuras o nunca ocurren.

## `message` de transferencia
- Es lo que el asistente dice mientras hace la transferencia.
- Debe sonar natural: "Perfecto, te voy a conectar con [Nombre] quien puede mostrarte más detalles."
- Evitar: "Transfiriendo llamada…" — suena como sistema IVR.
