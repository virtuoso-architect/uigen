# Guía de IA y RAG en n8n

- **Arquitectura**: El nodo AI Agent debe tener sub-nodos conectados (LLM, Memory, Tools).
- **Regla Crítica**: El modelo de embeddings DEBE ser el mismo para ingestión y consulta.
- **Memoria**: Usa "Window Buffer Memory" para conversaciones con contexto persistente.
