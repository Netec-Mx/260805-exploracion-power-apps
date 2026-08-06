# Implementación de Lógica de Negocio con Power Fx en AppInventario

## Metadatos

| Campo | Valor |
|-------|-------|
| **Duración** | 8 minutos |
| **Complejidad** | Media |
| **Nivel Bloom** | Aplicar |

## Descripción General

En este laboratorio implementarás la capa completa de lógica de negocio en la aplicación `AppInventario_[InicialNombre]` construida en los Labs 02 y 03. Añadirás búsqueda en tiempo real con `Search()`, una colección local `colCarritoTrabajo` para gestionar items seleccionados, navegación condicional con paso de contexto hacia una pantalla de detalle, y contadores dinámicos con `CountRows()`. Al finalizar, la aplicación tendrá 5 pantallas funcionales con lógica interactiva completa.

## Objetivos de Aprendizaje

- [ ] Implementar fórmulas Power Fx (`Filter`, `Search`, `Sort`, `If`, `Concatenate`, `CountRows`) para manipular y presentar datos de SharePoint
- [ ] Utilizar variables globales (`Set`) y variables de contexto (`UpdateContext`) para gestionar el estado entre pantallas
- [ ] Crear y manipular una colección local (`ClearCollect`, `Collect`, `Remove`) como "carrito de trabajo"
- [ ] Implementar navegación condicional entre pantallas pasando parámetros de contexto con `Navigate`

## Prerrequisitos

### Conocimiento Previo

- Completar **Lab 03-00-01**: aplicación `AppInventario_[InicialNombre]` con 4 pantallas conectada a SharePoint `Inventario_Lab` y Dataverse `Productos_Lab`
- Comprensión básica de la sintaxis Power Fx (cubierta en el módulo teórico 4.1)
- Familiaridad con el diseñador Power Apps Canvas Studio (barra de fórmulas, panel de propiedades, vista de árbol)

### Acceso Requerido

- Cuenta Microsoft 365 con licencia Power Apps (E3 o Power Apps per-user plan)
- Acceso al entorno `LabEnvironment-PowerApps` en make.powerapps.com
- Lista SharePoint `Inventario_Lab` con mínimo 10 registros activos en `https://[tenant].sharepoint.com/sites/LabSite-PowerApps`

## Entorno del Laboratorio

| Componente | Requisito |
|------------|-----------|
| Navegador | Chrome 124+ o Edge 124+ |
| Resolución | 1920×1080 recomendado |
| Conexión | 10 Mbps mínimo |
| Entorno Power Platform | `LabEnvironment-PowerApps` |

### Verificación Inicial

1. Abre `https://make.powerapps.com`
2. En el selector superior derecho, confirma que el entorno sea **LabEnvironment-PowerApps**
3. En **Aplicaciones**, localiza `AppInventario_[InicialNombre]` y haz clic en **Editar**
4. Verifica en la vista de árbol (panel izquierdo) que existen las pantallas: `scr_Inicio`, `scr_Lista` (o `Pantalla_Lista`), `scr_Detalle` (o `Pantalla_Detalle`) y `scr_Configuracion`

---

## Paso a Paso

### Paso 1: Agregar búsqueda en tiempo real en Pantalla_Lista

**Objetivo:** Insertar un control `TextInput` y configurar la propiedad `Items` de la Gallery para filtrar registros mediante `Search()`.

**Instrucciones:**

1. En la vista de árbol, selecciona la pantalla `scr_Lista` (o `Pantalla_Lista`).
2. En el menú superior, haz clic en **Insertar** → **Entrada de texto** (TextInput).
3. Renombra el control a `txt_Buscar` (panel derecho → campo Nombre o doble clic en la vista de árbol).
4. Posiciona `txt_Buscar` en la parte superior de la pantalla, encima de la Gallery existente. Ajusta las propiedades de posición:
   - `X`: 20
   - `Y`: 80
   - `Width`: 400
   - `Height`: 50
5. En la propiedad `HintText` de `txt_Buscar`, escribe:

```plaintext
"Buscar por nombre o categoría..."
```

6. Selecciona la Gallery conectada a SharePoint (debería llamarse `gal_ListaProductos` o similar).
7. En la propiedad **`Items`** de la Gallery, reemplaza la fórmula actual por:

```plaintext
Sort(
    Search(
        'Inventario_Lab',
        txt_Buscar.Text,
        "Nombre",
        "Categoria"
    ),
    Nombre,
    SortOrder.Ascending
)
```

8. En la propiedad `OnChange` de `txt_Buscar`, escribe:

```plaintext
Reset(txt_Buscar)
```

> **Nota:** La función `Search()` busca coincidencias parciales (contiene) en las columnas especificadas. Cuando `txt_Buscar.Text` está vacío, `Search()` devuelve todos los registros.

**Resultado Esperado:**

La Gallery muestra todos los registros al inicio. Al escribir texto en `txt_Buscar`, la Gallery se filtra en tiempo real mostrando solo los items cuyo `Nombre` o `Categoria` contenga el texto ingresado.

**Verificación:**

- Escribe "elec" en `txt_Buscar` → solo aparecen items con "elec" en nombre o categoría
- Borra el texto → aparecen todos los registros nuevamente
- Los registros aparecen ordenados alfabéticamente por Nombre

---

### Paso 2: Agregar botón para añadir items a la colección colCarritoTrabajo

**Objetivo:** Crear un botón dentro de la Gallery que ejecute `Collect()` para añadir el item seleccionado a una colección global.

**Instrucciones:**

1. Con la pantalla `scr_Lista` activa, selecciona la Gallery `gal_ListaProductos`.
2. Haz clic en el ícono de lápiz (editar plantilla) dentro de la Gallery para entrar en modo de edición de la plantilla.
3. Inserta un botón dentro de la plantilla de la Gallery: **Insertar** → **Botón**.
4. Renombra el botón a `btn_Agregar`.
5. Configura las siguientes propiedades de `btn_Agregar`:

| Propiedad | Valor |
|-----------|-------|
| `Text` | `"+ Carrito"` |
| `Width` | `100` |
| `Height` | `35` |
| `X` | Posiciónalo al lado derecho dentro de la plantilla |

6. En la propiedad **`OnSelect`** de `btn_Agregar`, escribe:

```plaintext
If(
    IsBlank(LookUp(colCarritoTrabajo, ID = ThisItem.ID)),
    Collect(colCarritoTrabajo, ThisItem);
    Set(varContadorCarrito, CountRows(colCarritoTrabajo));
    Notify("Item agregado al carrito", NotificationType.Success),
    Notify("Este item ya está en el carrito", NotificationType.Warning)
)
```

7. Para inicializar la colección al abrir la aplicación, selecciona la pantalla `scr_Inicio` y en su propiedad **`OnVisible`**, agrega al inicio:

```plaintext
ClearCollect(colCarritoTrabajo, Blank());
Clear(colCarritoTrabajo);
Set(varContadorCarrito, 0)
```

> **Explicación:** La fórmula del botón verifica con `LookUp` si el item ya existe en la colección antes de agregarlo, evitando duplicados. `Set(varContadorCarrito, ...)` actualiza una variable global que usaremos como contador.

**Resultado Esperado:**

Al presionar `btn_Agregar` en cualquier fila de la Gallery, el item se añade a `colCarritoTrabajo` y aparece una notificación verde. Si se intenta agregar el mismo item, aparece una advertencia amarilla.

**Verificación:**

- Presiona `btn_Agregar` en un item → aparece notificación "Item agregado al carrito"
- Presiona el mismo botón nuevamente → aparece "Este item ya está en el carrito"
- En el menú **Variables** (vista de árbol → Variables), confirma que `colCarritoTrabajo` contiene el registro

---

### Paso 3: Crear Pantalla_Carrito con visualización y eliminación de items

**Objetivo:** Crear una quinta pantalla que muestre la colección `colCarritoTrabajo` en una Gallery con la opción de eliminar items.

**Instrucciones:**

1. En la vista de árbol, haz clic derecho → **Nueva pantalla** → **En blanco**.
2. Renombra la pantalla a `scr_Carrito`.
3. Inserta una etiqueta (Label) en la parte superior:
   - Nombre: `lbl_TituloCarrito`
   - Propiedad `Text`: `"Carrito de Trabajo (" & CountRows(colCarritoTrabajo) & " items)"`
   - `Size`: 20
   - `FontWeight`: `FontWeight.Bold`
   - `X`: 20, `Y`: 20, `Width`: 500

4. Inserta una **Gallery vertical en blanco**: **Insertar** → **Galería** → **Vertical en blanco**.
5. Renombra la Gallery a `gal_Carrito`.
6. En la propiedad **`Items`** de `gal_Carrito`, escribe:

```plaintext
colCarritoTrabajo
```

7. Dentro de la plantilla de `gal_Carrito`, inserta los siguientes controles:

   a. **Label** para el nombre:
   - Nombre: `lbl_NombreCarrito`
   - `Text`: `ThisItem.Nombre`

   b. **Label** para la categoría:
   - Nombre: `lbl_CategoriaCarrito`
   - `Text`: `ThisItem.Categoria`

   c. **Label** para cantidad y precio:
   - Nombre: `lbl_InfoCarrito`
   - `Text`: `"Cant: " & ThisItem.Cantidad & " | $" & ThisItem.Precio`

   d. **Botón** para eliminar:
   - Nombre: `btn_EliminarItem`
   - `Text`: `"✕ Quitar"`
   - Propiedad **`OnSelect`**:

```plaintext
Remove(colCarritoTrabajo, ThisItem);
Set(varContadorCarrito, CountRows(colCarritoTrabajo))
```

8. Inserta un botón de navegación para regresar:
   - Nombre: `btn_VolverLista`
   - `Text`: `"← Volver a Lista"`
   - `OnSelect`: `Navigate(scr_Lista, ScreenTransition.UnCoverRight)`
   - Posición: parte inferior de la pantalla

9. Agrega navegación hacia `scr_Carrito` desde `scr_Lista`. Inserta un botón en `scr_Lista`:
   - Nombre: `btn_VerCarrito`
   - `Text`: `"🛒 Carrito (" & varContadorCarrito & ")"`
   - `OnSelect`: `Navigate(scr_Carrito, ScreenTransition.CoverLeft)`
   - Posición: esquina superior derecha de `scr_Lista`

**Resultado Esperado:**

La pantalla `scr_Carrito` muestra todos los items agregados previamente. Cada item tiene un botón para eliminarlo. El título refleja dinámicamente la cantidad de items.

**Verificación:**

- Navega a `scr_Carrito` → se muestran los items agregados en el Paso 2
- Presiona "✕ Quitar" en un item → desaparece de la Gallery y el contador se actualiza
- El botón "← Volver a Lista" regresa correctamente a `scr_Lista`

---

### Paso 4: Implementar navegación con contexto hacia Pantalla_Detalle

**Objetivo:** Configurar la Gallery de `scr_Lista` para que al seleccionar un item, navegue a `scr_Detalle` pasando el registro completo como variable de contexto.

**Instrucciones:**

1. En `scr_Lista`, selecciona la Gallery `gal_ListaProductos`.
2. En la propiedad **`OnSelect`** de la Gallery (no del botón `btn_Agregar`), escribe:

```plaintext
Navigate(
    scr_Detalle,
    ScreenTransition.Cover,
    {varItemSeleccionado: ThisItem}
)
```

> **Nota:** Si la Gallery ya tiene un `OnSelect`, reemplázalo. El parámetro `{varItemSeleccionado: ThisItem}` pasa todo el registro como variable de contexto a la pantalla destino.

3. Selecciona la pantalla `scr_Detalle` (o `Pantalla_Detalle`).
4. Elimina o reemplaza el contenido existente de detalle. Inserta las siguientes etiquetas:

   a. **Título del producto:**
   - Nombre: `lbl_DetalleNombre`
   - `Text`: `varItemSeleccionado.Nombre`
   - `Size`: 24, `FontWeight`: `FontWeight.Bold`
   - `X`: 30, `Y`: 80

   b. **Categoría:**
   - Nombre: `lbl_DetalleCategoria`
   - `Text`: `"Categoría: " & varItemSeleccionado.Categoria`
   - `Y`: 130

   c. **Cantidad en stock:**
   - Nombre: `lbl_DetalleCantidad`
   - `Text`: `"Cantidad en stock: " & Text(varItemSeleccionado.Cantidad)`
   - `Y`: 170

   d. **Precio:**
   - Nombre: `lbl_DetallePrecio`
   - `Text`: `"Precio unitario: $" & Text(varItemSeleccionado.Precio, "#,##0.00")`
   - `Y`: 210

   e. **Valor total del stock:**
   - Nombre: `lbl_DetalleValorTotal`
   - `Text`: `"Valor total: $" & Text(varItemSeleccionado.Cantidad * varItemSeleccionado.Precio, "#,##0.00")`
   - `Y`: 250
   - `Color`: `Color.DarkBlue`
   - `FontWeight`: `FontWeight.Semibold`

5. Inserta un botón para regresar:
   - Nombre: `btn_VolverDesdeDetalle`
   - `Text`: `"← Regresar"`
   - `OnSelect`: `Navigate(scr_Lista, ScreenTransition.UnCover)`
   - Posición: parte inferior o superior

6. Inserta un botón para agregar al carrito desde el detalle:
   - Nombre: `btn_AgregarDesdeDetalle`
   - `Text`: `"+ Agregar al Carrito"`
   - `OnSelect`:

```plaintext
If(
    IsBlank(LookUp(colCarritoTrabajo, ID = varItemSeleccionado.ID)),
    Collect(colCarritoTrabajo, varItemSeleccionado);
    Set(varContadorCarrito, CountRows(colCarritoTrabajo));
    Notify("Agregado desde detalle", NotificationType.Success),
    Notify("Ya existe en el carrito", NotificationType.Warning)
)
```

**Resultado Esperado:**

Al tocar un item en la Gallery de `scr_Lista`, la aplicación navega a `scr_Detalle` mostrando toda la información del registro seleccionado: nombre, categoría, cantidad, precio y valor total calculado.

**Verificación:**

- En `scr_Lista`, toca cualquier item (fuera del botón `btn_Agregar`) → navega a `scr_Detalle`
- Todos los campos muestran datos correctos del item seleccionado
- El valor total se calcula correctamente (Cantidad × Precio)
- El botón "← Regresar" devuelve a `scr_Lista`

---

### Paso 5: Agregar contador dinámico en Pantalla_Inicio

**Objetivo:** Mostrar en la pantalla de inicio un indicador con el número de items en `colCarritoTrabajo` usando `CountRows()`.

**Instrucciones:**

1. Selecciona la pantalla `scr_Inicio`.
2. Inserta una etiqueta (Label):
   - Nombre: `lbl_ContadorCarrito`
   - Posición: zona visible prominente (por ejemplo, `X`: 30, `Y`: 300, `Width`: 350, `Height`: 50)

3. En la propiedad **`Text`** de `lbl_ContadorCarrito`, escribe:

```plaintext
If(
    varContadorCarrito > 0,
    "🛒 Tienes " & varContadorCarrito & " item(s) en tu carrito de trabajo",
    "🛒 Tu carrito de trabajo está vacío"
)
```

4. Configura propiedades visuales condicionales. En la propiedad **`Color`**:

```plaintext
If(varContadorCarrito > 0, Color.DarkGreen, Color.Gray)
```

5. En la propiedad **`FontWeight`**:

```plaintext
If(varContadorCarrito > 0, FontWeight.Bold, FontWeight.Normal)
```

6. **Guarda la aplicación**: `Ctrl + S` → Asigna un comentario de versión: "Lab 04: Lógica Power Fx completa".

**Resultado Esperado:**

La pantalla de inicio muestra un mensaje dinámico que refleja la cantidad de items en el carrito. Si hay items, el texto es verde y en negritas; si está vacío, aparece gris con texto normal.

**Verificación:**

- Al iniciar la app (sin items en carrito) → muestra "Tu carrito de trabajo está vacío" en gris
- Navega a `scr_Lista`, agrega 2 items, regresa a `scr_Inicio` → muestra "Tienes 2 item(s) en tu carrito de trabajo" en verde

---

## Validación y Pruebas

Ejecuta la siguiente secuencia completa para validar la funcionalidad integrada:

| # | Acción | Resultado Esperado |
|---|--------|-------------------|
| 1 | Abre la app en modo vista previa (`F5` o botón ▶) | Inicia en `scr_Inicio`, contador muestra "carrito vacío" |
| 2 | Navega a `scr_Lista` | Gallery muestra los 10+ registros de SharePoint ordenados |
| 3 | Escribe "acc" en `txt_Buscar` | Gallery filtra mostrando solo items con "acc" en nombre/categoría |
| 4 | Borra el texto de búsqueda | Gallery restaura todos los registros |
| 5 | Presiona `btn_Agregar` en el primer item | Notificación verde: "Item agregado al carrito" |
| 6 | Presiona `btn_Agregar` en el segundo item | Notificación verde nuevamente |
| 7 | Presiona `btn_Agregar` en el primer item otra vez | Notificación amarilla: "Este item ya está en el carrito" |
| 8 | Toca el tercer item (cuerpo de la Gallery) | Navega a `scr_Detalle` con datos del tercer item |
| 9 | Verifica campos en `scr_Detalle` | Nombre, categoría, cantidad, precio y valor total correctos |
| 10 | Presiona "← Regresar" | Vuelve a `scr_Lista` |
| 11 | Presiona `btn_VerCarrito` | Navega a `scr_Carrito`, muestra 2 items, título dice "(2 items)" |
| 12 | Presiona "✕ Quitar" en un item | Item desaparece, título actualiza a "(1 items)" |
| 13 | Presiona "← Volver a Lista" | Regresa a `scr_Lista` |
| 14 | Navega a `scr_Inicio` | Contador muestra "Tienes 1 item(s) en tu carrito de trabajo" en verde |

---

## Solución de Problemas

### Problema 1: La Gallery no filtra al escribir en txt_Buscar

**Síntomas:** Al escribir texto en el campo de búsqueda, la Gallery sigue mostrando todos los registros sin cambio alguno.

**Causa:** La función `Search()` requiere que las columnas especificadas sean de tipo texto (una línea de texto). Si las columnas `Nombre` o `Categoria` en SharePoint son de tipo diferente (por ejemplo, "Opción" o "Varias líneas de texto"), `Search()` no funciona. Otra causa común es que el nombre de la lista o las columnas en la fórmula no coincidan exactamente con los nombres internos en SharePoint.

**Solución:**
1. Verifica en SharePoint que las columnas `Nombre` y `Categoria` sean de tipo **Una línea de texto**.
2. En la fórmula `Search()`, usa los nombres exactos de columna tal como aparecen en la conexión de datos (respetando acentos y mayúsculas).
3. Si el problema persiste, reemplaza `Search()` por `Filter()` con `in`:

```plaintext
Filter(
    'Inventario_Lab',
    txt_Buscar.Text in Nombre Or txt_Buscar.Text in Categoria
)
```

---

### Problema 2: La variable varItemSeleccionado aparece en blanco en scr_Detalle

**Síntomas:** Al navegar a la pantalla de detalle, todas las etiquetas muestran valores vacíos o el error "Name isn't valid".

**Causa:** El `OnSelect` de la Gallery está siendo interceptado por el `OnSelect` del botón `btn_Agregar` dentro de la plantilla. Cuando el usuario toca el botón en lugar del cuerpo de la fila, se ejecuta `Collect()` pero no `Navigate()`. Alternativamente, el nombre de la variable de contexto en `Navigate()` no coincide con el usado en las etiquetas de `scr_Detalle`.

**Solución:**
1. Verifica que el nombre de la variable sea **exactamente** `varItemSeleccionado` tanto en la fórmula `Navigate(..., {varItemSeleccionado: ThisItem})` como en las referencias `varItemSeleccionado.Nombre` en las etiquetas.
2. Para evitar conflictos con el botón, inserta un ícono de flecha (`>`) o un área transparente en la plantilla de la Gallery dedicada exclusivamente a la navegación, con su propio `OnSelect`:

```plaintext
Navigate(scr_Detalle, ScreenTransition.Cover, {varItemSeleccionado: ThisItem})
```

3. Asegúrate de que la propiedad `OnSelect` de la Gallery misma (no de controles internos) contenga la fórmula de navegación.

---

## Limpieza

Al finalizar el laboratorio:

1. **Guarda la aplicación** con `Ctrl + S` y añade el comentario de versión: `"Lab 04 completo - Lógica Power Fx"`.
2. **Cierra el modo de edición** haciendo clic en "← Atrás" para volver a la lista de aplicaciones.
3. **No elimines** la aplicación ni la colección; serán necesarias para los laboratorios siguientes.
4. Si creaste controles de prueba temporales durante la depuración, elimínalos para mantener la aplicación limpia.

---

## Resumen

En este laboratorio implementaste la capa de lógica de negocio completa de `AppInventario_[InicialNombre]`:

| Concepto Implementado | Función Power Fx | Ubicación |
|----------------------|------------------|-----------|
| Búsqueda en tiempo real | `Search()`, `Sort()` | `scr_Lista` → Gallery Items |
| Colección como carrito | `Collect()`, `Remove()`, `Clear()` | `btn_Agregar`, `scr_Carrito` |
| Prevención de duplicados | `IsBlank()`, `LookUp()` | `btn_Agregar` OnSelect |
| Navegación con contexto | `Navigate()` + `UpdateContext` implícito | Gallery → `scr_Detalle` |
| Contadores dinámicos | `CountRows()`, `Set()` | `scr_Inicio`, `scr_Carrito` |
| Formato condicional | `If()` en propiedades `Color`, `FontWeight` | `lbl_ContadorCarrito` |

### Recursos Adicionales

- [Función Search en Power Apps — Microsoft Learn](https://learn.microsoft.com/es-es/power-apps/maker/canvas-apps/functions/function-filter-lookup)
- [Variables y colecciones en Canvas Apps](https://learn.microsoft.com/es-es/power-apps/maker/canvas-apps/working-with-variables)
- [Navigate y Back en Power Apps](https://learn.microsoft.com/es-es/power-apps/maker/canvas-apps/functions/function-navigate)
