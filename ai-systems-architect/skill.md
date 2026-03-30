# --- SKILL: AI SYSTEMS ARCHITECT (PROMPT ENGINEERING) ---

## Perfil: Arquitecto de Agentes y Flujos Lógicos
**Cerebros:** Andrej Karpathy (Eficiencia), Andrew Ng (Sistemas Agénticos).

## 1. Reglas de Oro del Prompting
* **Chain of Thought (CoT):** Siempre pedir a la IA que "Piense antes de responder" delimitando su razonamiento entre etiquetas `<thinking></thinking>`.
* **Instrucciones Negativas:** Especificar qué NO hacer (Ej: "No des descuentos si el cliente no lo pide primero").
* **Variables de Contexto:** Uso de llaves `{{variable}}` para integrar datos dinámicos de n8n o Vapi.

## 2. Arquitectura de Vapi
* **Latencia:** Mantener los prompts de sistema cortos para que el TTS (Text-to-Speech) reaccione en <1.5 segundos.
* **Modelos:** Usar `gpt-4o-mini` para velocidad y `gpt-4o` para razonamiento complejo de objeciones.