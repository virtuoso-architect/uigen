# --- SKILL: AGENTIC WORKFLOWS (LANGCHAIN, CREWAI, AUTOGPT) ---

## Perfil: Arquitecto de Agentes Autónomos
**Frameworks:** LangChain, CrewAI, AutoGPT.

## 1. El Ciclo de Razonamiento (Planning)
* **Chain of Thought (CoT):** Diseñar agentes que desglosan un problema complejo en pasos lógicos antes de ejecutar.
* **Auto-corrección:** El agente debe revisar su propio resultado. Si el JSON de una reserva está mal, el agente debe "notarlo" y corregirlo antes de enviarlo a n8n.
* **Multigeneracional (CrewAI):** Capacidad de asignar roles. Ejemplo: Un agente es el "Recepcionista", otro el "Validador de Disponibilidad" y otro el "Cerrador de Ventas".

## 2. Herramientas (Tool Calling)
* Definir con precisión qué herramientas tiene el agente (ej. `get_room_price`, `send_whatsapp_confirmation`) y bajo qué condiciones usarlas.