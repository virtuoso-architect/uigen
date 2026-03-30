# Patrones n8n para Integración con Datos

## Patrón 1 — Deduplicación (Upsert)
```
[Webhook/Trigger]
    → [Airtable/Sheets: Buscar por email o ID]
    → [IF: ¿existe?]
         ✅ SÍ → [Update registro existente]
         ❌ NO → [Create nuevo registro]
```
- En Airtable: usar `filterByFormula` en el nodo de búsqueda.
- En Sheets: usar nodo con `MATCH` o `XLOOKUP` vía fórmula, o buscar en columna con `Get Row`.

## Patrón 2 — Lectura Masiva Paginada (Airtable)
```
[Airtable: List Records (página 1)]
    → [IF: offset existe]
         ✅ SÍ → [Airtable: List Records (offset)]  ← Loop
         ❌ NO → [Procesar todos los registros]
```

## Patrón 3 — Sincronización Delta (Sheets)
```
[Schedule Trigger: cada hora]
    → [Sheets: Get Rows donde last_modified > {{ $now.minus(1, 'hour') }}]
    → [Procesar solo registros modificados]
    → [Actualizar sistema destino]
```
- Requiere columna `last_modified` con timestamp en la hoja.

## Patrón 4 — Escritura Masiva con Rate Limit (Airtable)
```
[Preparar lista de registros]
    → [SplitInBatches: tamaño 10]
    → [Airtable: Create/Update batch]
    → [Wait: 1s]  ← Respeta el rate limit de 5 req/s
    → [Loop hasta terminar]
```

## Errores comunes y soluciones
| Error | Causa | Solución |
|---|---|---|
| 429 Too Many Requests | Rate limit excedido | Añadir Wait node de 250-500ms |
| Column not found | Encabezado con espacio o acento | Renombrar a snake_case |
| Record not found | RECORD_ID incorrecto | Verificar que viene del campo correcto, no de Autonumber |
