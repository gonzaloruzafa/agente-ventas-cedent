# Agente Ventas Cedent

Configuración y recursos para el agente de ventas de Cedent implementado en Odoo.

## 📁 Archivos

- **`PROMPT.md`**: Prompt completo para configurar el agente de IA con instrucciones de formato HTML
- **`odoo-chat.css`**: CSS para corregir el renderizado de mensajes en el chat de Odoo

## 🎯 Propósito

Este repositorio almacena la configuración del asistente de ventas automatizado para:
- Responder consultas de productos
- Proporcionar precios actualizados
- Generar links directos a productos en https://www.cedent.com.ar/shop

## 🚀 Uso

### Configurar el Prompt en Odoo

1. Copia el contenido de `PROMPT.md`
2. Pégalo en la configuración del agente de IA en Odoo
3. Ajusta según necesites

### Aplicar el CSS

1. Accede al panel de administración de Odoo
2. Ve a Configuración → Técnico → Vistas
3. Busca la vista del chat/livechat
4. Agrega el contenido de `odoo-chat.css` en la sección de estilos

## 📝 Notas Técnicas

- El chat de Odoo **NO soporta Markdown** correctamente
- Se debe usar **HTML puro** con etiquetas `<br>` y `<a>`
- El CSS fuerza `white-space: pre-wrap` para respetar saltos de línea
- Los enlaces deben incluir `target="_blank"` para abrir en nueva pestaña

## 🔗 Enlaces Relacionados

- [Tienda Cedent](https://www.cedent.com.ar/shop)
- [Documentación Odoo](https://www.odoo.com/documentation)

## 📄 Licencia

Uso interno de Cedent.
