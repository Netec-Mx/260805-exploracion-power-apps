# Creación de una Aplicación Canvas con Navegación y Gallery

## Metadatos

| Campo | Valor |
|-------|-------|
| **Duración** | 6 minutos |
| **Complejidad** | Fácil |
| **Nivel Bloom** | Aplicar |
| **Tipo de aplicación** | Canvas App (Lienzo) |
| **Entorno** | LabEnvironment-PowerApps |

---

## Descripción General

En este laboratorio crearás desde cero una aplicación Canvas llamada `AppInventario_[InicialNombre]` en formato tableta dentro del entorno `LabEnvironment-PowerApps`. La aplicación contendrá tres pantallas con navegación funcional entre ellas y una Gallery conectada a un archivo Excel en OneDrive. Este ejercicio aplica directamente el concepto de Canvas App aprendido en la lección 2.1: diseño visual libre, múltiples controles y conexión a una fuente de datos externa (Excel/OneDrive).

---

## Objetivos de Aprendizaje

- [ ] Crear una aplicación Canvas nueva desde cero en formato tableta usando Power Apps Studio
- [ ] Identificar y utilizar los componentes del diseñador: árbol de componentes, panel de propiedades, barra de fórmulas y panel de datos
- [ ] Agregar y configurar controles básicos (Label, TextInput, Button, Gallery) en múltiples pantallas
- [ ] Implementar navegación entre 3 pantallas usando la función `Navigate()`
- [ ] Conectar un archivo Excel desde OneDrive como fuente de datos para poblar una Gallery

---

## Prerrequisitos

### Conocimiento Previo
- Completar Lab 01-00-01: entorno `LabEnvironment-PowerApps` activo y credenciales verificadas
- Comprensión básica de la interfaz de Power Platform (selector de entorno, menú de navegación)
- Familiaridad con el concepto de Canvas App según la lección 2.1

### Acceso Requerido
- Cuenta Microsoft 365 con licencia Power Apps (E3 o Power Apps per-user plan)
- Archivo `inventario_inicial.xlsx` cargado en la raíz de OneDrive del estudiante

### Estructura del Archivo Excel

El archivo `inventario_inicial.xlsx` debe contener una tabla llamada `Tabla1` con las siguientes columnas y al menos 5 registros:

| ID | Nombre | Categoria | Cantidad | Precio |
|----|--------|-----------|----------|--------|
| 1 | Teclado Mecánico | Periféricos | 25 | 89.99 |
| 2 | Monitor 27" | Pantallas | 10 | 349.00 |
| 3 | Mouse Inalámbrico | Periféricos | 50 | 45.50 |
| 4 | Webcam HD | Accesorios | 15 | 79.99 |
| 5 | Audífonos USB | Audio | 30 | 55.00 |

> **Nota:** El instructor debe proveer este archivo antes del laboratorio. Verificar que los datos estén formateados como tabla de Excel (Insertar > Tabla).

---

## Entorno del Laboratorio

### Software Requerido

| Componente | Versión | Propósito |
|------------|---------|-----------|
| Microsoft Edge o Chrome | 124.x | Navegador para acceder a Power Apps Studio |
| Power Apps Studio (web) | 3.24044.18+ | Diseñador de la aplicación Canvas |
| OneDrive for Business | Servicio cloud | Almacenamiento del archivo Excel |

### Verificación Previa

Antes de comenzar, confirmar:
1. Acceso a `https://make.powerapps.com`
2. El entorno `LabEnvironment-PowerApps` aparece en el selector superior derecho
3. El archivo `inventario_inicial.xlsx` es visible en `https://onedrive.com`

---

## Procedimiento Paso a Paso

### Paso 1: Crear la Aplicación Canvas en Blanco

**Objetivo:** Iniciar una nueva Canvas App en formato tableta dentro del entorno correcto.

**Instrucciones:**

1. Abre el navegador y navega a `https://make.powerapps.com`.
2. En la esquina superior derecha, haz clic en el selector de entorno y selecciona **LabEnvironment-PowerApps**.
3. En el panel izquierdo, haz clic en **+ Crear**.
4. En la sección "Comenzar desde", selecciona **Aplicación en blanco**.
5. En el diálogo que aparece, bajo "Aplicación de lienzo en blanco", haz clic en **Crear**.
6. En el cuadro de diálogo "Aplicación de lienzo en blanco":
   - **Nombre de la aplicación:** `AppInventario_[InicialNombre]` (ejemplo: `AppInventario_JG`)
   - **Formato:** selecciona **Tableta**
7. Haz clic en **Crear**.

**Resultado Esperado:** Power Apps Studio se abre con una pantalla en blanco llamada `Screen1` y el diseñador muestra el lienzo en formato tableta (1366×768).

**Verificación:** Confirma que en la barra de título superior aparece el nombre `AppInventario_[InicialNombre]` y que el árbol de componentes (panel izquierdo) muestra `Screen1`.

---

### Paso 2: Configurar las Tres Pantallas y Renombrarlas

**Objetivo:** Crear la estructura de 3 pantallas y aplicar la convención de nomenclatura con prefijo `scr_`.

**Instrucciones:**

1. En el árbol de componentes (panel izquierdo, ícono de pantallas/árbol), haz clic derecho sobre `Screen1` y selecciona **Cambiar nombre**.
2. Escribe `scr_Inicio` y presiona **Enter**.
3. En la barra superior o el panel de pantallas, haz clic en **Nueva pantalla** > **En blanco**.
4. Renombra la nueva pantalla como `scr_Lista`.
5. Haz clic nuevamente en **Nueva pantalla** > **En blanco**.
6. Renombra esta tercera pantalla como `scr_Detalle`.

**Resultado Esperado:** El árbol de componentes muestra tres pantallas:
```
scr_Inicio
scr_Lista
scr_Detalle
```

**Verificación:** Haz clic en cada pantalla en el árbol y confirma que el lienzo cambia mostrando una pantalla vacía diferente para cada una.

---

### Paso 3: Diseñar la Pantalla de Inicio (scr_Inicio)

**Objetivo:** Agregar controles Label, Image y Button en la pantalla de inicio con la convención de nomenclatura.

**Instrucciones:**

1. Selecciona `scr_Inicio` en el árbol de componentes.
2. En la barra superior, haz clic en **+ Insertar**.
3. Agrega un control **Etiqueta (Label)**:
   - Arrástralo al centro superior de la pantalla.
   - En el panel de propiedades (derecha), establece:
     - **Text:** `"Sistema de Inventario"`
     - **Size (tamaño de fuente):** `28`
     - **FontWeight:** `Bold`
     - **Align:** `Center`
   - En el árbol de componentes, renombra el control como `lbl_Titulo`.

4. Agrega un segundo control **Etiqueta (Label)**:
   - Colócalo debajo del título.
   - **Text:** `"Bienvenido a la aplicación de gestión de inventario"`
   - **Size:** `16`
   - **Align:** `Center`
   - Renombra como `lbl_Subtitulo`.

5. Agrega un control **Botón (Button)**:
   - Colócalo en la parte central-inferior de la pantalla.
   - **Text:** `"Ver Inventario"`
   - **Size:** `16`
   - Renombra como `btn_IrLista`.

6. Selecciona `btn_IrLista` y en la barra de fórmulas (parte superior del lienzo), selecciona la propiedad **OnSelect**.
7. Escribe la siguiente fórmula:

```
Navigate(scr_Lista, ScreenTransition.Fade)
```

**Resultado Esperado:** La pantalla `scr_Inicio` muestra un título, un subtítulo y un botón. El árbol de componentes muestra:
```
scr_Inicio
  ├── lbl_Titulo
  ├── lbl_Subtitulo
  └── btn_IrLista
```

**Verificación:** Mantén presionada la tecla **Alt** y haz clic en el botón `btn_IrLista`. La aplicación debe navegar a `scr_Lista`.

---

### Paso 4: Conectar el Archivo Excel como Fuente de Datos

**Objetivo:** Agregar el conector de Excel Online (OneDrive for Business) para acceder a los datos de inventario.

**Instrucciones:**

1. En el panel izquierdo, haz clic en el ícono de **Datos** (cilindro/base de datos).
2. Haz clic en **+ Agregar datos**.
3. En el cuadro de búsqueda, escribe `Excel Online` y selecciona **Excel Online (Business)**.
4. Si se solicita, autoriza la conexión con tus credenciales de Microsoft 365.
5. Navega a **OneDrive for Business** > selecciona el archivo `inventario_inicial.xlsx`.
6. Marca la casilla de la tabla **Tabla1** (o el nombre de la tabla que contenga los datos).
7. Haz clic en **Conectar**.

**Resultado Esperado:** En el panel de Datos aparece `Tabla1` como fuente de datos disponible. Puedes expandirla para ver las columnas: ID, Nombre, Categoria, Cantidad, Precio.

**Verificación:** Confirma que `Tabla1` aparece listada en el panel de Datos sin errores de conexión (sin íconos de advertencia).

---

### Paso 5: Diseñar la Pantalla de Lista (scr_Lista) con Gallery

**Objetivo:** Agregar una Gallery vertical conectada a los datos de Excel y un TextInput para búsqueda.

**Instrucciones:**

1. Selecciona `scr_Lista` en el árbol de componentes.
2. Haz clic en **+ Insertar** y agrega un control **Entrada de texto (TextInput)**:
   - Posiciónalo en la parte superior de la pantalla (ancho completo o casi completo).
   - En propiedades, establece **HintText:** `"Buscar producto..."`
   - Limpia la propiedad **Default** (déjala vacía: `""`).
   - Renombra como `txt_Buscar`.

3. Haz clic en **+ Insertar** > **Diseño** > **Galería vertical en blanco** (o **Galería vertical**).
   - Posiciona la Gallery debajo del TextInput, ocupando la mayor parte de la pantalla.
   - Renombra como `gal_ListaProductos`.

4. Selecciona `gal_ListaProductos` y en el panel de propiedades, establece la propiedad **Items**. En la barra de fórmulas escribe:

```
Filter(Tabla1, StartsWith(Nombre, txt_Buscar.Text))
```

5. Dentro de la Gallery, agrega los siguientes controles (haz clic dentro de la primera fila de la Gallery y luego **+ Insertar**):
   - **Label** para el nombre → propiedad **Text:** `ThisItem.Nombre` → renombrar como `lbl_NombreProducto`
   - **Label** para la categoría → propiedad **Text:** `ThisItem.Categoria` → renombrar como `lbl_CategoriaProducto`
   - **Label** para la cantidad → propiedad **Text:** `"Stock: " & ThisItem.Cantidad` → renombrar como `lbl_CantidadProducto`

6. Agrega un control **Botón (Button)** en la parte superior izquierda de la pantalla (fuera de la Gallery):
   - **Text:** `"← Inicio"`
   - **OnSelect:** `Navigate(scr_Inicio, ScreenTransition.Fade)`
   - Renombra como `btn_VolverInicio`.

**Resultado Esperado:** La pantalla `scr_Lista` muestra un campo de búsqueda en la parte superior, una Gallery poblada con los registros del archivo Excel y un botón para volver. El árbol muestra:
```
scr_Lista
  ├── txt_Buscar
  ├── gal_ListaProductos
  │     ├── lbl_NombreProducto
  │     ├── lbl_CategoriaProducto
  │     └── lbl_CantidadProducto
  └── btn_VolverInicio
```

**Verificación:** Los datos del archivo Excel deben aparecer en la Gallery. Escribe un texto en `txt_Buscar` (por ejemplo "Mon") y confirma que la Gallery filtra mostrando solo los productos cuyo nombre comienza con ese texto.

---

### Paso 6: Diseñar la Pantalla de Detalle (scr_Detalle) con Navegación

**Objetivo:** Crear una pantalla de detalle que muestre información del producto seleccionado y configurar la navegación desde la Gallery.

**Instrucciones:**

1. Selecciona `scr_Detalle` en el árbol de componentes.
2. Agrega los siguientes controles **Label**:

   | Control | Text | Nombre |
   |---------|------|--------|
   | Label título | `"Detalle del Producto"` | `lbl_TituloDetalle` |
   | Label nombre | `gal_ListaProductos.Selected.Nombre` | `lbl_DetalleNombre` |
   | Label categoría | `"Categoría: " & gal_ListaProductos.Selected.Categoria` | `lbl_DetalleCategoria` |
   | Label cantidad | `"Cantidad en stock: " & gal_ListaProductos.Selected.Cantidad` | `lbl_DetalleCantidad` |
   | Label precio | `"Precio: $" & gal_ListaProductos.Selected.Precio` | `lbl_DetallePrecio` |

3. Configura `lbl_TituloDetalle` con **Size:** `24` y **FontWeight:** `Bold`.

4. Agrega un **Botón (Button)**:
   - **Text:** `"← Volver a Lista"`
   - **OnSelect:** `Navigate(scr_Lista, ScreenTransition.Fade)`
   - Renombra como `btn_VolverLista`.

5. Regresa a `scr_Lista`. Selecciona la Gallery `gal_ListaProductos`.
6. En la barra de fórmulas, selecciona la propiedad **OnSelect** de la Gallery y escribe:

```
Navigate(scr_Detalle, ScreenTransition.Fade)
```

**Resultado Esperado:** Al hacer clic en un elemento de la Gallery, la aplicación navega a `scr_Detalle` mostrando los datos del producto seleccionado. El árbol de `scr_Detalle`:
```
scr_Detalle
  ├── lbl_TituloDetalle
  ├── lbl_DetalleNombre
  ├── lbl_DetalleCategoria
  ├── lbl_DetalleCantidad
  ├── lbl_DetallePrecio
  └── btn_VolverLista
```

**Verificación:** Usa **Alt + clic** en un producto de la Gallery → la pantalla de detalle muestra el nombre, categoría, cantidad y precio del producto seleccionado.

---

### Paso 7: Guardar y Verificar la Aplicación

**Objetivo:** Guardar la aplicación en el entorno y probar el flujo completo de navegación.

**Instrucciones:**

1. Presiona **Ctrl + S** (o haz clic en **Archivo** > **Guardar**).
2. Si es la primera vez que guardas, confirma que el nombre es `AppInventario_[InicialNombre]` y haz clic en **Guardar**.
3. Haz clic en el botón **Vista previa** (ícono ▶ en la esquina superior derecha) o presiona **F5**.
4. Prueba el flujo completo:
   - En `scr_Inicio`: haz clic en "Ver Inventario" → navega a `scr_Lista`.
   - En `scr_Lista`: verifica que la Gallery muestra datos → escribe en el buscador → confirma filtrado.
   - Haz clic en un producto → navega a `scr_Detalle` con los datos correctos.
   - Haz clic en "← Volver a Lista" → regresa a `scr_Lista`.
   - Haz clic en "← Inicio" → regresa a `scr_Inicio`.
5. Presiona **Esc** para salir de la vista previa.

**Resultado Esperado:** La navegación entre las 3 pantallas funciona sin errores. Los datos se muestran correctamente en la Gallery y en la pantalla de detalle.

**Verificación:** Todas las transiciones de pantalla funcionan. No aparecen errores en la barra de fórmulas (sin íconos rojos de error).

---

## Validación y Pruebas

Completa la siguiente lista de verificación para confirmar que el laboratorio fue exitoso:

| # | Criterio | Estado |
|---|----------|--------|
| 1 | La aplicación `AppInventario_[InicialNombre]` existe en el entorno `LabEnvironment-PowerApps` | ☐ |
| 2 | La aplicación tiene exactamente 3 pantallas: `scr_Inicio`, `scr_Lista`, `scr_Detalle` | ☐ |
| 3 | Todos los controles siguen la convención de nomenclatura (prefijos `lbl_`, `btn_`, `txt_`, `gal_`) | ☐ |
| 4 | El botón "Ver Inventario" navega de `scr_Inicio` a `scr_Lista` | ☐ |
| 5 | La Gallery `gal_ListaProductos` muestra datos del archivo Excel | ☐ |
| 6 | El filtro de búsqueda funciona correctamente | ☐ |
| 7 | Al seleccionar un producto en la Gallery, se navega a `scr_Detalle` con datos correctos | ☐ |
| 8 | Los botones "Volver" funcionan en ambas pantallas | ☐ |
| 9 | La aplicación se guardó sin errores | ☐ |

---

## Solución de Problemas

### Problema 1: La Gallery no muestra datos del archivo Excel

**Síntomas:** La Gallery `gal_ListaProductos` aparece vacía, sin registros visibles, aunque la conexión a Excel fue establecida.

**Causa:** El archivo `inventario_inicial.xlsx` no tiene los datos formateados como tabla de Excel. Power Apps requiere que los datos estén dentro de una tabla con nombre (no simplemente un rango de celdas).

**Solución:**
1. Abre el archivo `inventario_inicial.xlsx` en Excel Online o la aplicación de escritorio.
2. Selecciona todo el rango de datos (incluyendo encabezados).
3. Ve a **Insertar** > **Tabla** y confirma que "La tabla tiene encabezados" está marcado.
4. En la pestaña **Diseño de tabla**, verifica que el nombre de la tabla sea `Tabla1`.
5. Guarda el archivo en OneDrive.
6. En Power Apps Studio, ve al panel de **Datos**, elimina la conexión existente y vuelve a agregarla siguiendo el Paso 4.

---

### Problema 2: Error "Name isn't valid" al escribir la fórmula Navigate()

**Síntomas:** Al escribir `Navigate(scr_Lista, ScreenTransition.Fade)` en la propiedad OnSelect de un botón, la barra de fórmulas muestra un error rojo indicando que el nombre no es válido.

**Causa:** La pantalla destino no ha sido renombrada correctamente, o existe un error tipográfico en el nombre. Power Fx es sensible al nombre exacto del componente.

**Solución:**
1. Verifica en el árbol de componentes que la pantalla destino se llama exactamente `scr_Lista` (sin espacios adicionales ni caracteres especiales).
2. Si el nombre tiene un espacio accidental (ejemplo: `scr_Lista `), haz clic derecho en la pantalla → **Cambiar nombre** y corrígelo.
3. Reescribe la fórmula. Power Apps ofrece autocompletado: al escribir `Navigate(scr` debería aparecer una sugerencia con `scr_Lista`. Si no aparece, el nombre no coincide.
4. Confirma que estás editando la propiedad **OnSelect** (no otra propiedad como Text o Fill).

---

## Limpieza

Para este laboratorio **no se requiere limpieza**. La aplicación `AppInventario_[InicialNombre]` será utilizada y extendida en los laboratorios posteriores (Lab 03 en adelante). 

> **Importante:** No elimines la aplicación ni la conexión de datos. Ambos elementos son prerrequisitos para los siguientes laboratorios del curso.

---

## Resumen

En este laboratorio aplicaste el concepto de **Canvas App** aprendido en la lección 2.1, confirmando en la práctica que este tipo de aplicación permite:

- **Diseño visual libre:** colocaste controles en posiciones personalizadas en el lienzo
- **Múltiples fuentes de datos:** conectaste Excel desde OneDrive como fuente externa
- **Fórmulas Power Fx:** usaste `Navigate()` y `Filter()` con sintaxis similar a Excel
- **Experiencia personalizada para usuarios internos:** construiste una interfaz con título, búsqueda y navegación adaptada a un caso de inventario

### Conceptos Clave Aplicados

| Concepto | Aplicación en el Lab |
|----------|---------------------|
| Canvas App = diseño libre | Posicionamiento manual de controles en 3 pantallas |
| Conectores múltiples | Excel Online (Business) desde OneDrive |
| Power Fx declarativo | `Navigate()`, `Filter()`, `StartsWith()`, `ThisItem` |
| Convención de nomenclatura | Prefijos `scr_`, `lbl_`, `btn_`, `txt_`, `gal_` |

### Recursos Adicionales

- [Documentación: Crear una Canvas App en blanco](https://learn.microsoft.com/es-es/power-apps/maker/canvas-apps/create-blank-app)
- [Referencia: Función Navigate()](https://learn.microsoft.com/es-es/power-apps/maker/canvas-apps/functions/function-navigate)
- [Referencia: Función Filter()](https://learn.microsoft.com/es-es/power-apps/maker/canvas-apps/functions/function-filter)
- [Guía: Conectar Excel a Power Apps](https://learn.microsoft.com/es-es/power-apps/maker/canvas-apps/connections/connection-excel)

---
