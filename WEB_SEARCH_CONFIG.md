# Configuración Web Search para Usuarios Públicos en Odoo

## 🚨 Problema

Por defecto, Odoo bloquea el acceso a `ai.agent._ai_tool_web_search` para usuarios públicos (visitantes no autenticados). Esto causa errores cuando el chatbot intenta buscar productos en la tienda.

**Error típico:**
```
AccessError: El usuario 'Público' no tiene permiso para ejecutar acciones en ai.agent
```

---

## ✅ Solución: Agregar `.sudo()` al Web Search Tool

### Paso 1: Ubicar el código

1. Ve a: **Configuración → Técnico → Automation → Server Actions**
2. Busca la acción del **Web Search Tool** (o la herramienta que ejecuta búsquedas en el agente)
3. Abre la acción para editarla

### Paso 2: Modificar el código Python

Busca esta línea en el código:

**❌ ANTES (código bloqueado para público):**
```python
ai['result'] = env["ai.agent"]._ai_tool_web_search(query, max_results, search_depth, include_domains)
```

**✅ DESPUÉS (código con permisos):**
```python
ai['result'] = env["ai.agent"].sudo()._ai_tool_web_search(query, max_results, search_depth, include_domains)
```

**Cambio:** Solo agregás `.sudo()` después de `env["ai.agent"]` y antes del punto.

---

### Paso 3: 🛑 CRÍTICO - Vaciar "Grupos Permitidos"

En la misma ventana de edición, arriba a la derecha verás el campo **"Grupos permitidos"** (Allowed Groups).

**IMPORTANTE:**
- Si tiene algún valor (ej: "User", "Employee", "Internal User"): **BÓRRALO COMPLETAMENTE**
- Debe quedar **vacío**

**¿Por qué?**
- **Campo vacío** = Accesible para todos (incluido usuarios públicos)
- **Campo con datos** = Solo esos grupos específicos pueden usarlo

---

### Paso 4: Guardar

Haz clic en **Guardar** y prueba el chatbot.

---

## 🔍 ¿Qué hace `.sudo()`?

`.sudo()` es un método de Odoo que significa **"Superuser Do"** (Hacer como Superusuario).

Le dice a Odoo:
> *"Ejecuta esta búsqueda con permisos de Administrador, sin importar quién la esté pidiendo"*

Sin esto, cuando un visitante web (usuario "Público") pregunta algo, Odoo verifica sus permisos y lo bloquea porque no tiene acceso al modelo `ai.agent`.

Con `.sudo()`, Odoo ejecuta la búsqueda con permisos elevados y el visitante recibe la respuesta.

---

## 🧪 Verificación

Después de aplicar los cambios:

1. **Cierra sesión** o abre una ventana privada/incógnito
2. Ve a tu sitio web de Cedent
3. Abre el chatbot (sin hacer login)
4. Pregunta por un producto (ej: "Busco implantes dentales")
5. **Resultado esperado:** El bot responde con productos y precios
6. **Si falla:** Revisa que:
   - El `.sudo()` esté bien colocado
   - El campo "Grupos permitidos" esté completamente vacío
   - No haya errores en los logs de Odoo (Configuración → Técnico → Logging)

---

## ⚠️ Consideraciones de Seguridad

**¿Es seguro usar `.sudo()` aquí?**

✅ **SÍ**, en este caso específico es seguro porque:

1. Solo se aplica a la **búsqueda de productos** (lectura)
2. No permite escribir/modificar datos
3. El agente está diseñado para responder consultas, no para acceder a datos sensibles
4. Es el comportamiento esperado: cualquier visitante debe poder consultar productos

**NO uses `.sudo()` indiscriminadamente** en otras acciones que involucren:
- Modificación de datos
- Acceso a información confidencial
- Transacciones financieras
- Datos de clientes

---

## 📝 Resumen Rápido

```python
# 1. Agregá .sudo() en el código
ai['result'] = env["ai.agent"].sudo()._ai_tool_web_search(...)

# 2. Vacía "Grupos permitidos"
[Campo vacío]

# 3. Guarda y prueba
```

**¡Listo!** El chatbot ahora podrá responder a visitantes públicos sin errores de permisos.
