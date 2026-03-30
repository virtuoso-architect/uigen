# GHL Setup - Virtuoso IA + Cloudbeds Connector
# Propiedad: Virtuoso Solucion Partner Account
# Version: 1.0 | Fecha: 2026-03-30
# ============================================================

## INDICE
1. Custom Fields requeridos (Date type)
2. Configuracion del Workflow Trigger
3. Accion Webhook hacia n8n
4. Configuracion de OAuth2 en n8n (Cloudbeds)
5. Room Type IDs del Sandbox
6. Checklist de Activacion

---

## 1. CUSTOM FIELDS REQUERIDOS EN GHL

Ir a: Settings > Custom Fields > Add Field

### 1.1 Campo: Checkin Date (OBLIGATORIO)
```
Nombre del campo : Checkin Date
Field Key        : checkin_date
Tipo             : Date
Formato          : YYYY-MM-DD
Grupo            : Reservation Details
Obligatorio      : SI
Descripcion      : Fecha de entrada al hotel (formato ISO 8601)
```

### 1.2 Campo: Checkout Date (OBLIGATORIO)
```
Nombre del campo : Checkout Date
Field Key        : checkout_date
Tipo             : Date
Formato          : YYYY-MM-DD
Grupo            : Reservation Details
Obligatorio      : SI
Descripcion      : Fecha de salida del hotel (formato ISO 8601)
```

### 1.3 Campo: Room Type (OBLIGATORIO)
```
Nombre del campo : Room Type
Field Key        : room_type
Tipo             : Dropdown
Opciones         : DQ | DK
Etiquetas        : Deluxe Queen | Deluxe King
Grupo            : Reservation Details
Obligatorio      : SI
Descripcion      : Tipo de habitacion seleccionada
```

### 1.4 Campo: Adults Count (RECOMENDADO)
```
Nombre del campo : Adults Count
Field Key        : adults_count
Tipo             : Number
Valor por defecto: 1
Min              : 1
Max              : 4
Grupo            : Reservation Details
Obligatorio      : NO
Descripcion      : Numero de adultos en la reserva
```

### 1.5 Campo: Children Count (OPCIONAL)
```
Nombre del campo : Children Count
Field Key        : children_count
Tipo             : Number
Valor por defecto: 0
Min              : 0
Max              : 4
Grupo            : Reservation Details
Obligatorio      : NO
Descripcion      : Numero de menores en la reserva
```

### 1.6 Campo: Cloudbeds Reservation ID (SOLO LECTURA)
```
Nombre del campo : Cloudbeds Reservation ID
Field Key        : cloudbeds_reservation_id
Tipo             : Text
Grupo            : Reservation Details
Obligatorio      : NO
Descripcion      : ID generado por Cloudbeds tras la reserva exitosa.
                   Este campo es llenado automaticamente por n8n.
                   NO debe ser editado manualmente.
```

### 1.7 Campo: Reservation Status (SOLO LECTURA)
```
Nombre del campo : Reservation Status
Field Key        : reservation_status
Tipo             : Dropdown
Opciones         : pending | confirmed | failed | cancelled
Grupo            : Reservation Details
Obligatorio      : NO
Descripcion      : Estado de la reserva sincronizado desde Cloudbeds.
```

---

### ALERTA - LEADS MUERTOS: Causas mas comunes
```
CAUSA 1: checkin_date o checkout_date en formato incorrecto
  - GHL envia fechas como MM/DD/YYYY cuando el campo es tipo Text
  - SOLUCION: Usar campo tipo "Date" -- el nodo Code en n8n normaliza automaticamente

CAUSA 2: room_type con valor libre (no dropdown)
  - Si el campo es tipo Text, el usuario puede escribir "queen", "DQ ", "deluxe queen"
  - SOLUCION: Usar campo tipo Dropdown con opciones fijas DQ y DK

CAUSA 3: email vacio o invalido
  - Cloudbeds rechaza la reserva si el email no tiene formato valido
  - SOLUCION: Activar validacion de email en el formulario de GHL

CAUSA 4: checkout <= checkin
  - El nodo Code lanza error y GHL recibe HTTP 422
  - SOLUCION: Validar en el formulario GHL que checkout > checkin
```

---

## 2. CONFIGURACION DEL WORKFLOW TRIGGER EN GHL

Ir a: Automation > Workflows > Create New Workflow

### 2.1 Trigger recomendado: Form Submitted
```
Trigger Type : Form Submitted
Filtro       : Form is [Nombre del formulario de reserva]
Condicion    : checkin_date is not empty
               AND checkout_date is not empty
               AND room_type is not empty
               AND email is not empty
```

### 2.2 Trigger alternativo: Pipeline Stage Changed
```
Trigger Type : Pipeline Stage Changed
Pipeline     : Reservas Virtuoso
Stage        : Listo para Reservar
Filtro       : checkin_date is not empty AND checkout_date is not empty
```

### 2.3 Configuracion importante del Trigger
```
Allow Re-enrollment  : NO (evitar reservas duplicadas)
                       EXCEPCION: Si el Pipeline Stage puede volver atras,
                       considerar activarlo solo para leads con status "failed"

Run Only Once        : SI (primera vez que cumple la condicion)
Execution Window     : 24/7 (las reservas pueden llegar a cualquier hora)
```

---

## 3. ACCION WEBHOOK EN GHL (hacia n8n)

Dentro del Workflow, agregar accion:

### 3.1 Configuracion del Webhook
```
Tipo de Accion : Outgoing Webhook
Metodo HTTP    : POST          <- SIEMPRE POST salvo indicacion expresa
URL            : https://[tu-instancia-n8n].com/webhook/cloudbeds-reserva-ghl
Content-Type   : application/json
Timeout        : 30 segundos
```

### 3.2 Body del Webhook (JSON)
```json
{
  "first_name"  : "{{contact.first_name}}",
  "last_name"   : "{{contact.last_name}}",
  "email"       : "{{contact.email}}",
  "checkin"     : "{{contact.checkin_date}}",
  "checkout"    : "{{contact.checkout_date}}",
  "room_type"   : "{{contact.room_type}}",
  "adults"      : "{{contact.adults_count}}",
  "children"    : "{{contact.children_count}}",
  "contact_id"  : "{{contact.id}}",
  "property_id" : "PROPERTY_ID_SANDBOX"
}
```

NOTA: Reemplazar PROPERTY_ID_SANDBOX con el ID real de la propiedad
en Cloudbeds. Ver seccion 5 de este documento.

### 3.3 Acciones post-Webhook (condicional por respuesta)

Agregar un Wait Step de 5 segundos ANTES del webhook para asegurar
que todos los campos del contacto esten guardados en GHL.

```
Wait Step    : 5 segundos
Webhook POST : [configuracion anterior]
```

Luego agregar IF/Else segun el response del webhook:
```
IF   : {{ webhookResponse.success }} == true
  - Accion: Actualizar campo cloudbeds_reservation_id
            con {{ webhookResponse.reservationID }}
  - Accion: Actualizar campo reservation_status = "confirmed"
  - Accion: Mover pipeline a stage "Reserva Confirmada"
  - Accion: Enviar SMS/Email de confirmacion al huesped

ELSE (success == false o timeout):
  - Accion: Actualizar campo reservation_status = "failed"
  - Accion: Mover pipeline a stage "Error en Reserva"
  - Accion: Notificar al equipo via Slack/Email interno
  - Accion: Agregar nota al contacto con el error recibido
```

---

## 4. CONFIGURACION DE OAUTH2 EN N8N (Cloudbeds Sandbox)

Ir en n8n: Settings > Credentials > Add Credential > OAuth2 API

```
Nombre Credential  : Cloudbeds OAuth2 - Sandbox Virtuoso
Tipo               : OAuth2 API
Grant Type         : Authorization Code

Authorization URL  : https://hotels.cloudbeds.com/api/v1.2/oauth
Access Token URL   : https://hotels.cloudbeds.com/api/v1.2/access_token
Client ID          : virtuoso_solucion_Qk2ty4lw9HRAUYWaoKpq7eJD
Client Secret      : [Obtener desde Cloudbeds Developer Portal]
Scope              : read:reservation write:reservation read:room read:guest
Auth URI Query Params: response_type=code
Token Content-Type : application/x-www-form-urlencoded

Auth Flow          : Authorization Code (PKCE recomendado para Sandbox)
Redirect URL       : https://[tu-instancia-n8n].com/rest/oauth2-credential/callback
```

Pasos para activar el token:
1. Guardar la credencial en n8n
2. Hacer clic en "Connect my account"
3. Iniciar sesion en el portal de Cloudbeds con la cuenta Sandbox
4. Autorizar los permisos solicitados
5. Verificar en n8n que el token quedo activo (icono verde)

---

## 5. ROOM TYPE IDs DEL SANDBOX CLOUDBEDS

Los IDs de habitacion SON NUMERICOS y especificos del Sandbox.
Para obtenerlos, ejecutar esta llamada ANTES de configurar el workflow:

```
GET https://api.cloudbeds.com/api/v1.2/getRoomTypes
  ?propertyID=[PROPERTY_ID_SANDBOX]
Authorization: Bearer [access_token]
```

Respuesta esperada:
```json
{
  "success": true,
  "data": {
    "roomTypes": {
      "XXXX": {
        "roomTypeName": "Deluxe Queen",
        "roomTypeID": "XXXX",
        "roomTypeShortName": "DQ",
        "maxGuests": 2
      },
      "YYYY": {
        "roomTypeName": "Deluxe King",
        "roomTypeID": "YYYY",
        "roomTypeShortName": "DK",
        "maxGuests": 2
      }
    }
  }
}
```

Una vez obtenidos los IDs reales, actualizar el nodo Format_Fechas_YYYYMMDD
en n8n (lineas marcadas con "-- reemplazar con ID real"):

```javascript
const ROOM_TYPE_MAP = {
  'DQ': 'XXXX',   // <- ID real Deluxe Queen del Sandbox
  'DK': 'YYYY'    // <- ID real Deluxe King del Sandbox
};
```

---

## 6. CHECKLIST DE ACTIVACION

### Pre-activacion
- [ ] Credencial OAuth2 de Cloudbeds creada y token activo en n8n
- [ ] Custom Fields creados en GHL (seccion 1)
- [ ] ROOM_TYPE_MAP actualizado con IDs reales del Sandbox
- [ ] PROPERTY_ID_SANDBOX reemplazado con el ID real
- [ ] URL del Webhook n8n confirmada y accesible desde internet
- [ ] Formulario de GHL con validacion de campos obligatorios activa

### Prueba de humo (Test en Sandbox)
1. Crear un contacto de prueba en GHL con todos los campos completos
2. Ejecutar el Workflow manualmente desde GHL
3. Verificar en n8n que el Webhook recibio el payload
4. Verificar en n8n que el nodo Code no arrojo errores
5. Verificar en n8n que el HTTP Request retorno success:true
6. Confirmar en Cloudbeds Sandbox que aparece la reserva nueva
7. Verificar que el campo cloudbeds_reservation_id se actualizo en GHL

### Post-activacion
- [ ] Configurar Error Workflow en n8n para notificaciones por fallo
- [ ] Activar logs de ejecucion (saveExecutionProgress: true)
- [ ] Documentar URL del Webhook en el repositorio del proyecto
- [ ] Programar revision mensual del token OAuth2 (expira cada 30 dias)
