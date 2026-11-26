# Jev o Laya
Jev responde preguntas, no escribe texto.

Jev es para elegir, puntuar o booleanas. No es que responda preguntas booleanas, sino que nos puntúa entre 0 y 1 como de bueno o malo es algo por ejemplo.

Cuando usar Jev:
- Clasificar y enrutar
- Modelar contenido
- Ordenar y recomendar
- Validar antes de actuar

Cuando NO usar Jev:
- Redactar texto
- Conversar
- Razonar en varios pasos
## Diferencias entre Jev o Laya
Diferencias entre Jev y Laya, Jev es un modelo privado solo se puede usar mediante peticiones a su propia API mientras que Laya, también permite peticiones a su propia API, pero también podríamos descargar el modelo de Laya, ya que es un modelo abierto y usarlo sería completamente gratuito.

Una requests a Jev o Laya se vería así:

```JSON
curl -X POST https://api.typesafe.ai/v1/systemone \
  -H "Authorization: Bearer TU_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "jev-latest",
    "state": {
      "message": "Me habéis cobrado dos veces este mes. Devolvedme el dinero o cancelo la cuenta hoy mismo."
    },
    "questions": {
      "departamento": {
        "type": "choice",
        "instructions": "¿Qué departamento debe gestionar este mensaje?",
        "criteria": {
          "facturacion": "cobros duplicados, reembolsos o facturas",
          "soporte_tecnico": "errores de la plataforma o fallos técnicos",
          "bajas": "el cliente quiere cancelar su suscripción"
        }
      },
      "es_urgente": {
        "type": "noul",
        "instructions": "¿El mensaje comunica presión de tiempo o urgencia inmediata?"
      },
      "frustracion": {
        "type": "score",
        "instructions": "¿Qué nivel de frustración muestra el cliente?",
        "criteria": ["calmado", "preocupado", "molesto", "muy enfadado"]
      }
    }
  }'

```

Una respuesta de ejemplo que nos podria dar Jev o Laya a esa request seria:

```JSON
{
  "id": "sys1_9f8d7c6b5a4e3d2c1b0a",
  "object": "systemone.response",
  "created": 1795715000,
  "model": "jev-latest",
  "answers": {
    "departamento": {
      "type": "choice",
      "value": "facturacion",
      "probabilities": {
        "facturacion": 0.892,
        "bajas": 0.098,
        "soporte_tecnico": 0.010
      }
    },
    "es_urgente": {
      "type": "noul",
      "value": true,
      "probability": 0.964
    },
    "frustracion": {
      "type": "score",
      "value": "muy enfadado",
      "index": 3,
      "expected_value": 2.91,
      "probabilities": [0.002, 0.011, 0.072, 0.915]
    }
  },
  "usage": {
    "prompt_tokens": 142,
    "questions_evaluated": 3
  }
}

```