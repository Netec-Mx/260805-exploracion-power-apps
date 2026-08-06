# Conexión a SharePoint Online y Dataverse — Demostración Aplicada

## Metadatos

| Campo | Detalle |
|-------|---------|
| **Duración** | 8 minutos |
| **Complejidad** | Media |
| **Nivel Bloom** | Aplicar |
| **Entorno** | LabEnvironment-PowerApps |

## Descripción General

En este laboratorio extenderás la aplicación Canvas `AppInventario_[InicialNombre]` creada en el Lab 02, reemplazando la fuente de datos Excel por la lista de SharePoint Online `Inventario_Lab` y agregando una nueva pantalla conectada a la tabla `Productos_Lab` de Dataverse. Al finalizar, la aplicación tendrá 4 pantallas funcionales con navegación completa y dos conectores de datos configurados (SharePoint y Microsoft Dataverse), sin dependencia del archivo Excel original.

## Objetivos de Aprendizaje

- [ ] Reemplazar la fuente de datos Excel por una Lista de SharePoint Online (`Inventario_Lab`) como conector de datos en la aplicación Canvas existente
- [ ] Explorar la tabla `Productos_Lab` en Dataverse identificando su estructura (columnas, tipos de datos) desde el portal de Power Apps
- [ ] Configurar una segunda conexión de datos apuntando a la tabla `Productos_Lab` de Dataverse y mostrar sus registros en una nueva pantalla
- [ ] Describir los conceptos de roles de seguridad de Dataverse y permisos de SharePoint aplicados al entorno del laboratorio

## Prerrequisitos

### Conocimientos Previos

| Requisito | Descripción |
|-----------|-------------|
| Lab 02 completado | Aplicación `AppInventario_[InicialNombre]` con 3 pantallas guardada en el entorno `LabEnvironment-PowerApps` |
| Conectores básicos | Comprensión del concepto de conector (Lección 3.1) |
| Navegación Power Apps | Experiencia con el diseñador Canvas, controles Gallery y fórmulas Navigate() |

### Acceso Requerido

| Recurso | Detalle |
|---------|---------|
| Power Apps Studio | Acceso a make.powerapps.com con licencia Microsoft 365 E3 o Power Apps per-user |
| SharePoint Online | Permisos de lectura/escritura en `https://[tenant].sharepoint.com/sites/LabSite-PowerApps` |
| Dataverse | Rol de seguridad con lectura sobre la tabla `Productos_Lab` en el entorno `LabEnvironment-PowerApps` |
| Power Platform Admin Center | Acceso de lectura para visualizar roles de seguridad (proporcionado por el instructor) |

### Recursos Precreados por el Instructor

- Lista `Inventario_Lab` en SharePoint con columnas: ID (número), Nombre (texto), Categoria (texto), Cantidad (número), Precio (número) — mínimo 10 registros
- Tabla `Productos_Lab` en Dataverse con columnas: cr_nombre (texto), cr_categoria (texto), cr_stock (entero), cr_preciounitario (decimal), cr_activo (booleano) — mínimo 5 registros

## Entorno del Laboratorio

### Software Necesario

| Componente | Versión |
|------------|---------|
| Navegador web | Chrome 124.0.6367.82 o Edge 124.0.2478.51 |
| Power Apps Studio (web) | 3.24044.18+ |
| SharePoint Online | Servicio cloud abril 2024 |
| Microsoft Dataverse | 9.2.24044.00182 |

### Verificación Inicial del Entorno

Antes de comenzar, confirma que el entorno correcto está seleccionado:

1. Navega a `https://make.powerapps.com`
2. En la esquina superior derecha, verifica que el selector de entorno muestre **LabEnvironment-PowerApps**
3. Si no es correcto, haz clic en el selector y elige `LabEnvironment-PowerApps`

---

## Procedimiento Paso a Paso

### Paso 1: Abrir la aplicación existente y eliminar la conexión a Excel

**Objetivo:** Acceder a la aplicación del Lab 02 y remover la fuente de datos Excel para preparar la migración a SharePoint.

**Instrucciones:**

1. En `make.powerapps.com`, confirma que el entorno sea **LabEnvironment-PowerApps** (selector superior derecho).
2. En el panel izquierdo, haz clic en **Aplicaciones**.
3. Localiza `AppInventario_[InicialNombre]` en la lista y haz clic en los tres puntos (⋮) → **Editar**.
4. Espera a que se abra Power Apps Studio completamente.
5. En la barra lateral izquierda, haz clic en el ícono de **Datos** (cilindro de base de datos).
6. Localiza la conexión a Excel (aparecerá con el nombre de la tabla del archivo Excel).
7. Haz clic en los tres puntos (⋮) junto al nombre de la tabla Excel y selecciona **Quitar**.
8. Confirma la eliminación cuando el sistema lo solicite.

**Resultado Esperado:**

- El panel de Datos ya no muestra ninguna fuente de datos Excel.
- Es posible que la Gallery en `scr_Lista` (o `Pantalla_Lista`) muestre errores temporales (ícono de advertencia amarillo) — esto es esperado y se resolverá en el siguiente paso.

**Verificación:**

- En el panel de Datos, la sección de fuentes conectadas debe estar vacía o sin referencias a Excel.
- El ícono de advertencia (⚠️) aparece en la barra de fórmulas al seleccionar la Gallery, indicando que la fuente de datos no existe.

---

### Paso 2: Agregar el conector de SharePoint y conectar la lista `Inventario_Lab`

**Objetivo:** Establecer la conexión con SharePoint Online y vincular la lista `Inventario_Lab` como fuente de datos principal de la Gallery existente.

**Instrucciones:**

1. En el panel de **Datos** (barra lateral izquierda), haz clic en **+ Agregar datos**.
2. En el cuadro de búsqueda, escribe `SharePoint`.
3. Selecciona el conector **SharePoint** de la lista de resultados.
4. Si es la primera vez que usas este conector, selecciona la conexión existente o haz clic en **+ Agregar una conexión** y autentica con tus credenciales de Microsoft 365.
5. En el campo **Conectarse a un sitio de SharePoint**, ingresa la URL:
   ```
   https://[tenant].sharepoint.com/sites/LabSite-PowerApps
   ```
   > **Nota:** Reemplaza `[tenant]` con el nombre de tu tenant proporcionado por el instructor.
6. Haz clic en **Conectar**.
7. En la lista de listas disponibles, marca la casilla junto a **Inventario_Lab**.
8. Haz clic en **Conectar** (botón inferior).

**Resultado Esperado:**

- En el panel de Datos aparece `Inventario_Lab` como fuente de datos conectada.
- El ícono de SharePoint (verde) se muestra junto al nombre de la lista.

**Verificación:**

- Haz clic en los tres puntos (⋮) junto a `Inventario_Lab` → **Ver tabla**. Debes ver las columnas: ID, Nombre, Categoria, Cantidad, Precio y los registros de muestra.

---

### Paso 3: Actualizar la Gallery existente para usar la lista de SharePoint

**Objetivo:** Reconfigurar el control Gallery en la pantalla de lista para que muestre los datos de SharePoint en lugar de Excel.

**Instrucciones:**

1. En el panel izquierdo de **Vista de árbol**, navega a la pantalla `scr_Lista` (o el nombre que hayas asignado a la pantalla con la Gallery en el Lab 02).
2. Selecciona el control Gallery (por ejemplo, `gal_ListaProductos`).
3. En la barra de fórmulas superior, localiza la propiedad **Items**.
4. Reemplaza la fórmula actual (que mostraba error) por:
   ```powerapps
   Inventario_Lab
   ```
5. Presiona **Enter** para confirmar.
6. Si la Gallery no muestra los campos correctos automáticamente, ajusta los campos del layout:
   - Haz clic en **Editar campos** (lápiz) en el panel de propiedades de la Gallery.
   - Asigna:
     - **Title** (Título): `Nombre`
     - **Subtitle** (Subtítulo): `Categoria`
     - **Body** (Cuerpo): `Cantidad`
7. (Opcional) Para demostrar la función `Filter()`, modifica la propiedad **Items** a:
   ```powerapps
   Filter(Inventario_Lab, Cantidad > 0)
   ```

**Resultado Esperado:**

- La Gallery muestra los registros de la lista `Inventario_Lab` de SharePoint con nombre, categoría y cantidad visibles.
- Los errores previos desaparecen de la barra de fórmulas.

**Verificación:**

- Cuenta visualmente los registros en la Gallery: deben ser al menos 10 (o los que cumplan el filtro si aplicaste `Filter()`).
- Haz clic en **Vista previa** (▶️ en la esquina superior derecha) y confirma que los datos se cargan correctamente.

---

### Paso 4: Explorar la tabla `Productos_Lab` en Dataverse

**Objetivo:** Familiarizarse con la estructura de la tabla Dataverse desde el portal de Power Apps antes de conectarla a la aplicación.

**Instrucciones:**

1. Abre una nueva pestaña del navegador y navega a `https://make.powerapps.com`.
2. Confirma que el entorno sea **LabEnvironment-PowerApps**.
3. En el panel de navegación izquierdo, haz clic en **Tablas**.
4. En el buscador superior, escribe `Productos_Lab`.
5. Haz clic en la tabla **Productos_Lab** para abrirla.
6. Observa y documenta la estructura:

| Columna (Nombre de visualización) | Nombre lógico | Tipo de dato |
|-----------------------------------|---------------|--------------|
| Nombre | cr_nombre | Texto (una línea) |
| Categoria | cr_categoria | Texto (una línea) |
| Stock | cr_stock | Número entero |
| PrecioUnitario | cr_preciounitario | Número decimal (2 decimales) |
| Activo | cr_activo | Booleano (Sí/No) |

7. Haz clic en la pestaña **Datos** (o **Filas de datos**) para visualizar los registros existentes (mínimo 5).
8. Regresa a la pestaña del navegador donde tienes Power Apps Studio abierto.

**Resultado Esperado:**

- Puedes ver la estructura completa de la tabla con sus 5 columnas personalizadas más las columnas del sistema (Created On, Modified By, etc.).
- Los registros de muestra son visibles en la vista de datos.

**Verificación:**

- Confirma que la columna `cr_activo` tiene valor predeterminado `true` (Sí) en al menos un registro.
- Verifica que `cr_preciounitario` muestra valores con 2 decimales.

---

### Paso 5: Agregar el conector de Microsoft Dataverse a la aplicación

**Objetivo:** Configurar la segunda conexión de datos en la aplicación, apuntando a la tabla `Productos_Lab` de Dataverse.

**Instrucciones:**

1. En Power Apps Studio (pestaña de edición de tu app), ve al panel de **Datos**.
2. Haz clic en **+ Agregar datos**.
3. En el cuadro de búsqueda, escribe `Dataverse` (o busca directamente `Productos_Lab`).
4. Selecciona el conector **Microsoft Dataverse**.
5. En la lista de tablas disponibles, busca y selecciona **Productos_Lab**.
   > **Nota:** Si no aparece inmediatamente, usa el buscador dentro del panel de selección de tablas.
6. Haz clic en **Conectar**.

**Resultado Esperado:**

- En el panel de Datos ahora aparecen **dos** fuentes de datos:
  - `Inventario_Lab` (SharePoint — ícono verde)
  - `Productos_Lab` (Dataverse — ícono morado/azul)

**Verificación:**

- Ambas fuentes de datos son visibles simultáneamente en el panel de Datos sin errores.

---

### Paso 6: Crear la pantalla `scr_Dataverse` con Gallery conectada a Dataverse

**Objetivo:** Agregar una cuarta pantalla a la aplicación con una Gallery que muestre los registros de la tabla `Productos_Lab`.

**Instrucciones:**

1. En la barra superior de Power Apps Studio, haz clic en **+ Nueva pantalla** → **En blanco**.
2. En el panel de Vista de árbol (izquierda), haz doble clic en el nombre de la nueva pantalla y renómbrala a:
   ```
   scr_Dataverse
   ```
3. Agrega un control **Label** (Etiqueta) en la parte superior:
   - **Nombre del control:** `lbl_TituloDataverse`
   - **Propiedad Text:** `"Productos (Dataverse)"`
   - **Propiedad Size:** `24`
   - **Propiedad Align:** `Center`
   - Posición: X=0, Y=0, Width=Parent.Width, Height=80
4. Agrega un control **Gallery** (Galería vertical en blanco):
   - **Nombre del control:** `gal_ProductosDataverse`
   - Posición: X=0, Y=80, Width=Parent.Width, Height=Parent.Height - 160
5. Selecciona `gal_ProductosDataverse` y configura la propiedad **Items**:
   ```powerapps
   Filter(Productos_Lab, cr_activo = true)
   ```
6. Dentro de la Gallery, agrega los siguientes Labels:
   - **Label 1** — Propiedad Text: `ThisItem.cr_nombre` (Nombre del producto)
   - **Label 2** — Propiedad Text: `ThisItem.cr_categoria` (Categoría)
   - **Label 3** — Propiedad Text: `"Stock: " & Text(ThisItem.cr_stock)` (Stock disponible)
   - **Label 4** — Propiedad Text: `"$" & Text(ThisItem.cr_preciounitario, "0.00")` (Precio unitario)
7. Agrega un botón de navegación de regreso:
   - **Nombre del control:** `btn_VolverDesdeDataverse`
   - **Propiedad Text:** `"← Inicio"`
   - **Propiedad OnSelect:**
     ```powerapps
     Navigate(scr_Inicio, ScreenTransition.Fade)
     ```
   - Posición: parte inferior de la pantalla (Y = Parent.Height - 70)

**Resultado Esperado:**

- La pantalla `scr_Dataverse` muestra una Gallery con los productos activos de Dataverse, incluyendo nombre, categoría, stock y precio.
- El botón de regreso está visible en la parte inferior.

**Verificación:**

- Haz clic en **Vista previa** (▶️) y navega a `scr_Dataverse`. Los registros deben mostrarse con los datos correctos de la tabla Dataverse.
- Verifica que solo aparecen productos donde `cr_activo = true`.

---

### Paso 7: Actualizar la navegación desde la pantalla de inicio

**Objetivo:** Agregar un botón en la pantalla de inicio que permita navegar a la nueva pantalla de Dataverse, completando la navegación de 4 pantallas.

**Instrucciones:**

1. Navega a la pantalla `scr_Inicio` en la Vista de árbol.
2. Agrega un nuevo control **Button** (Botón):
   - **Nombre del control:** `btn_IrDataverse`
   - **Propiedad Text:** `"Ver Productos (Dataverse)"`
   - **Propiedad OnSelect:**
     ```powerapps
     Navigate(scr_Dataverse, ScreenTransition.Cover)
     ```
   - Posición: debajo de los botones existentes, con suficiente espacio visual.
3. (Opcional) Ajusta el color del botón para diferenciarlo:
   - **Propiedad Fill:** `ColorValue("#7B2D8B")` (morado, representando Dataverse)
   - **Propiedad Color:** `White`

**Resultado Esperado:**

- La pantalla de inicio ahora tiene botones de navegación a todas las pantallas: Lista (SharePoint), Detalle, y Productos (Dataverse).

**Verificación:**

- En modo Vista previa, haz clic en el nuevo botón y confirma que navega correctamente a `scr_Dataverse`.
- Desde `scr_Dataverse`, haz clic en "← Inicio" y confirma el regreso.

---

### Paso 8: Explorar roles de seguridad en el Centro de Administración

**Objetivo:** Visualizar los roles de seguridad de Dataverse y comprender la diferencia de permisos entre SharePoint y Dataverse.

**Instrucciones:**

1. Abre una nueva pestaña y navega a `https://admin.powerplatform.microsoft.com`.
2. En el panel izquierdo, haz clic en **Entornos**.
3. Selecciona el entorno **LabEnvironment-PowerApps**.
4. Haz clic en **Configuración** (barra superior) → **Usuarios + permisos** → **Roles de seguridad**.
5. Observa los roles disponibles. Identifica al menos estos dos:
   - **Propietario de entorno (Environment Maker):** puede crear recursos pero no administrar seguridad.
   - **Usuario básico (Basic User):** puede ejecutar aplicaciones y acceder a datos según los permisos de la tabla.
6. (Solo observación) Haz clic en un rol para ver los permisos por tabla — nota cómo se asignan permisos de Crear, Leer, Escribir, Eliminar por entidad.

> **Documentación de diferencias — SharePoint vs Dataverse:**
>
> | Aspecto | SharePoint | Dataverse |
> |---------|-----------|-----------|
> | **Modelo de permisos** | Basado en sitio/lista (Lectura, Edición, Control total) | Basado en roles de seguridad por tabla y operación CRUD |
> | **Granularidad** | Nivel de lista o elemento | Nivel de tabla, fila (registro) y columna |
> | **Administración** | Administrador del sitio SharePoint | Administrador del entorno Power Platform |
> | **Herencia** | Permisos heredados del sitio padre | Roles asignados a equipos o usuarios individuales |
> | **Caso de uso ideal** | Datos colaborativos, documentos, listas simples | Datos empresariales con lógica de negocio y relaciones complejas |

**Resultado Esperado:**

- Puedes visualizar la lista de roles de seguridad del entorno.
- Comprendes que Dataverse ofrece control granular por tabla/operación, mientras SharePoint opera a nivel de sitio/lista.

**Verificación:**

- Confirma visualmente que el rol "Basic User" existe en la lista de roles del entorno `LabEnvironment-PowerApps`.

---

### Paso 9: Guardar y publicar la aplicación

**Objetivo:** Persistir todos los cambios realizados y publicar la versión actualizada.

**Instrucciones:**

1. Regresa a la pestaña de Power Apps Studio.
2. Presiona **Ctrl + S** (o haz clic en el ícono de guardar en la barra superior).
3. Si se solicita un comentario de versión, escribe: `Lab 03: Conexión SharePoint + Dataverse, 4 pantallas`.
4. Haz clic en **Publicar** (botón en la barra superior).
5. Confirma la publicación seleccionando **Publicar esta versión**.

**Resultado Esperado:**

- La aplicación se guarda exitosamente sin errores.
- La versión publicada está disponible para ejecución.

**Verificación:**

- El mensaje "La aplicación se publicó correctamente" aparece en pantalla.
- En `make.powerapps.com` → **Aplicaciones**, la fecha de modificación de `AppInventario_[InicialNombre]` refleja la hora actual.

---

## Validación y Pruebas

Ejecuta las siguientes comprobaciones para confirmar que el laboratorio se completó correctamente:

| # | Prueba | Resultado Esperado | ✓/✗ |
|---|--------|-------------------|------|
| 1 | Abrir panel de Datos en la app | Solo aparecen `Inventario_Lab` (SharePoint) y `Productos_Lab` (Dataverse). NO hay conexión Excel. | |
| 2 | Vista previa → `scr_Lista` | Gallery muestra ≥10 registros de SharePoint con Nombre, Categoria y Cantidad | |
| 3 | Vista previa → `scr_Dataverse` | Gallery muestra registros activos de Dataverse con nombre, categoría, stock y precio | |
| 4 | Navegación completa | Desde `scr_Inicio` se puede navegar a todas las pantallas y regresar sin errores | |
| 5 | Conteo de pantallas | La aplicación tiene exactamente 4 pantallas en la Vista de árbol | |

---

## Solución de Problemas

### Problema 1: La lista `Inventario_Lab` no aparece al conectar SharePoint

**Síntomas:** Después de ingresar la URL del sitio SharePoint y hacer clic en Conectar, la lista `Inventario_Lab` no aparece en el panel de selección de listas.

**Causa:** La URL del sitio es incorrecta (error tipográfico, falta `/sites/` en la ruta) o el usuario no tiene permisos de lectura en el sitio de SharePoint.

**Solución:**
1. Verifica la URL exacta con el instructor: `https://[tenant].sharepoint.com/sites/LabSite-PowerApps` (sin barra final).
2. Abre la URL directamente en el navegador para confirmar que puedes acceder al sitio.
3. Si el sitio carga pero la lista no aparece, solicita al instructor que verifique los permisos de "Miembro" o "Visitante" del sitio para tu cuenta.
4. Intenta desconectar y reconectar: en el panel de Datos → tres puntos junto a SharePoint → **Quitar** → volver a agregar.

---

### Problema 2: La tabla `Productos_Lab` no muestra datos en la Gallery de Dataverse

**Síntomas:** La Gallery `gal_ProductosDataverse` aparece vacía aunque la tabla tiene registros visibles en el portal de tablas de Power Apps.

**Causa:** El filtro `cr_activo = true` excluye todos los registros (si fueron creados con valor `false`), o el rol de seguridad del usuario no tiene permiso de lectura sobre la tabla `Productos_Lab`.

**Solución:**
1. Temporalmente, cambia la propiedad Items de la Gallery a solo `Productos_Lab` (sin filtro) para descartar el problema del filtro.
2. Si sigue vacía, el problema es de permisos. Solicita al instructor que:
   - Vaya a admin.powerplatform.microsoft.com → Entornos → LabEnvironment-PowerApps → Configuración → Roles de seguridad.
   - Verifique que tu usuario tenga asignado un rol con permiso de **Lectura** en la tabla `Productos_Lab` (nivel Organización o Unidad de negocio).
3. Si el paso 1 muestra datos, revisa los valores de la columna `cr_activo` en la tabla — al menos un registro debe tener valor `true` (Sí).

---

## Limpieza

No se requiere limpieza destructiva para este laboratorio, ya que la aplicación actualizada se utilizará en laboratorios posteriores.

**Acciones opcionales de orden:**
- Cierra las pestañas adicionales del navegador (portal de tablas, admin center).
- Verifica que la aplicación quedó publicada (no solo guardada).
- Cierra Power Apps Studio haciendo clic en la flecha de regreso (←) en la esquina superior izquierda.

> ⚠️ **Importante:** NO elimines las conexiones a SharePoint ni a Dataverse. Serán necesarias en los laboratorios siguientes.

---

## Resumen

En este laboratorio aplicaste los conceptos de conectores vistos en la Lección 3.1 de forma práctica:

- **Migraste** la fuente de datos de Excel a SharePoint Online, demostrando la flexibilidad de cambiar conectores sin reconstruir la aplicación.
- **Exploraste** la estructura de una tabla Dataverse, identificando tipos de datos y la diferencia con listas de SharePoint.
- **Configuraste** dos conectores simultáneos (SharePoint estándar + Dataverse premium) en una misma aplicación Canvas.
- **Implementaste** la función `Filter()` de Power Fx para mostrar solo registros activos desde Dataverse.
- **Identificaste** las diferencias en modelos de seguridad: permisos de sitio/lista en SharePoint vs. roles de seguridad granulares en Dataverse.

### Estado Final de la Aplicación

| Pantalla | Fuente de datos | Propósito |
|----------|----------------|-----------|
| `scr_Inicio` | — | Navegación principal |
| `scr_Lista` | SharePoint (`Inventario_Lab`) | Lista de inventario |
| `scr_Detalle` | — | Detalle de producto seleccionado |
| `scr_Dataverse` | Dataverse (`Productos_Lab`) | Productos desde Dataverse |

### Recursos Adicionales

- [Conector de SharePoint para Power Apps — Microsoft Learn](https://learn.microsoft.com/es-es/connectors/sharepointonline/)
- [Conector de Microsoft Dataverse — Microsoft Learn](https://learn.microsoft.com/es-es/connectors/commondataserviceforapps/)
- [Roles de seguridad en Dataverse — Microsoft Learn](https://learn.microsoft.com/es-es/power-platform/admin/security-roles-privileges)
- [Función Filter en Power Fx — Microsoft Learn](https://learn.microsoft.com/es-es/power-platform/power-fx/reference/function-filter-lookup)
