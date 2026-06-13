# Guía del taller (90 minutos): Construye tu chatbot de empresa

## Objetivo de aprendizaje

Al final de la sesión, cada estudiante tendrá un chatbot personalizado que:

- tiene una idea de empresa y un propósito claro,
- responde usando datos de la empresa,
- tiene una personalidad definida,
- usa herramientas (tools) opcionales,
- muestra **tokens reales**, **tiempo de respuesta** y **coste** por petición,
- tiene un logotipo de empresa,
- puede desplegarse online desde GitHub.

## Qué incluye la app actual

La plantilla usa **Streamlit** y llama a **CompactifAI** (API compatible con OpenAI).

### Lo que el estudiante edita en el repositorio (como un desarrollador real)

```text
config/
  company.json        → nombre, bot, objetivo, usuarios, zona horaria, preguntas de ejemplo
  personality.json    → tono, rol, estilo, reglas de seguridad
  tools.json          → herramientas activadas por defecto
  model.json          → modelo por defecto, temperatura, precios por modelo

company_data/         → PDF, DOCX, TXT, MD, CSV, JSON (datos permanentes)
company_logo/         → un logo (PNG, JPG, WEBP)
```

**Importante:** no hay subida de archivos ni de logo dentro de la interfaz. Todo va al repo en GitHub.

### Lo que el estudiante controla en la barra lateral (experimento en vivo)

- **Modelo:** `hypernova-60b` (por defecto), `blackstar-10b`, `glm-5-1`
- **Herramientas:** activar/desactivar cada tool
- **Historial:** enviar o no el historial del chat al modelo
- **Reiniciar chat** y **limpiar registro de comparación**

### Pestañas de la interfaz

- **Chat** — conversación, preguntas sugeridas y caja de mensaje (siempre abajo)
- **Laboratorio de comparación** — tabla con tokens, tiempo, coste, herramientas e historial; descarga CSV

### Métricas bajo cada respuesta

Solo se muestran datos **reales** devueltos por la API (`usage`):

- tiempo (s)
- tokens totales
- coste en USD (calculado con los precios de `config/model.json`)
- modelo usado
- herramientas llamadas

Si no hay API key o la API no devuelve usage → `tokens n/a` y no se registra fila en el laboratorio.

---

## Cronograma del taller

### 0–10 min: ¿Qué estamos construyendo?

Explicad el resultado final: un chatbot pequeño adaptable a cualquier empresa.

Conceptos clave:

- **Modelo:** genera la respuesta.
- **Personalidad:** instrucciones que definen estilo y comportamiento.
- **Conocimiento/datos:** ficheros en `company_data/` que el bot puede usar.
- **Herramientas (tools):** acciones que el modelo puede invocar (calculadora, búsqueda, fecha/hora, ticket de soporte demo).
- **Tokens:** unidades que consume el modelo para leer contexto, definiciones de tools y generar respuestas.
- **Coste:** depende del modelo y de cuántos tokens de entrada/salida se usan (precios en `config/model.json`).
- **Despliegue:** publicar la app online con Streamlit Cloud desde GitHub.

Mini-ejercicio para el grupo:

```text
Elegid una empresa u organización. ¿En qué debe ayudar vuestro bot?
Ejemplos: atención al cliente, RR. HH. interno, asistente técnico, ventas, onboarding.
```

### 10–25 min: Elegir empresa y objetivo del bot

Editad en GitHub:

```text
config/company.json
```

Campos importantes:

- `company_name` — nombre de la empresa
- `bot_name` — nombre del asistente
- `bot_goal` — para qué sirve el bot
- `target_users` — a quién ayuda
- `product_service` — producto o servicio
- `contact_email` — email de escalado (solo informativo; no envía correos)
- `timezone` — por defecto `Europe/Madrid` (hora de España)
- `welcome_message` — saludo inicial
- `examples_of_good_questions` — preguntas sugeridas que aparecen como botones en el chat

Ejemplos de proyectos:

```text
Bot de hotel que responde dudas de huéspedes.
Bot técnico de placas solares que explica códigos de error.
Bot interno de RR. HH. para onboarding.
Bot de gimnasio que ayuda a elegir clases.
```

### 25–40 min: Añadir datos de la empresa

Los estudiantes buscan información pública o inventan datos ficticios.

Subid ficheros a:

```text
company_data/
```

Formatos recomendados:

- TXT o Markdown — más rápidos de probar
- PDF — manuales o folletos
- DOCX — políticas o documentos internos
- CSV — listas de productos o precios

Ideas de contenido:

- FAQ
- descripción de servicios
- política de devoluciones
- horarios
- manual técnico
- tabla de resolución de problemas
- lista de precios
- normas de seguridad

Regla importante:

```text
Solo datos públicos o ficticios. No subáis datos privados, médicos, bancarios ni confidenciales.
```

Comprobad en la cabecera de la app: si pone **«No company data yet»** / **«0 knowledge chunks»**, aún no hay datos cargados.

### 40–50 min: Añadir branding

Subid **un** logo a:

```text
company_logo/
```

Nombre recomendado:

```text
logo.png
```

Tras guardar en GitHub, Streamlit recargará la app y mostrará el logo arriba. **No hay subida temporal desde la interfaz.**

### 50–62 min: Diseñar la personalidad

Editad:

```text
config/personality.json
```

Definid:

- `role` — rol del asistente
- `tone` — tono
- `language` — idioma de respuesta
- `answer_length` — longitud de las respuestas
- `do` / `dont` — qué debe y no debe hacer
- `safety_rules` — reglas de seguridad

Ejercicio:

Haced la misma pregunta con dos personalidades distintas y comparad las respuestas.

Tonos de ejemplo:

```text
cercano y sencillo
premium y exclusivo
técnico y preciso
tranquilo y reconfortante
con humor pero profesional
```

### 62–78 min: Herramientas, modelos, historial y comparación de costes

#### Herramientas disponibles

Valores por defecto en `config/tools.json`; en el taller se cambian desde la barra lateral:


| Tool                    | Qué hace                                                                |
| ----------------------- | ----------------------------------------------------------------------- |
| `search_company_data`   | Busca en los ficheros de `company_data/`                                |
| `calculator`            | Calcula expresiones matemáticas (precios, descuentos, medidas)          |
| `current_datetime`      | Devuelve fecha y hora en la zona de `company.json` (España por defecto) |
| `create_support_ticket` | **Demo:** simula un ticket; **no** envía email ni guarda nada real      |


Combinaciones sugeridas:

```text
Atención al cliente: search_company_data + create_support_ticket
Ventas: search_company_data + calculator
Técnico: search_company_data + calculator + create_support_ticket
Asistente interno: search_company_data + current_datetime
```

#### Modelos del taller


| Modelo          | Nota                                       |
| --------------- | ------------------------------------------ |
| `hypernova-60b` | Por defecto; buen equilibrio coste/calidad |
| `blackstar-10b` | Más barato; ideal para comparar coste      |
| `glm-5-1`       | Más caro; comparar precio vs calidad       |


Precios oficiales CompactifAI en `config/model.json` → sección `pricing`.

#### Experimentos obligatorios (pestaña «Laboratorio de comparación»)

1. **Coste de tools:** misma pregunta con todas las tools OFF y luego ON → comparad *Prompt tok* y *Cost ($)*.
2. **Coste de modelo:** misma pregunta en `hypernova-60b`, `blackstar-10b` y `glm-5-1` → comparad tokens, tiempo y coste.
3. **Uso de tools:** «¿Cuánto es 120 × 0,21?» con calculadora ON vs OFF → mirad *Tools called*.
4. **Coste de historial:** haced una pregunta de seguimiento con **Send chat history** ON y luego OFF → comparad *Prompt tok*.

Recordatorio para el profesor:

```text
Las tools aumentan tokens aunque no se usen: las definiciones se envían en cada petición.
Si el modelo llama una tool, puede haber varias rondas de API → más tokens y más tiempo.
El coste solo es fiable con COMPACTIF_API_KEY configurada y usage real de la API.
```

### 78–87 min: Probar y mejorar

Probad con cinco preguntas:

1. Una fácil que esté en los documentos.
2. Una que **no** esté en los documentos.
3. Una que requiera cálculo (`calculator`).
4. Una que requiera escalado a humano (`create_support_ticket` activado).
5. Una en otro idioma.

Mejorad los JSON según los resultados. Revisad el laboratorio de comparación y descargad el CSV si queréis debatir en grupo.

### 87–90 min: Demos rápidas

Cada estudiante, 30 segundos:

```text
Mi empresa es...
Mi bot ayuda a...
He añadido estos ficheros en company_data/...
He elegido esta personalidad...
He activado estas tools...
Con tools ON/OFF vi esta diferencia de tokens y coste...
Con historial ON/OFF vi esta diferencia...
El mejor modelo para mi caso fue... porque...
```

---

## Checklist del profesor

Antes de la clase:

- Plantilla en GitHub (como template repo si es posible).
- Despliegue en Streamlit Community Cloud probado (`app.py` como entrypoint).
- API key de CompactifAI en secrets de Streamlit:

```toml
COMPACTIF_API_KEY = "vuestra-clave"
COMPACTIF_BASE_URL = "https://api.compactif.ai/v1"
```

- Confirmar que las respuestas muestran tokens, tiempo, coste y tools llamadas.
- Confirmar que el logo carga desde `company_logo/`.
- Confirmar que el laboratorio de comparación registra filas y permite descargar CSV.
- Preparar una pregunta de ejemplo que muestre claramente más tokens con tools activadas (p. ej. «¿Cuánto es 120 × 0,21?»).
- Preparar una segunda pregunta de seguimiento para el experimento de historial ON/OFF.

Durante la clase:

- Recordar: **no hay subida de archivos en la UI** — todo va a GitHub.
- Recordar: `create_support_ticket` es simulación; no envía emails.
- Si alguien ve `tokens n/a`, revisar que la API key esté bien en secrets.

## Referencia rápida de la interfaz

```text
Barra lateral
  ├── Modelo (hypernova-60b | blackstar-10b | glm-5-1)
  ├── Tools (checkboxes)
  ├── Send chat history (ON/OFF)
  ├── Reiniciar chat
  └── Limpiar registro de comparación

Pestaña Chat
  ├── Preguntas sugeridas (solo al inicio)
  ├── Historial de mensajes + métricas
  └── Caja de mensaje (fija abajo)

Pestaña Laboratorio de comparación
  ├── Deltas vs petición anterior (tokens, tiempo, coste, tools)
  ├── Total de coste de la sesión
  ├── Tabla de experimentos + descarga CSV
  └── Tarifas por modelo (desde model.json)
```

## Aviso legal / de taller

Este proyecto es un **prototipo educativo**, no un sistema de atención al cliente en producción. No uséis datos reales de clientes, contraseñas, información médica, bancaria ni confidencial.