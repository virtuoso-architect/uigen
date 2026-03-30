# Guia de Demo: Virtuoso IA + Cloudbeds en Tiempo Real
# Audiencia: Tomadores de decision de hoteles (Director / GM / Revenue Manager)
# Duracion estimada: 20-25 minutos
# Objetivo: Que el prospecto vea el Calendario de Cloudbeds actualizarse
#           en vivo tras un flujo automatizado de Virtuoso IA / GHL
# ============================================================

## INDICE
1. Checklist de pre-demo (10 min antes)
2. Estructura del guion (6 actos)
3. Flujo tecnico paso a paso
4. Manejo de objeciones tecnicas
5. Cierre y siguiente paso

---

## 1. CHECKLIST DE PRE-DEMO (10 min antes)

### Pantallas a tener abiertas
```
Pantalla 1 (compartir): Cloudbeds Sandbox - Vista Calendario (Room Grid)
  URL: https://hotels.cloudbeds.com/connect/[sandbox-id]#/calendar

Pantalla 2 (compartir): GHL - Pipeline "Reservas Virtuoso"
  Tener visible al menos un contacto de prueba en stage "Listo para Reservar"

Pantalla 3 (privada): n8n - Workflow "Cloudbeds_Reserva_Automatica_VirtuosoIA"
  En modo escucha / ejecucion activa

Pantalla 4 (privada): Postman o browser con el formulario de test GHL
```

### Datos del contacto de prueba (pre-cargados en GHL)
```
first_name  : Ana
last_name   : Martinez
email       : ana.martinez.demo@virtuosoia.com
room_type   : DQ   (Deluxe Queen)
adults      : 2
checkin     : [fecha de hoy + 7 dias]
checkout    : [fecha de hoy + 10 dias]
```

### Verificacion de sistema (60 segundos antes)
```
- [ ] Cloudbeds Sandbox: habitaciones DQ y DK visibles en calendario
- [ ] n8n: Workflow activo (toggle ON, icono verde)
- [ ] GHL: Workflow de automatizacion ACTIVO
- [ ] Token OAuth2 de Cloudbeds: vigente (no expirado)
- [ ] Calendario de Cloudbeds: SIN reserva para Ana Martinez esas fechas
```

---

## 2. ESTRUCTURA DEL GUION (6 Actos)

### ACTO 1 - RECONEXION (1-2 min)
```
Objetivo: Validar el dolor especifico del prospecto antes de mostrar nada.

Script:
"[Nombre], antes de arrancar quiero asegurarme de conectar esto
directamente con lo que platicamos la semana pasada.

Me mencionaste que [dolor especifico -- ejemplos abajo]:
  - 'Los fines de semana no tenemos quien responda'
  - 'Perdemos reservas porque tardamos en confirmar'
  - 'El equipo de recepcion no puede dar seguimiento a cada lead'

[Confirmacion]: 'Sigue siendo ese el punto critico o cambio algo?'
```

Esperar respuesta. Escuchar. Adaptar el resto de la demo al dolor confirmado.

### ACTO 2 - EL COSTO DEL PROBLEMA (2-3 min)
```
Objetivo: Cuantificar la perdida antes de mostrar la solucion.
Tecnica: SPIN Selling -- que el prospecto calcule su propia perdida.

Script:
"Una pregunta rapida antes de la demo:
 Cuantas consultas de reserva llegan por WhatsApp o formulario web
 en un fin de semana tipico?"

[Dejar que respondan -- ej: '8 o 10']

"Y de esas 8 o 10, cuantas crees que se van si no hay respuesta
en la primera hora?"

[Dejar que respondan -- ej: '3 o 4']

"Entonces estamos hablando de 3 o 4 habitaciones que podrian ser
[precio de habitacion x noches] cada fin de semana que no se cierran
porque no hay respuesta a tiempo. Correcto?"

[Prospecto confirma su propia perdida]

"Exacto. Eso es lo que Virtuoso IA resuelve. Te lo muestro en vivo."
```

### ACTO 3 - LA VISION (1 min, sin pantalla)
```
Objetivo: Plantar la imagen mental antes del producto.
NO abrir pantallas todavia.

Script:
"Imagina por un momento que son las 11 de la noche del sabado.
Llega una consulta de reserva en tu formulario web.

Sin que nadie del equipo haga nada,
en los proximos 30 segundos:
- El sistema califica al huesped automaticamente
- Verifica disponibilidad
- Crea la reserva directamente en tu Cloudbeds
- Envia la confirmacion al huesped por WhatsApp

Todo sin intervension humana. Sin errores de captura. Sin leads perdidos.

Eso es lo que vamos a ver ahora en tiempo real."
```

### ACTO 4 - LA DEMO EN VIVO (10-12 min)
```
[Aqui inicia el flujo tecnico - ver Seccion 3 para detalle paso a paso]

Puntos clave a verbalizar durante la demo:

Al mostrar el Formulario de GHL:
"Este es el formulario que recibe la consulta del huesped.
Puede ser tu web, WhatsApp, o incluso un anuncio de Meta Ads."

Al mostrar el Flujo en n8n (breve):
"Aqui es donde Virtuoso IA procesa la informacion automaticamente.
Valida las fechas, selecciona el tipo de habitacion, y se comunica
con Cloudbeds en menos de 5 segundos."

Al mostrar Cloudbeds Calendario (el momento clave):
"Mira el calendario... [refrescar] ...ahi esta.
La reserva de Ana Martinez acaba de aparecer.
Esto paso en tiempo real, sin que nadie del equipo tocara nada."
```

### ACTO 5 - PRUEBA SOCIAL (2 min)
```
Objetivo: Anclar con evidencia numerica justo cuando la dopamina esta alta.
Presentar inmediatamente despues de que el prospecto vea el calendario.

Script:
"Implementamos exactamente esto en [hotel/cadena similar].
En las primeras 4 semanas:
- Tiempo de confirmacion: de 4 horas a menos de 30 segundos
- Reservas perdidas por fin de semana: de 4 a 0
- Ingresos recuperados: [dato concreto si disponible]

Y lo mas importante: el equipo de recepcion dejo de hacer data entry
y se enfoco en la atencion al huesped que ya llega confirmado."
```

### ACTO 6 - PRECIO Y CIERRE (3-5 min)
```
Objetivo: Presentar la inversion DESPUES de anclar el costo del problema.
Tecnica: Ancla de perdida antes que precio.

Script:
"Antes de hablar de la inversion, recuerda lo que calculamos:
[X habitaciones] x [precio promedio] x [noches] = [perdida mensual].

La implementacion de Virtuoso IA para tu propiedad es [precio mensual].
[PAUSA -- no llenar el silencio]

Eso significa que con recuperar [N] reservas que hoy se pierden,
el sistema se paga solo en el primer mes."

Cierre directo:
"La pregunta es: cuando arrancamos, esta semana o la siguiente?"
```

---

## 3. FLUJO TECNICO PASO A PASO (durante el Acto 4)

```
PASO 1 -- Mostrar el estado inicial del Calendario (15 seg)
  Accion  : Abrir Cloudbeds > Calendar > Room Grid View
  Mostrar : Que las fechas [checkin/checkout de Ana Martinez] estan vacias
  Script  : "Aqui esta el calendario de Cloudbeds ahora mismo.
             Estas fechas -- del [checkin] al [checkout] -- estan disponibles."

  -----------------------------------------------------------------

PASO 2 -- Disparar el Workflow desde GHL (30 seg)
  Accion  : Ir a GHL > Pipeline > Contacto "Ana Martinez"
            Cambiar stage a "Listo para Reservar"
            O ejecutar el Workflow manualmente desde GHL
  Script  : "Esto simula lo que ocurre cuando una persona llena
             tu formulario o tu agente de ventas califica el lead.
             Con un solo movimiento en el pipeline..."

  -----------------------------------------------------------------

PASO 3 -- Mostrar n8n procesando (30-45 seg)
  Accion  : Cambiar a pantalla de n8n (solo si el prospecto es tecnico)
            Si no es tecnico, saltar directamente al Paso 4
  Mostrar : Execuciones del workflow corriendo en tiempo real
  Script  : "Aqui puedes ver como Virtuoso IA procesa la reserva:
             valida los datos, formatea las fechas, y llama a
             la API de Cloudbeds. Todo en menos de 5 segundos."

  Alternativa no tecnica:
  Script  : "Mientras cambiamos de pantalla, Virtuoso IA ya esta
             procesando la reserva. Vamos al calendario..."

  -----------------------------------------------------------------

PASO 4 -- Revelar la reserva en Cloudbeds (el momento WOW)
  Accion  : Volver a Cloudbeds Calendar
             Hacer refresh (F5 o boton de recarga del calendario)
  Script  : "Vamos al calendario... [pausa dramatica al hacer refresh]
             Ahi esta."

  Mostrar : La reserva de Ana Martinez apareciendo en el Room Grid
             con las fechas correctas y el tipo de habitacion DQ

  -----------------------------------------------------------------

PASO 5 -- Abrir el detalle de la reserva (45 seg)
  Accion  : Hacer clic en la reserva para ver el detalle
  Mostrar :
    - Nombre: Ana Martinez
    - Email: ana.martinez.demo@virtuosoia.com
    - Fechas: [checkin] al [checkout]
    - Room Type: Deluxe Queen
    - Source: "Virtuoso IA / GHL"
    - Status: Confirmed

  Script  : "Todos los datos del huesped llegaron perfectamente.
             Sin errores de captura. Sin doble digitacion.
             Y el campo Source dice 'Virtuoso IA / GHL' para
             que tu equipo sepa que fue una reserva automatizada."

  -----------------------------------------------------------------

PASO 6 -- Volver a GHL para mostrar el loop completo (30 seg)
  Accion  : Cambiar a GHL > Contacto Ana Martinez
  Mostrar : Campo "Cloudbeds Reservation ID" actualizado con el ID real
            Campo "Reservation Status" = confirmed
            Nota automatica en el timeline del contacto

  Script  : "Y aqui en GHL, el sistema ya actualizo automaticamente
             el ID de la reserva y el estado. Tu equipo puede ver
             en tiempo real que la reserva esta confirmada en Cloudbeds
             sin tener que entrar a verificarlo manualmente."
```

---

## 4. MANEJO DE OBJECIONES TECNICAS

### Objecion: "Y si Cloudbeds cae o hay error?"
```
Script:
"Buena pregunta. El sistema tiene tres capas de proteccion:

Primero, n8n reintenta automaticamente 3 veces con 2 segundos
de espera si hay un error de conexion.

Segundo, si los 3 intentos fallan, GHL recibe una notificacion
de error y mueve el lead a un stage de 'Revision Manual'
para que alguien del equipo lo atienda.

Tercero, todas las ejecuciones quedan loggeadas en n8n con el
detalle del error para diagnostico.

En otras palabras: ningun lead se pierde silenciosamente."
```

### Objecion: "Que pasa si el huesped pone fechas incorrectas?"
```
Script:
"El nodo de validacion en n8n atrapa exactamente eso antes
de llegar a Cloudbeds:
- Detecta si checkout es anterior a checkin
- Detecta fechas en el pasado
- Detecta formatos invalidos
- Detecta campos vacios

Si algo falla la validacion, GHL recibe el error especifico
y puede enviar al huesped un mensaje automatico pidiendo
que corrija la informacion. Cero reservas corruptas en Cloudbeds."
```

### Objecion: "Tenemos varios tipos de habitacion, no solo DQ y DK"
```
Script:
"El sistema es completamente configurable. El mapeo de habitaciones
es una tabla en el codigo que se expande en 5 minutos:

Standard: 'STD' -> ID_123
Deluxe Queen: 'DQ' -> ID_456
Deluxe King: 'DK' -> ID_789
Suite: 'STE' -> ID_012

Cualquier codigo que uses en tu operacion se mapea al ID
de Cloudbeds correspondiente. La logica no cambia."
```

### Objecion: "Ya tenemos el Channel Manager integrado con Cloudbeds"
```
Script:
"Virtuoso IA no reemplaza tu Channel Manager -- trabaja en paralelo.
El Channel Manager gestiona disponibilidad desde OTAs (Booking, Expedia).
Virtuoso IA gestiona los leads directos: los que vienen por tu web,
WhatsApp, o tu equipo de ventas.

Son canales distintos. Los leads directos son los que tienen
mayor margen porque no pagan comision de OTA. Ese es
exactamente el mercado que Virtuoso IA esta protegiendo para ti."
```

---

## 5. CIERRE Y SIGUIENTE PASO

### Despues de la demo
```
NO preguntar: "Que te parecio?" (abre evaluacion infinita)

SI preguntar: "Viendo esto, en cuantas reservas directas
              por semana estamos hablando para tu propiedad?"
              [Dejar que cuantifiquen su propio potencial]

Luego: "La pregunta es solo de timing: cuando arrancamos,
        esta semana o la siguiente?"
```

### Si piden tiempo para decidir
```
Tecnica Voss - Etiqueta emocional:
"Parece que hay algo que no quedo del todo claro.
Que parte necesitas revisar?"
[Silencio -- no llenar]
```

### Si confirman
```
Micro-compromiso inmediato:
"Perfecto. Vamos a agendar la sesion de onboarding ahora
para que en 48 horas ya tengan el sistema corriendo en el Sandbox
con sus datos reales. Martes a las 10am o miercoles a las 3pm?"
```

---

## NOTAS TECNICAS ADICIONALES

```
Latencia esperada del flujo completo:
  GHL Webhook        : ~1 seg
  n8n Procesamiento  : ~1-2 seg
  Cloudbeds API      : ~2-3 seg
  Total              : 4-6 segundos desde trigger hasta reserva visible

Latencia maxima tolerable para la demo:
  Si el calendario tarda mas de 15 seg en mostrar la reserva,
  verificar que el token OAuth2 de n8n este vigente.
  Un token expirado causa timeouts de 10-15 segundos antes de fallar.

Recomendacion: Ejecutar una reserva de prueba 10 minutos antes
de la demo real para verificar que todo el flujo esta en caliente.
```
