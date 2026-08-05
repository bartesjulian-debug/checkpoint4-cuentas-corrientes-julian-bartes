# Checkpoint 4 - Cuentas Corrientes

## Preentregable

Sincronización del Cerebro Agéntico con Ecosistemas de Negocio.

## Objetivo

Automatizar la recepción y clasificación de consultas de cuentas corrientes, integrando herramientas externas reales y manteniendo revisión humana antes de cualquier respuesta al cliente.

## Conectores utilizados

- Gmail: recepción de correos y creación de borradores.
- Airtable: fuente de datos de clientes, cuentas corrientes y memoria del agente.
- Telegram: canal interno de notificación al equipo.
- Groq: modelo de lenguaje utilizado por el agente.

## Flujo general

1. Gmail detecta un correo nuevo.
2. Un nodo IF bloquea respuestas automáticas, mensajes de ausencia, correos rebotados y remitentes no-reply.
3. Un nodo Edit Fields reduce el payload a los campos necesarios.
4. Airtable recupera la memoria previa por Session_ID.
5. El workflow busca al cliente por email antes de crear un registro nuevo.
6. Si el cliente existe, consulta su cuenta corriente.
7. Si no existe, crea el contacto sin duplicarlo.
8. El agente clasifica la consulta y genera una respuesta estructurada.
9. Gmail crea exclusivamente un borrador para revisión humana.
10. Airtable actualiza la memoria mediante Upsert.
11. Telegram notifica al equipo con un payload mínimo.

## Controles implementados

- Filtro anti-loop inmediatamente posterior al Gmail Trigger.
- Lookup previo al Create para evitar contactos duplicados.
- Payload mínimo antes de los conectores externos.
- Respuesta estructurada mediante Output Parser.
- Creación de borradores en Gmail, sin envío automático.
- Revisión humana obligatoria en reclamos, diferencias de saldo y casos sin cuenta asociada.
- Upsert de memoria por Session_ID.

## Pruebas realizadas

- Cliente existente con cuenta corriente.
- Cliente nuevo.
- Cliente existente sin cuenta corriente asociada.
- Correo automático bloqueado.
- Reclamo por diferencia de saldo con revisión humana obligatoria.

## Archivo entregable

`checkpoint4_Julian_Bartes.json`

## Seguridad

El repositorio no contiene tokens, claves API ni secretos de autenticación. Las credenciales deben configurarse nuevamente al importar el workflow en otra instancia de n8n.
