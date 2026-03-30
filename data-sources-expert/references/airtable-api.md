# API de Airtable — Referencia Técnica

## Rate Limits
- **5 requests/segundo** por base. Excederlo genera error `429 Too Many Requests`.
- En n8n: añadir nodo `Wait` de 250ms entre iteraciones para flujos de alta frecuencia.

## Batching (creación/actualización masiva)
- Máximo **10 registros** por POST o PATCH.
- Para listas grandes: usar `SplitInBatches` en n8n con tamaño de batch = 10.

## Paginación (lectura masiva)
- La API devuelve máximo 100 registros por llamada.
- Usar el campo `offset` de la respuesta para paginar:
```json
{ "records": [...], "offset": "abc123" }
```
- En n8n: loop con condición `IF offset exists → GET next page`.

## filterByFormula (deduplicación y búsqueda)
```
filterByFormula=({email}="cliente@ejemplo.com")
```
- Devuelve registros que cumplen la condición.
- Usar para verificar existencia antes de crear (patrón upsert).

## Attachments
- No se suben binarios directamente — se requiere URL pública accesible.
- Formato: `[{ "url": "https://...", "filename": "archivo.pdf" }]`

## RECORD_ID() y Autonumber
- `RECORD_ID()` es el identificador único nativo de Airtable (formato `rec...`).
- `Autonumber` es un campo numérico secuencial — útil como ID legible.
- Usar siempre uno de los dos como llave primaria en integraciones.
