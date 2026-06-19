# Pipeline de Contenido Autónomo con HITL e Infraestructura Resiliente

## 📌 Descripción del Proyecto
Este ecosistema automatiza el ciclo completo de creación, curación y publicación de contenidos B2B para LinkedIn. El sistema combina las capacidades de razonamiento de un Agente de IA con un estricto control humano en el bucle (Human-In-The-Loop) y una arquitectura diseñada para tolerar fallos de red o de API externas.

## 🛠️ Arquitectura y Componentes
1. **Fase de Generación (RAG Activo):** El sistema escucha nuevas ideas semilla en Airtable, recupera las directrices de marca corporativas de una base de conocimiento y prepara el contexto estructurado para el Agente de IA.
2. **Punto de Pausa HITL (Human-In-The-Loop):** La propuesta generada se almacena en estado "Pendiente de Aprobación" y se despacha una alerta automatizada por correo electrónico (SMTP). El flujo externo se congela hasta que un humano valida el texto y activa manualmente el checkbox de aprobación.
3. **Fase de Publicación y Resiliencia (Manejo de Errores):** Al recibir la aprobación, se prepara el payload para redes sociales. El nodo de comunicación externa cuenta con una bifurcación activa ante errores (`On Error: Continue`). Si la API externa experimenta caídas o rebotes de credenciales, el sistema captura el error de forma elegante y actualiza el registro en Airtable a "Fallo de Conexión / Revisar API" en lugar de romper el flujo general.

---

## 🔗 Enlaces Obligatorios de Entrega

* **Base de Datos (Airtable - Modo Lectura):** https://airtable.com/invite/l?inviteId=inv3LT4FV51T2SEul&inviteToken=e1d69e59e26089409478958a1797cf710d6eff6e998ac1cdc9bd258fc4fdec58&utm_medium=email&utm_source=product_team&utm_content=transactional-alerts
* **Evidencias de Ejecución:** Las capturas de pantalla que demuestran el flujo en el lienzo (n8n completo.png), la actualización de registros en Airtable (Airtable con contenido.png) y el correo de notificación recibido por SMTP (Gmail.png) se encuentran disponibles en la raíz de este repositorio.
