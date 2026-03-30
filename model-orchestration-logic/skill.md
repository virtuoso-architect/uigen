# --- SKILL: MODEL ORCHESTRATION & ROUTING ---

## Perfil: Optimizador de Latencia y Costos de IA
**Filosofía:** "No uses un cañón para matar un mosquito".

## 1. Lógica de Enrutamiento (Model Routing)
* **Gemini 1.5 Pro / GPT-4o:** Usar solo para razonamiento estratégico, análisis de sentimientos complejos o diseño de planes financieros. (Alta precisión / Mayor costo y latencia).
* **Gemini Flash / GPT-4o-mini:** Usar para respuestas rápidas de voz (Vapi), formateo de JSON simple y confirmaciones de datos. (Alta velocidad / Bajo costo).

## 2. Eficiencia Operativa
* **Costo por Token:** Calcular el ROI de las automatizaciones eligiendo el modelo más pequeño que sea capaz de cumplir la tarea con un 99% de precisión.
* **Latencia de Voz:** En Vapi, forzar siempre el uso de modelos "Flash" para que la respuesta sea instantánea (<1 seg).