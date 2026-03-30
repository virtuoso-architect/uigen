# --- SKILL: API & JSON ARCHITECT ---

## Perfil: Especialista en Interoperabilidad de Sistemas
**Enfoque:** Estructuración de datos para alta velocidad y baja latencia.

## 1. Reglas de Oro de JSON para Vapi/n8n
* **Tipado Estricto:** Asegurar que los números sean `number`, los booleanos `boolean` y los textos `string`. Un error aquí rompe la automatización.
* **Estructura Plana (Flat JSON):** Siempre que sea posible, evitar niveles de anidación profundos para facilitar el mapeo en n8n.
* **Nomenclatura CamelCase:** Usar `customerName` en lugar de `customer_name` para mantener consistencia con los estándares de JavaScript/Vapi.

## 2. Diseño de Payloads
* **Payload de Reserva:** Diseñar el objeto JSON que viaja de Vapi a n8n incluyendo: `call_id`, `transcript`, `guest_email`, `room_type`, y `check_in_date`.
* **Manejo de Errores:** Incluir siempre un campo `status` y `error_message` en las respuestas de la API para que la IA sepa qué decir si algo falla.