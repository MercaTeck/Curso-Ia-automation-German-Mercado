# GesFlota Manager M4 — Integraciones Avanzadas

Workflow de n8n para el Módulo 4 del curso **IA Automation Avanzado** (Coderhouse).

**Caso de uso:** GesFlota, un SaaS de gestión de flotas vehiculares. Este workflow conecta el agente de IA con tres herramientas externas del negocio (Gmail, HubSpot y Slack) mediante OAuth2, automatizando la recepción de emails de soporte, la gestión de contactos en el CRM y la notificación al equipo de operaciones.

## Flujo del workflow

```
Gmail Trigger → Is Auto-Reply? (If) → AI Agent (Anthropic) → Clean Payload (Set)
→ Lookup Contact (HubSpot) → Contact Exists? (If) → Update / Create contact
→ Draft Response (Gmail HITL) → Notify Slack
```

### Nodos clave

| Nodo | Función | Problema que resuelve |
|------|---------|----------------------|
| **Is Auto-Reply?** | Filtra auto-replies, out of office, undeliverable, no-reply@ | Corta bucle infinito de auto-respuestas |
| **Lookup Contact** | Busca el contacto en HubSpot antes de crear | Previene Error 409 (duplicados en CRM) |
| **Draft Response (HITL)** | Crea borrador en Gmail, no envía directo | Guardrail Human-in-the-loop |
| **Clean Payload** | Limpia el email a 4 campos útiles | Previene Error 400 (payload pesado) |

## Cómo importar en n8n

1. Abrí n8n (cloud o self-hosted).
2. Ir a **Workflows** → **Import from File**.
3. Seleccionar `checkpoint4_mercado_german.json`.
4. El workflow se carga con todos los nodos y conexiones.

## Credenciales necesarias (OAuth2)

Antes de ejecutar, configurar las siguientes credenciales en n8n:

- **Gmail** — OAuth2 con scopes de lectura y creación de borradores.
- **HubSpot** — OAuth2 con scopes de contactos (lectura y escritura).
- **Slack** — OAuth2 con scope de envío de mensajes al canal de operaciones.

## Cómo probar

1. Configurar las 3 credenciales OAuth2 (verificar semáforo verde en cada una).
2. Hacer clic en **Execute Workflow**.
3. Enviar un email de prueba a la casilla de Gmail conectada.
4. Verificar que:
   - El email pasa el filtro anti auto-reply.
   - El AI Agent genera una respuesta.
   - El contacto se crea o actualiza en HubSpot.
   - Aparece un borrador en Gmail (no se envía directo).
   - Llega la notificación al canal de Slack.

## Autor

**Germán Mercado** — IA Automation Avanzado, Coderhouse (2026)
