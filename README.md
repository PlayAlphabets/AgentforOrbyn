# Orbyn — Bot de cualificación de leads en Telegram

Prueba técnica para el proceso de selección de Orbyn. Bot conversacional de
Telegram que cualifica leads B2B contra el ICP de Orbyn y los registra en una
Google Sheet.

## Funcionalidad

- Recibe mensajes en lenguaje natural por Telegram.
- Mantiene conversación con memoria por usuario hasta tener los 5 datos del lead.
- Clasifica el lead aplicando un knowledge base configurable (`knowledge_base.md`).
- Devuelve `CUALIFICADO` o `NO CUALIFICADO` con razonamiento en 2-3 frases.
- Loguea cada lead en Google Sheets (fecha, datos, decisión, motivo).

## ICP

Para cualificar, el lead debe cumplir los 4 criterios:
- Empresa de servicios o consultoría
- Mínimo 5 empleados
- Ubicada en España o Latinoamérica
- Interés explícito en automatización o IA

## Stack

- Python 3.9+
- `python-telegram-bot` 21 (polling)
- `openai` (gpt-4o-mini por defecto)
- `gspread` + `google-auth` (Sheets API)
- Knowledge base en Markdown, inyectado en el prompt en cada llamada

## Estructura

```
.
├── main.py              # Bot completo
├── knowledge_base.md    # Conocimiento de negocio (editable sin tocar código)
├── requirements.txt
├── .env.example         # Plantilla de variables de entorno
└── .gitignore           # Excluye .env y credentials.json
```

## Variables de entorno

| Variable | Descripción |
|---|---|
| `TELEGRAM_TOKEN` | Token del bot (de @BotFather) |
| `OPENAI_API_KEY` | API key de OpenAI |
| `OPENAI_MODEL` | Modelo a usar (default: `gpt-4o-mini`) |
| `GOOGLE_SHEET_ID` | ID del Sheet donde loguear |
| `GOOGLE_CREDENTIALS_JSON` | JSON completo de la service account (para deploy) |
| `GOOGLE_CREDENTIALS_PATH` | Alternativa local: ruta al fichero JSON (default `credentials.json`) |

## Ejecución local

```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
cp .env.example .env   # rellena con tus valores
python main.py
```

## Endurecimientos incluidos

- Timeout y reintentos en llamadas a OpenAI.
- `max_tokens` para acotar coste por respuesta.
- Límite de longitud del mensaje del usuario (1000 caracteres) — anti
  prompt injection y control de coste.
- Reglas anti prompt injection en el knowledge base.
- Try/except en cada llamada externa; el bot avisa al usuario en vez de
  caerse si el LLM o Sheets fallan.
- El logging a Sheets nunca rompe la respuesta al usuario.
