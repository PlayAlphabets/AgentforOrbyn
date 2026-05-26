# Mini Proyecto de Prueba — Orbyn

## Contexto

Siguiente paso del proceso de selección de Orbyn: construir un **agente de cualificación de leads conectado a Telegram**. Será probado por el equipo de Orbyn. Cuando esté listo, enviarlo a **sales@orbyn.ai**. La selección se hace por orden de llegada.

---

## Qué tiene que hacer el agente

1. **Recibir un mensaje en Telegram** con los datos de un lead en texto libre.
   - Ejemplo: *"Empresa de consultoría, 15 empleados, Madrid, quieren automatizar su proceso de ventas."*

2. **Analizar los datos con un LLM** (OpenAI, Claude, el que prefieras) y decidir si el lead encaja con este ICP:
   - Empresa de **servicios o consultoría**
   - **Mínimo 5 empleados**
   - **España o Latinoamérica**
   - **Interés en automatización o IA**

3. **Responder en el mismo chat de Telegram** con la decisión:
   - Cualificado o no
   - 2 o 3 líneas explicando el razonamiento

4. **Loguear cada lead en una Google Sheet** con:
   - Fecha
   - Datos recibidos
   - Decisión
   - Motivo

**Herramientas libres:** n8n, Make, código propio, lo que se domine.

---

## Qué entregar

Enviar a **sales@orbyn.ai**:

- [ ] El **username del bot de Telegram** para que lo puedan probar
- [ ] El **flujo exportado** (JSON de n8n o Make) o **repo en GitHub** si se hizo con código
- [ ] Un **video de 1 minuto** explicando el flujo
- [ ] **3 frases** explicando qué cambiaría o mejoraría si esto fuera a producción real

---

## Cómo lo evalúan

| Criterio | Puntos |
|---|---|
| El bot funciona | 3 pts |
| La lógica de cualificación tiene criterio real, no responde siempre lo mismo | 3 pts |
| El logging en Google Sheet funciona correctamente | 2 pts |
| Las 3 frases de producción demuestran entendimiento de riesgos reales (manejo de errores, prompt injection, costes de API) | 2 pts |
| **Total** | **10 pts** |

**Puntuación mínima para entrar a la pool: 7/10**

---

## Stack / Decisiones a tomar antes de empezar

- [ ] **Herramienta principal:** n8n / Make / código propio (Python o Node)
- [ ] **Proveedor LLM:** OpenAI / Anthropic Claude / otro
- [ ] **Hosting del bot:** local (ngrok) / VPS / Railway / Render / serverless
- [ ] **Google Sheets:** API directa con service account / nodo nativo de n8n-Make / Sheets vía Apps Script

---

## Riesgos a abordar en las "3 frases de producción"

1. **Manejo de errores** — qué pasa si falla la API del LLM, si Telegram no responde, si Google Sheets cae.
2. **Prompt injection** — un lead malicioso podría escribir instrucciones que manipulen al LLM.
3. **Costes de API** — sin rate limiting ni control de longitud, un troll puede disparar la factura.
