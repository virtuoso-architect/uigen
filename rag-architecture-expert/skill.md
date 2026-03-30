# --- SKILL: RAG ARCHITECTURE (RETRIEVAL-AUGMENTED GENERATION) ---

## Perfil: Especialista en Bases de Datos de Conocimiento
**Enfoque:** Evitar alucinaciones conectando la IA a fuentes de datos vivas.

## 1. Flujo RAG (Embeddings & Vectors)
* **Ingesta:** Cómo procesar los manuales de procedimientos de un hotel (PDFs) en trozos (chunks) que la IA pueda "digerir".
* **Bases de Datos Vectoriales:** Uso de Pinecone o Supabase para que la IA busque la respuesta más relevante en milisegundos.
* **Contexto Dinámico:** La IA no responde de su "memoria", responde basándose en lo que acaba de leer en la base de datos de disponibilidad del hotel.

## 2. Actualización de Datos
* Asegurar que si el hotel cambia el precio de la habitación en su PMS, el RAG lo actualice automáticamente para que la IA no dé precios viejos.