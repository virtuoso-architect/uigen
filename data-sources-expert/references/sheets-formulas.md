# Fórmulas Avanzadas de Google Sheets

## QUERY (SQL-like)
```
=QUERY(Datos!A:F, "SELECT A, B, C WHERE D = 'activo' ORDER BY E DESC", 1)
```
- Filtrado, ordenamiento y selección de columnas en una sola fórmula.
- El último argumento (1) indica que la primera fila es encabezado.

## ARRAYFORMULA
```
=ARRAYFORMULA(IF(A2:A="", "", B2:B * C2:C))
```
- Procesa columnas enteras sin arrastrar fórmulas.
- Indispensable para hojas que reciben datos de API (n8n no puede arrastrar).

## XLOOKUP
```
=XLOOKUP(valor_buscado, rango_búsqueda, rango_retorno, "No encontrado", 0)
```
- Reemplaza VLOOKUP con búsqueda bidireccional y manejo de errores nativo.
- El 4to argumento es el valor si no encuentra; el 5to es el modo de coincidencia.

## IMPORTRANGE con QUERY
```
=QUERY(IMPORTRANGE("URL_hoja", "Hoja1!A:F"), "SELECT Col1, Col2 WHERE Col3 > 100", 1)
```
- Importa y filtra datos de otra hoja en una sola operación.
- Latencia alta — usar solo para dashboards, no para hojas operativas con n8n.

## Buenas prácticas para integración con n8n
- Nombres de columna en `snake_case` (sin espacios, sin acentos)
- Sin filas o columnas fusionadas
- Una hoja por entidad (no mezclar Clientes con Pedidos en la misma hoja)
- Columna ID en posición A siempre
