# 🚀 Pipeline de Contenido Autónomo con HITL e Infraestructura Resiliente

## 📌 Descripción del Proyecto

Este proyecto implementa un ecosistema automatizado para la generación, revisión y publicación de contenido B2B en LinkedIn.

La solución combina:

- 🤖 Inteligencia Artificial para la generación de contenido.
- 👤 Human-in-the-Loop (HITL) para la aprobación manual.
- 📚 RAG (Retrieval-Augmented Generation) para mantener la coherencia con la marca.
- 🛡️ Arquitectura resiliente con manejo de errores y recuperación ante fallos.

El objetivo es automatizar el ciclo completo de publicación garantizando calidad, seguridad y continuidad operativa.

---

# 🏗️ Arquitectura del Pipeline

## 1️⃣ Generación de Contenido (RAG Activo)

El flujo monitorea nuevas ideas almacenadas en Airtable.

Para cada registro:

- Recupera las directrices de marca desde la base de conocimiento.
- Construye el contexto.
- Envía el contexto al Agente de IA.
- Genera el contenido listo para revisión.

---

## 2️⃣ Human-in-the-Loop (HITL)

El contenido generado **nunca se publica automáticamente**.

El proceso:

- Guarda el contenido en Airtable.
- Cambia el estado a **Pendiente de aprobación**.
- Envía un correo electrónico mediante SMTP.
- Detiene completamente el flujo.

La ejecución continúa únicamente cuando un usuario marca el campo **Aprobado** en Airtable.

---

## 3️⃣ Publicación y Resiliencia

Una vez aprobado:

- Se genera el payload para LinkedIn.
- Se ejecuta la llamada HTTP.

El nodo HTTP utiliza:

```
On Error → Continue
```

Si ocurre un problema:

- caída de la API
- credenciales inválidas
- timeout
- error de red

el flujo **no se detiene**.

En cambio:

- captura el error
- actualiza Airtable con el estado

```
Fallo de Conexión / Revisar API
```

y finaliza correctamente sin afectar el resto del sistema.

---

# 🔗 Recursos del Proyecto

## Base de Datos (Airtable)

> Modo lectura

https://airtable.com/invite/l?inviteId=inv3LT4FV51T2SEul&inviteToken=e1d69e59e26089409478958a1797cf710d6eff6e998ac1cdc9bd258fc4fdec58&utm_medium=email&utm_source=product_team&utm_content=transactional-alerts

---

## Evidencias

En la raíz del repositorio se incluyen las siguientes evidencias:

- 📷 n8n completo.png
- 📷 Airtable con contenido.png
- 📷 Gmail.png

---

# 📋 Evaluación del Proyecto

## 📘 1. Manual Operativo de Estructuras de Datos (20%)

La comunicación entre Airtable y n8n utiliza un payload JSON estandarizado.

```json
{
  "recordId": "recXXXXX",
  "ideaSemilla": "Idea inicial ingresada por el usuario",
  "autor": "Cuenta Principal",
  "contenido": "Texto generado por IA",
  "postPayload": {
    "author": "urn:li:person:norberto-loyola",
    "lifecycleState": "PUBLISHED",
    "specificContent": {
      "com.linkedin.ugc.ShareContent": {
        "shareCommentary": {
          "text": "Contenido del post"
        },
        "shareMediaCategory": "NONE"
      }
    }
  }
}
```

Este esquema permite desacoplar la generación del contenido respecto del mecanismo de publicación.

---

# 💰 2. Optimización de Costos y Recursos (20%)

| Estrategia | Descripción |
|------------|-------------|
| Modelos Especializados | Gemini generan el contenido utilizando contexto RAG. |
| Procesamiento Local | Validaciones, filtros y transformaciones se realizan mediante JavaScript y nodos nativos de n8n, evitando llamadas innecesarias a APIs. |
| Procesamiento por Lotes | Airtable procesa registros mediante intervalos programados, reduciendo ejecuciones duplicadas y consumo de tokens. |

---

# 🔒 3. Seguridad, Privacidad y Resiliencia (20%)

## Minimización de Datos

El pipeline únicamente procesa:

- Idea Semilla
- Contenido Generado

No se transfieren:

- credenciales
- datos personales
- información sensible

cumpliendo con buenas prácticas de privacidad.

---

## Manejo de Errores

El nodo HTTP Request implementa:

```
On Error → Continue (Using Error Output)
```

Ante cualquier fallo:

- el flujo continúa
- el error queda registrado
- Airtable actualiza automáticamente el estado a:

```
Fallo de Conexión / Revisar API
```

---

## Human-in-the-Loop

Antes de cualquier publicación:

✅ El contenido debe ser aprobado manualmente.

La publicación sólo ocurre cuando un operador activa el checkbox **Aprobado** en Airtable.

Este mecanismo elimina publicaciones automáticas no verificadas y reduce significativamente riesgos legales o reputacionales.

---

# ✅ Tecnologías Utilizadas

- n8n
- Airtable
- OpenAI / Gemini
- SMTP
- LinkedIn API
- JavaScript
- JSON

---

# 🎯 Características Principales

- Generación automática de contenido
- Integración con RAG
- Human-in-the-Loop (HITL)
- Publicación automatizada
- Manejo robusto de errores
- Actualización automática de estados
- Arquitectura resiliente
- Optimización de costos mediante procesamiento híbrido

## 🎥 Video de Demostración del Ecosistema

Puedes ver el funcionamiento en tiempo real de la automatización, la validación humana (HITL) y la resiliencia del pipeline:

[![Ver Demo del Pipeline](https://img.youtube.com/vi/TW5Sg53YWJ8/maxresdefault.jpg)](https://www.youtube.com/watch?v=TW5Sg53YWJ8)
