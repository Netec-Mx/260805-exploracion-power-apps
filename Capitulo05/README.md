# Demostración / Caso Aplicado — Integración Final con Copilot y Power Automate

## Metadatos

| Campo | Valor |
|-------|-------|
| **Duración** | 10 minutos |
| **Complejidad** | Alta |
| **Nivel Bloom** | Aplicar |

## Descripción General

Este laboratorio cierra la serie de prácticas produciendo la versión final de la aplicación **AppInventario_[InicialNombre]**. Integrarás tres capacidades clave: generación de una pantalla con Copilot, creación y conexión de un flujo de Power Automate para notificaciones por correo, y aplicación de buenas prácticas de nombrado y manejo de errores. Al finalizar, la aplicación contará con 6 pantallas, un flujo funcional y estará publicada en el entorno de laboratorio.

## Objetivos de Aprendizaje

- [ ] Utilizar Copilot en Power Apps Studio para generar una pantalla de formulario y optimizar una fórmula existente.
- [ ] Crear un flujo de nube instantáneo en Power Automate que envíe una notificación por correo electrónico al activarse desde Power Apps.
- [ ] Conectar el flujo a la aplicación mediante el conector Power Automate y activarlo desde un botón en `Pantalla_Carrito`.
- [ ] Aplicar buenas prácticas de integración: nombrado consistente con prefijos, manejo de errores con `IfError()` y organización del árbol de componentes.

## Prerrequisitos

### Conocimiento previo

| Requisito | Detalle |
|-----------|---------|
| Lab 04-00-01 completado | Aplicación con 5 pantallas, colección `colCarritoTrabajo` y navegación condicional funcionando |
| Fórmulas Power Fx básicas | `Navigate()`, `Collect()`, `CountRows()`, `Patch()` |
| Conceptos de flujos | Comprensión de triggers y acciones en Power Automate |

### Acceso requerido

| Recurso | Verificación |
|---------|-------------|
| Power Apps Studio con Copilot habilitado | Panel lateral de Copilot visible al abrir el diseñador |
| Power Automate (make.powerautomate.com) | Inicio de sesión exitoso con cuenta Microsoft 365 |
| Conexión Office 365 Outlook aprobada | Sin bloqueos DLP en el entorno `LabEnvironment-PowerApps` |
| Correo electrónico Microsoft 365 activo | Capacidad de recibir y verificar correos entrantes |

## Entorno del Laboratorio

| Componente | Especificación |
|------------|---------------|
| Entorno Power Platform | `LabEnvironment-PowerApps` (verificar selector superior derecho) |
| Aplicación base | `AppInventario_[InicialNombre]` — 5 pantallas del Lab 04 |
| Flujo a crear | `Flujo_NotificacionInventario` — nube instantáneo |
| Lista SharePoint | `Inventario_Lab` en `https://[tenant].sharepoint.com/sites/LabSite-PowerApps` |
| Navegador | Chrome 124+ o Edge 124+ |

---

## Paso a Paso

### Paso 1 — Generar pantalla con Copilot

**Objetivo:** Usar Copilot en Power Apps Studio para crear una pantalla de formulario para registrar nuevos productos.

**Instrucciones:**

1. Abre `make.powerapps.com`, verifica que el entorno sea **LabEnvironment-PowerApps** en el selector superior derecho.
2. Abre la aplicación **AppInventario_[InicialNombre]** en modo **Editar**.
3. Localiza el panel de **Copilot** en el lado derecho del diseñador (ícono de estrella/sparkle). Si no es visible, haz clic en el ícono de Copilot en la barra de herramientas superior.
4. En el campo de texto del panel Copilot, escribe el siguiente prompt:

```
Agrega una pantalla de formulario para registrar un nuevo producto con campos Nombre, Categoría, Cantidad y Precio
```

5. Presiona **Enter** o haz clic en **Enviar**. Espera a que Copilot genere la pantalla.
6. Revisa la pantalla generada en el árbol de componentes (panel izquierdo).
7. Renombra la pantalla generada a **`scr_NuevoProducto`** haciendo doble clic sobre su nombre en el árbol.
8. Verifica que contenga controles de entrada de texto. Si Copilot generó nombres genéricos, renómbralos con los prefijos correctos:
   - `txt_NombreProducto`
   - `txt_CategoriaProducto`
   - `txt_CantidadProducto`
   - `txt_PrecioProducto`
9. Si la pantalla incluye un botón de guardar, renómbralo a **`btn_GuardarProducto`**.
10. Si Copilot no generó un botón de guardar, inserta uno manualmente: **Insertar > Botón**, renómbralo a `btn_GuardarProducto` y establece su propiedad `Text` a `"Guardar Producto"`.

**Resultado esperado:** Una sexta pantalla llamada `scr_NuevoProducto` aparece en el árbol de componentes con campos de entrada y un botón de guardar.

**Verificación:** En la vista de árbol (panel izquierdo), confirma que existen exactamente 6 pantallas y que `scr_NuevoProducto` contiene al menos 4 controles `TextInput` y 1 botón.

---

### Paso 2 — Optimizar fórmula con Copilot

**Objetivo:** Usar Copilot para sugerir una mejora a una fórmula existente en la aplicación.

**Instrucciones:**

1. Navega a la pantalla que contiene la galería de productos (por ejemplo, `scr_Productos` o la pantalla con `gal_ListaProductos`).
2. Selecciona la galería y haz clic en su propiedad **Items** en la barra de fórmulas.
3. Haz clic en el ícono de **Copilot** junto a la barra de fórmulas.
4. Escribe el siguiente prompt:

```
Mejora esta fórmula para que filtre los productos cuya cantidad sea mayor a cero
```

5. Revisa la sugerencia de Copilot. Debería proponer algo similar a:

```powerapps
Filter(Inventario_Lab, Cantidad > 0)
```

6. Si la sugerencia es adecuada para tu contexto, haz clic en **Aplicar**. Si no es exacta, ajústala manualmente para que coincida con el nombre real de tu origen de datos y columna.

**Resultado esperado:** La propiedad `Items` de la galería contiene una fórmula `Filter()` que excluye productos con cantidad cero.

**Verificación:** Ejecuta la aplicación en modo **Vista previa** (F5) y confirma que la galería no muestra productos con cantidad = 0. Si todos los registros tienen cantidad > 0, agrega temporalmente un registro con cantidad 0 en SharePoint para validar.

---

### Paso 3 — Crear flujo en Power Automate

**Objetivo:** Crear el flujo `Flujo_NotificacionInventario` como flujo de nube instantáneo activado desde Power Apps.

**Instrucciones:**

1. Abre una nueva pestaña del navegador y navega a **make.powerautomate.com**.
2. Verifica que el entorno sea **LabEnvironment-PowerApps** (selector superior derecho).
3. En el menú lateral, haz clic en **+ Crear**.
4. Selecciona **Flujo de nube instantáneo**.
5. En el campo **Nombre del flujo**, escribe:

```
Flujo_NotificacionInventario
```

6. En la sección **Elegir cómo desencadenar este flujo**, selecciona **PowerApps (V2)**.
7. Haz clic en **Crear**.
8. En el diseñador del flujo, haz clic en el trigger **PowerApps (V2)** para expandirlo.
9. Haz clic en **+ Agregar una entrada** y selecciona **Texto**.
10. En el campo de nombre del parámetro, escribe:

```
MensajeInventario
```

11. Haz clic en **+ Nuevo paso**.
12. Busca **"Send an email (V2)"** y selecciona la acción **Enviar un correo electrónico (V2)** del conector **Office 365 Outlook**.
13. Configura la acción con los siguientes valores:
    - **Para:** tu dirección de correo electrónico de Microsoft 365 (ejemplo: `estudiante@[tenant].onmicrosoft.com`)
    - **Asunto:** `Alerta de Inventario`
    - **Cuerpo:** Haz clic en el campo, luego selecciona **Contenido dinámico** y elige el parámetro `MensajeInventario` del trigger.

14. Haz clic en **Guardar** en la esquina superior derecha.
15. Verifica que el flujo muestre el estado **Activado** (ícono verde).

**Resultado esperado:** El flujo `Flujo_NotificacionInventario` aparece guardado y activado con un trigger PowerApps (V2) y una acción de envío de correo.

**Verificación:** En la lista **Mis flujos**, confirma que `Flujo_NotificacionInventario` aparece con estado **Activado** y que la última modificación corresponde al momento actual.

---

### Paso 4 — Conectar el flujo a la aplicación

**Objetivo:** Vincular el flujo a la aplicación y configurar un botón para ejecutarlo desde `Pantalla_Carrito`.

**Instrucciones:**

1. Regresa a la pestaña de **Power Apps Studio** con tu aplicación abierta.
2. En el menú lateral izquierdo, haz clic en el ícono de **Power Automate** (rayo/lightning bolt).
3. En el panel que se abre, haz clic en **+ Agregar flujo**.
4. Busca y selecciona **Flujo_NotificacionInventario** de la lista. Si no aparece, verifica que estás en el entorno correcto y que el flujo está activado.
5. El flujo ahora aparece vinculado a la aplicación en el panel de Power Automate.
6. Navega a la pantalla **Pantalla_Carrito** (o `scr_Carrito` según tu nomenclatura del Lab 04).
7. Inserta un nuevo botón: **Insertar > Botón**.
8. Renombra el botón a **`btn_Notificar`**.
9. Establece la propiedad `Text` del botón:

```powerapps
"Enviar Notificación"
```

10. Selecciona el botón `btn_Notificar` y en su propiedad **OnSelect**, escribe la siguiente fórmula:

```powerapps
Flujo_NotificacionInventario.Run(
    "Hay " & CountRows(colCarritoTrabajo) & " items en el carrito de trabajo"
)
```

11. Si el nombre del flujo en el intellisense aparece diferente (por ejemplo, con guiones bajos o sin acentos), usa el nombre exacto que muestra el autocompletado.

**Resultado esperado:** El botón `btn_Notificar` en `Pantalla_Carrito` está configurado para ejecutar el flujo pasando un mensaje dinámico con la cantidad de ítems del carrito.

**Verificación:** Observa que no hay errores (subrayado rojo) en la barra de fórmulas al seleccionar el botón. El nombre del flujo debe aparecer en el intellisense sin errores de referencia.

---

### Paso 5 — Aplicar buenas prácticas y manejo de errores

**Objetivo:** Implementar `IfError()` en la fórmula de guardado, verificar nomenclatura de controles y organizar el árbol de componentes.

**Instrucciones:**

1. Navega a la pantalla **`scr_NuevoProducto`** (creada en el Paso 1).
2. Selecciona el botón **`btn_GuardarProducto`** y establece su propiedad **OnSelect** con la siguiente fórmula que incluye manejo de errores:

```powerapps
IfError(
    Patch(
        Inventario_Lab,
        Defaults(Inventario_Lab),
        {
            Nombre: txt_NombreProducto.Text,
            Categoria: txt_CategoriaProducto.Text,
            Cantidad: Value(txt_CantidadProducto.Text),
            Precio: Value(txt_PrecioProducto.Text)
        }
    );
    Notify("Producto guardado exitosamente", NotificationType.Success);
    Navigate(scr_Productos, ScreenTransition.Fade),
    Notify("Error al guardar el producto. Verifica los datos.", NotificationType.Error)
)
```

3. Ajusta los nombres de controles en la fórmula si difieren de los que tienes (usa los nombres exactos del árbol de componentes).
4. **Revisión de nomenclatura:** Recorre el árbol de componentes completo (panel izquierdo) y verifica que TODOS los controles sigan la convención de prefijos:
   - `lbl_` para Labels
   - `btn_` para Buttons
   - `txt_` para TextInput
   - `gal_` para Galleries
   - `scr_` para Screens
   - `frm_` para Forms
   - `img_` para Images

5. Renombra cualquier control que aún tenga nombre genérico (como `Button1`, `Label3`, `Screen7`).
6. **Organización del árbol:** Verifica que las 6 pantallas estén en un orden lógico:
   - `scr_Inicio`
   - `scr_Productos` (o equivalente con galería)
   - `scr_Detalle`
   - `scr_Carrito` (Pantalla_Carrito)
   - `scr_NuevoProducto`
   - Pantalla adicional según tu app (configuración, confirmación, etc.)

7. Para reordenar pantallas, haz clic derecho sobre una pantalla en el árbol y selecciona **Mover arriba** o **Mover abajo**.

**Resultado esperado:** La fórmula de guardado incluye `IfError()` con notificaciones de éxito y error. Todos los controles siguen la convención de nombrado con prefijos.

**Verificación:** Selecciona `btn_GuardarProducto` y confirma que la barra de fórmulas no muestra errores. Revisa visualmente el árbol y confirma que no existen controles con nombres genéricos.

---

### Paso 6 — Probar y publicar la aplicación

**Objetivo:** Validar la funcionalidad completa y publicar la versión final.

**Instrucciones:**

1. Haz clic en el botón **▶ (Vista previa)** o presiona **F5** para ejecutar la aplicación.
2. Navega a `Pantalla_Carrito` y agrega al menos un ítem a `colCarritoTrabajo` (según la lógica de tu app del Lab 04).
3. Haz clic en el botón **Enviar Notificación** (`btn_Notificar`).
4. Espera 10-30 segundos y revisa tu bandeja de entrada de correo electrónico de Microsoft 365.
5. Confirma que recibiste un correo con:
   - **Asunto:** `Alerta de Inventario`
   - **Cuerpo:** `Hay X items en el carrito de trabajo` (donde X es el número de ítems)
6. Navega a `scr_NuevoProducto`, completa los campos con datos de prueba y haz clic en **Guardar Producto**.
7. Confirma que aparece la notificación de éxito y que navegas a la pantalla de productos.
8. Presiona **Esc** para salir de la vista previa.
9. Haz clic en **Publicar** (ícono de publicar en la esquina superior derecha o **Archivo > Publicar**).
10. En el diálogo de confirmación, haz clic en **Publicar esta versión**.

**Resultado esperado:** La aplicación funciona correctamente con las 6 pantallas, el flujo envía el correo y la aplicación queda publicada.

**Verificación:** En `make.powerapps.com > Aplicaciones`, confirma que **AppInventario_[InicialNombre]** muestra la fecha y hora actual como última publicación.

---

## Validación y Pruebas

| Criterio | Método de verificación | Resultado esperado |
|----------|----------------------|-------------------|
| 6 pantallas en la app | Contar en el árbol de componentes | Exactamente 6 pantallas visibles |
| Flujo ejecuta correctamente | Clic en `btn_Notificar` en vista previa | Correo recibido en bandeja de entrada |
| Copilot fue utilizado | Pantalla `scr_NuevoProducto` existe con formulario | Pantalla funcional con 4 campos de entrada |
| Manejo de errores | Probar `btn_GuardarProducto` con campo vacío | Mensaje de error aparece sin crash |
| Nomenclatura correcta | Inspección visual del árbol completo | Todos los controles con prefijos `lbl_`, `btn_`, `txt_`, etc. |
| Aplicación publicada | Verificar en `make.powerapps.com` | Estado "Publicada" con fecha actual |

---

## Solución de Problemas

### Problema 1: El flujo no aparece al intentar agregarlo en Power Apps Studio

**Síntomas:** Al hacer clic en **+ Agregar flujo** en el panel de Power Automate dentro de Power Apps Studio, la lista está vacía o el flujo `Flujo_NotificacionInventario` no aparece.

**Causa:** El flujo fue creado en un entorno diferente al que está seleccionado en Power Apps Studio, o el flujo no tiene el trigger `PowerApps (V2)` configurado correctamente.

**Solución:**
1. Verifica que tanto Power Apps Studio como Power Automate estén en el entorno **LabEnvironment-PowerApps** (selector superior derecho en ambos portales).
2. En Power Automate, abre el flujo y confirma que el trigger es **PowerApps (V2)** (no "PowerApps" sin V2, ni "Manualmente").
3. Guarda el flujo nuevamente en Power Automate.
4. En Power Apps Studio, cierra y reabre el panel de Power Automate, luego haz clic en **Actualizar** antes de buscar el flujo.

---

### Problema 2: El correo electrónico no llega después de ejecutar el flujo desde la app

**Síntomas:** Al hacer clic en `btn_Notificar`, no se muestra error en la app pero el correo nunca llega a la bandeja de entrada.

**Causa:** La conexión de Office 365 Outlook en el flujo no está autenticada correctamente, o el flujo falló silenciosamente por un problema de permisos DLP.

**Solución:**
1. Abre **make.powerautomate.com** y navega a **Mis flujos > Flujo_NotificacionInventario**.
2. Haz clic en el historial de ejecuciones (**Historial de ejecución de 28 días**).
3. Si la última ejecución muestra estado **Error**, haz clic para ver el detalle del fallo.
4. Si el error indica problemas de conexión: haz clic en **Editar** en el flujo, selecciona la acción de correo y haz clic en los tres puntos (**...**) > **Mis conexiones** para reautenticar la conexión de Office 365 Outlook.
5. Si el error indica DLP: contacta al instructor para verificar que la política de prevención de pérdida de datos permite el conector Office 365 Outlook en el entorno de laboratorio.
6. Revisa también la carpeta de **Correo no deseado/Spam** en tu buzón.

---

## Limpieza

No se requiere limpieza para este laboratorio. La aplicación publicada y el flujo deben permanecer activos como entregable final del curso. Si el instructor lo indica:

1. **No elimines** la aplicación ni el flujo — son el producto final evaluable.
2. Si necesitas liberar espacio en el entorno para otros estudiantes, el instructor coordinará la eliminación después de la evaluación.

---

## Resumen

En este laboratorio completaste la integración final de tu aplicación **AppInventario_[InicialNombre]**:

- **Copilot** generó una pantalla de formulario a partir de una instrucción en lenguaje natural, demostrando cómo la IA acelera el desarrollo sin reemplazar el criterio del creador.
- **Power Automate** se conectó a la aplicación para enviar notificaciones por correo electrónico, extendiendo las capacidades más allá de la interfaz de usuario.
- **Buenas prácticas** de nombrado (`btn_`, `txt_`, `scr_`), manejo de errores (`IfError()`) y organización del árbol de componentes aseguran mantenibilidad y profesionalismo.
- La aplicación final cuenta con **6 pantallas**, un flujo funcional y está **publicada** en el entorno de laboratorio.

### Recursos adicionales

- [Documentación de Copilot en Power Apps](https://learn.microsoft.com/es-es/power-apps/maker/canvas-apps/ai-overview)
- [Crear flujos instantáneos desde Power Apps](https://learn.microsoft.com/es-es/power-automate/create-flow-solution)
- [Función IfError() en Power Fx](https://learn.microsoft.com/es-es/power-platform/power-fx/reference/function-iferror)
- [Buenas prácticas de nomenclatura en Power Apps](https://learn.microsoft.com/es-es/power-apps/guidance/coding-guidelines/code-readability)

---
