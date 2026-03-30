# 🛠️ Stack Tecnológico 2026: Negocios Digitales Autónomos

Este documento lista las herramientas esenciales para ejecutar las skills detalladas en este directorio, clasificadas por su función operativa.

---

### 1. Cerebro y Orquestación (AI Logic)
* **Modelos de Lenguaje (LLMs):** * *Razonamiento Complejo:* Gemini 1.5 Pro / GPT-5 (Preview).
    * *Velocidad y Bajo Costo:* Gemini 1.5 Flash / Claude 3 Haiku.
* **Frameworks de Agentes:** * **CrewAI / LangGraph:** Para coordinar múltiples agentes con roles distintos.
    * **PydanticAI:** Para asegurar que la IA responda siempre con datos validados (Type-safe).

### 2. Memoria y Datos (Vector Storage)
* **Pinecone / Weaviate:** Bases de datos vectoriales para memoria de largo plazo y RAG.
* **Supabase:** Para gestión de base de datos relacional y autenticación de usuarios.
* **Unstructured.io:** Para procesar PDFs, imágenes y excels y convertirlos en conocimiento para la IA.

### 3. El Sistema Nervioso (Automation & APIs)
* **n8n (Self-hosted):** La opción preferida para máxima privacidad y flujos complejos con nodos de IA.
* **Make.com:** Para integraciones rápidas con más de 1,000 aplicaciones SaaS.
* **Stripe API:** Para la gestión autónoma de cobros, suscripciones y "metered billing" (pago por uso).

### 4. Seguridad y Observabilidad (Guardrails)
* **LangSmith / Arize Phoenix:** Para rastrear cada paso del pensamiento de la IA y detectar dónde falló.
* **NeMo Guardrails (NVIDIA):** Para crear "vallas de seguridad" que impidan temas prohibidos o fugas de datos.
* **Helicone:** Para monitorear el gasto de tokens y latencia en tiempo real.

### 5. Interfaz y Entrega (Frontend)
* **Vercel / Next.js:** Para despliegue de aplicaciones web ultra rápidas.
* **Typebot / Landbot:** Para interfaces de chat estructuradas que alimentan a la IA.
* **WhatsApp Business API (vía Twilio o Gupshup):** Para negocios que operan 100% por mensajería.

---

## 📈 Matriz de Selección

| Necesidad | Herramienta Recomendada | Por qué |
| :--- | :--- | :--- |
| **MVP Rápido** | Make + OpenAI + Airtable | Velocidad de despliegue. |
| **Escala Industrial** | n8n + Gemini Pro + Pinecone | Control total y costos optimizados. |
| **Privacidad Máxima** | Local LLM (Ollama) + PostgreSQL | Datos nunca salen de tu servidor. |