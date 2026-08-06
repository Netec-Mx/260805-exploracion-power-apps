# Demostración Aplicada — Compartición, Monitoreo y Gobernanza de una Canvas App

## Metadatos

| Campo | Valor |
|-------|-------|
| **Duración** | 10 minutos |
| **Complejidad** | Media |
| **Nivel Bloom** | Aplicar |

## Descripción General

Este laboratorio final integra los conocimientos adquiridos durante todo el curso "Exploración Power Apps". Ejecutarás el ciclo completo de puesta en producción de una Canvas App: compartirla con permisos diferenciados, validar su funcionamiento con herramientas de monitoreo, consultar métricas de uso y documentar lineamientos de gobernanza para usuarios finales. Trabajarás con la aplicación `AppInventario_[InicialNombre]` construida en laboratorios anteriores.

## Objetivos de Aprendizaje

- [ ] Compartir una Canvas App con al menos dos usuarios asignando permisos diferenciados (Can use vs. Can edit)
- [ ] Revisar el historial de versiones e identificar una versión anterior desde el panel de versiones
- [ ] Ejecutar la herramienta Monitor de Power Apps para identificar errores o advertencias en tiempo real
- [ ] Consultar métricas básicas de uso desde Power Platform Admin Center
- [ ] Documentar al menos tres lineamientos de gobernanza aplicables a usuarios finales

## Prerrequisitos

### Conocimientos Previos

| Requisito | Descripción |
|-----------|-------------|
| Power Apps Studio | Navegación por pantallas, panel de propiedades, barra de fórmulas |
| Canvas App funcional | Haber completado los laboratorios 01 al 05 del curso |
| Conceptos de compartición | Comprensión de roles Usuario y Copropietario (Lección 6.1) |
| Microsoft 365 | Familiaridad básica con el tenant y cuentas de usuario |

### Acceso Requerido

| Recurso | Detalle |
|---------|---------|
| Cuenta Microsoft 365 | Con licencia Power Apps habilitada (E3, E5 o Power Apps per-user plan) |
| Canvas App publicada | `AppInventario_[InicialNombre]` en el entorno `LabEnvironment-PowerApps` |
| Cuenta adicional | Al menos una cuenta de compañero de curso o cuenta de prueba en el mismo tenant |
| Power Platform Admin Center | Acceso con rol de administrador de entorno o superior |
| Fuente de datos | Lista SharePoint `Inventario_Lab` en `https://[tenant].sharepoint.com/sites/LabSite-PowerApps` |

## Entorno del Laboratorio

### Software Necesario

| Herramienta | Versión | URL |
|-------------|---------|-----|
| Power Apps Studio (Web) | 3.24053.18 | https://make.powerapps.com |
| Power Platform Admin Center | 2.0.106 | https://admin.powerplatform.microsoft.com |
| Microsoft Edge / Chrome | 124.x | Navegador local |

### Configuración Inicial

1. Abrir el navegador y navegar a `https://make.powerapps.com`
2. Verificar en la esquina superior derecha que el entorno seleccionado sea **LabEnvironment-PowerApps**
3. Confirmar que la aplicación `AppInventario_[InicialNombre]` aparece en la sección **Aplicaciones** con estado "Publicada"

> ⚠️ **Importante:** Si el entorno no es `LabEnvironment-PowerApps`, haz clic en el selector de entorno y cámbialo antes de continuar.

---

## Procedimiento Paso a Paso

### Paso 1: Compartir la Aplicación con Permisos Diferenciados

**Objetivo:** Otorgar acceso a la aplicación a dos usuarios con roles distintos (Can use y Can edit) y revisar el historial de versiones.

**Instrucciones:**

1. En `make.powerapps.com`, selecciona **Aplicaciones** en el panel de navegación izquierdo.

2. Localiza tu aplicación `AppInventario_[InicialNombre]` en la lista.

3. Haz clic en los tres puntos (**···**) a la derecha del nombre de la aplicación.

4. Selecciona **Compartir** en el menú contextual.

5. En el cuadro de búsqueda del panel de compartición, escribe el correo electrónico del **primer usuario** (compañero de curso o cuenta de prueba).

6. Asegúrate de que el permiso asignado sea **Can use** (Usuario). Este es el valor predeterminado.
   - Verifica que la casilla "Send an email invitation" esté marcada.

7. Haz clic en **Compartir** para confirmar.

8. Repite el proceso: haz clic nuevamente en **···** → **Compartir**.

9. Escribe el correo electrónico del **segundo usuario**.

10. Antes de confirmar, haz clic en el icono de permiso junto al nombre del usuario y cámbialo a **Co-owner** (Copropietario / Can edit).

11. Haz clic en **Compartir** para confirmar.

12. Ahora, revisa el historial de versiones: haz clic en **···** junto a tu aplicación y selecciona **Detalles**.

13. En la página de detalles, selecciona la pestaña **Versiones**.

14. Observa la lista de versiones disponibles: identifica la versión actualmente publicada (marcada con "Activa"), la fecha de última modificación y el autor.

15. Haz clic en una versión anterior (no la activa) y observa las opciones disponibles: **Restaurar** y **Detalles**.

> ⚠️ **No restaures** la versión anterior en este momento; solo confirma que la opción está disponible.

**Resultado Esperado:**

- El primer usuario recibe un correo de notificación indicando que puede **usar** la aplicación.
- El segundo usuario recibe un correo indicando que puede **editar** la aplicación.
- En la pestaña de versiones se visualizan al menos 2 versiones con fechas y autores.

**Verificación:**

| Criterio | Cumple (Sí/No) |
|----------|-----------------|
| El panel de compartición muestra al primer usuario con rol "Can use" | |
| El panel de compartición muestra al segundo usuario con rol "Co-owner" | |
| La pestaña Versiones muestra el historial con al menos una versión publicada | |
| La opción "Restaurar" está disponible para versiones anteriores | |

---

### Paso 2: Ejecutar Pruebas con Monitor y App Checker

**Objetivo:** Utilizar las herramientas de depuración integradas en Power Apps Studio para identificar errores, advertencias o problemas de rendimiento.

**Instrucciones:**

1. Desde la lista de aplicaciones en `make.powerapps.com`, haz clic en tu aplicación `AppInventario_[InicialNombre]` para abrirla en **Power Apps Studio** (clic en el nombre o selecciona **Editar**).

2. Espera a que el diseñador cargue completamente la aplicación.

3. En la barra de herramientas superior, localiza y haz clic en **App checker** (icono de escudo con marca de verificación o triángulo de advertencia).

4. Revisa el panel que se despliega:
   - **Errores (Errors):** fórmulas rotas o referencias inválidas.
   - **Advertencias (Warnings):** problemas de accesibilidad, rendimiento o buenas prácticas.
   - **Sugerencias (Tips):** recomendaciones opcionales.

5. Anota el número total de errores y advertencias. Si hay errores, haz clic en cada uno para ver la pantalla y el control afectado.

6. Cierra el panel de App Checker.

7. Ahora activa el **Monitor**: en la barra superior, selecciona **Herramientas avanzadas** (Advanced tools) → **Monitor** (o usa el menú **Configuración** → **Próximamente** según la versión).
   - Alternativa: desde la lista de aplicaciones en make.powerapps.com, haz clic en **···** → **Monitor**.

8. Se abrirá una nueva pestaña del navegador con la interfaz del Monitor y la aplicación se ejecutará en una sesión vinculada.

9. En la sesión de la aplicación (que se abre automáticamente), **navega por las pantallas principales**:
   - Pantalla de inicio (`scr_Inicio`)
   - Galería de productos (`gal_ListaProductos`)
   - Formulario de edición (si existe)

10. Regresa a la pestaña del Monitor y observa los eventos registrados:
    - **Network:** llamadas a conectores (SharePoint, Dataverse).
    - **Property changes:** cambios de propiedades en controles.
    - **Errors:** cualquier error en tiempo de ejecución.

11. Filtra los eventos por **Severity = Error** o **Severity = Warning** usando la barra de filtros superior.

12. Si hay eventos de error, haz clic en uno para expandir el detalle: verás la fórmula involucrada, la duración de la llamada y el código de respuesta.

13. Cierra la pestaña del Monitor y regresa a Power Apps Studio.

**Resultado Esperado:**

- El App Checker muestra un resumen de estado de la aplicación (idealmente 0 errores si los labs previos se completaron correctamente).
- El Monitor registra eventos de red correspondientes a las llamadas a SharePoint/Dataverse al navegar por la galería.
- No se presentan errores críticos de ejecución (status codes 4xx o 5xx repetidos).

**Verificación:**

| Criterio | Cumple (Sí/No) |
|----------|-----------------|
| App Checker se ejecutó y mostró resultados (errores/advertencias/tips) | |
| Monitor se activó correctamente y registró eventos al navegar la app | |
| Se identificaron las llamadas a conectores (Network) en el Monitor | |
| No hay errores críticos bloqueantes en la ejecución | |

---

### Paso 3: Consultar Métricas de Uso en Power Platform Admin Center

**Objetivo:** Acceder a las analíticas de uso de la aplicación para identificar sesiones, usuarios activos y posibles errores reportados.

**Instrucciones:**

1. Abre una nueva pestaña del navegador y navega a `https://admin.powerplatform.microsoft.com`.

2. Inicia sesión con tu cuenta de administrador del entorno (la misma cuenta Microsoft 365 del curso).

3. En el panel de navegación izquierdo, selecciona **Análisis** (Analytics) → **Power Apps**.

4. En la parte superior, asegúrate de que el filtro de entorno muestre **LabEnvironment-PowerApps** (cámbialo si es necesario).

5. Selecciona el período de tiempo: **Últimos 30 días**.

6. Observa el panel principal con las siguientes métricas:
   - **Número de sesiones** (App launches): cuántas veces se ha abierto la aplicación.
   - **Usuarios únicos** (Unique users): cuántos usuarios distintos la han ejecutado.
   - **Errores de servicio** (Service errors): fallos registrados del lado del servidor.

7. Si tu aplicación aparece en la lista de apps con actividad, haz clic en su nombre para ver el detalle.

8. Anota las métricas principales:

   | Métrica | Valor observado |
   |---------|-----------------|
   | Sesiones (últimos 30 días) | ___________ |
   | Usuarios únicos | ___________ |
   | Errores de servicio | ___________ |

> 📝 **Nota:** Si la aplicación fue publicada recientemente, es posible que las métricas muestren valores bajos o nulos. Las analíticas pueden tardar hasta 24-48 horas en reflejarse completamente.

9. Regresa a la vista general de Analytics.

**Resultado Esperado:**

- Se accede correctamente al panel de Analytics de Power Apps en el Admin Center.
- Se visualizan (o se confirma la ausencia temporal de) métricas de uso de la aplicación.
- Se comprende la ubicación y estructura de los reportes de uso.

**Verificación:**

| Criterio | Cumple (Sí/No) |
|----------|-----------------|
| Se accedió correctamente a admin.powerplatform.microsoft.com | |
| Se seleccionó el entorno LabEnvironment-PowerApps en el filtro | |
| Se visualizó la sección Analytics > Power Apps | |
| Se identificaron las métricas de sesiones y usuarios (aunque sean 0) | |

---

### Paso 4: Documentar Lineamientos de Gobernanza

**Objetivo:** Completar una plantilla de gobernanza con al menos tres lineamientos aplicables a la administración de la aplicación por usuarios finales.

**Instrucciones:**

1. Abre un documento nuevo (Word, OneNote o un archivo de texto) para documentar los lineamientos.

2. Crea el siguiente encabezado:

```
LINEAMIENTOS DE GOBERNANZA - AppInventario_[InicialNombre]
Fecha: [fecha actual]
Autor: [tu nombre completo]
Entorno: LabEnvironment-PowerApps
```

3. Documenta los siguientes **cinco lineamientos** (mínimo tres obligatorios):

**Lineamiento 1 — Convención de Nomenclatura:**

```
REGLA: Todas las aplicaciones deben seguir el formato:
  [Área]-[Función]-[Versión]

EJEMPLOS:
  - Inventario-Registro-v1
  - RRHH-Vacaciones-v2
  - Ventas-Cotizaciones-v1

APLICACIÓN: La app actual se identifica como:
  AppInventario_[InicialNombre] (convención del curso)
  En producción se renombraría a: Almacen-Inventario-v1
```

**Lineamiento 2 — Política de Compartición:**

```
REGLA: Solo el creador original o un administrador designado puede 
otorgar permisos de tipo "Co-owner" (Can edit).

RESTRICCIONES:
  - Los usuarios finales NUNCA deben recibir permisos Can edit
    a menos que sea aprobado por el administrador del entorno.
  - La compartición con "Toda la organización" requiere aprobación 
    del área de TI.
  - Se prefiere el uso de grupos de seguridad de Entra ID sobre 
    usuarios individuales.

RESPONSABLE: [Nombre del creador/administrador]
```

**Lineamiento 3 — Proceso de Solicitud de Cambios:**

```
REGLA: Los usuarios finales NO deben modificar la aplicación directamente.

PROCESO:
  1. El usuario identifica una mejora o error.
  2. Envía solicitud al creador vía:
     - Formulario de Teams (canal #soporte-apps)
     - Correo electrónico a [correo del administrador]
  3. El creador evalúa, implementa y prueba el cambio.
  4. Se publica una nueva versión con descripción del cambio.
  5. Se notifica al solicitante.

TIEMPO DE RESPUESTA ESPERADO: 3-5 días hábiles.
```

**Lineamiento 4 — Política de Retención de Versiones:**

```
REGLA: Mantener documentadas las últimas 10 versiones publicadas.

ACCIONES:
  - Cada versión publicada debe incluir una descripción clara 
    del cambio realizado (campo "Descripción" al publicar).
  - Formato de descripción: "v[X] - [Resumen del cambio] - [Fecha]"
  - Ejemplo: "v3 - Agregado filtro por categoría en galería - 2024-04-15"
  - Versiones anteriores a la #10 pueden eliminarse previa revisión.
```

**Lineamiento 5 — Revisión Periódica de Accesos:**

```
REGLA: El creador debe revisar la lista de usuarios con acceso 
cada 90 días.

ACCIONES:
  - Verificar que no existan usuarios inactivos o que hayan 
    cambiado de área/rol.
  - Remover accesos de personal que ya no requiere la aplicación.
  - Documentar la fecha de última revisión.

PRÓXIMA REVISIÓN: [Fecha actual + 90 días]
RESPONSABLE: [Nombre del creador]
```

4. Guarda el documento con el nombre: `Gobernanza_AppInventario_[InicialNombre].docx`

5. Verifica que el documento contenga al menos los tres primeros lineamientos completos.

**Resultado Esperado:**

- Un documento de gobernanza completo con mínimo 3 lineamientos (idealmente 5) claramente definidos.
- Cada lineamiento incluye: regla, acciones/restricciones y responsable.

**Verificación:**

| Criterio | Cumple (Sí/No) |
|----------|-----------------|
| El documento tiene encabezado con nombre de app, fecha y autor | |
| Lineamiento de nomenclatura documentado con formato y ejemplos | |
| Política de compartición documentada con restricciones claras | |
| Proceso de solicitud de cambios documentado con pasos secuenciales | |
| Al menos 3 de los 5 lineamientos están completos | |

---

### Paso 5: Cierre y Verificación Final

**Objetivo:** Confirmar que todos los elementos del laboratorio se completaron correctamente y reflexionar sobre el ciclo completo de desarrollo.

**Instrucciones:**

1. Regresa a `make.powerapps.com` → **Aplicaciones**.

2. Haz clic en **···** junto a `AppInventario_[InicialNombre]` → **Compartir**.

3. Confirma que en el panel de compartición aparecen los dos usuarios agregados en el Paso 1 con sus roles correctos.

4. Verifica que la aplicación tiene estado **Publicada** (ícono verde o indicador "Live").

5. Confirma que tu documento de gobernanza está guardado y accesible.

6. Completa la siguiente **lista de verificación final**:

| Elemento | Estado |
|----------|--------|
| App compartida con usuario "Can use" | ☐ Completado |
| App compartida con usuario "Can edit" (Co-owner) | ☐ Completado |
| Historial de versiones revisado | ☐ Completado |
| App Checker ejecutado | ☐ Completado |
| Monitor ejecutado y eventos revisados | ☐ Completado |
| Métricas consultadas en Admin Center | ☐ Completado |
| Documento de gobernanza con ≥3 lineamientos | ☐ Completado |

**Resultado Esperado:**

Todos los elementos de la lista de verificación marcados como completados, confirmando la ejecución exitosa del ciclo completo: desarrollo → publicación → compartición → monitoreo → gobernanza.

---

## Validación y Pruebas

Para confirmar que el laboratorio se completó exitosamente, verifica los siguientes criterios:

| # | Criterio de Validación | Método de Verificación |
|---|------------------------|------------------------|
| 1 | La app está compartida con 2 usuarios con roles distintos | Panel de compartición en make.powerapps.com |
| 2 | El historial muestra múltiples versiones | Pestaña Versiones en Detalles de la app |
| 3 | El Monitor no reporta errores críticos | Sesión de Monitor sin eventos de severidad "Error" bloqueante |
| 4 | Las métricas de Admin Center son accesibles | Navegación exitosa a Analytics > Power Apps |
| 5 | El documento de gobernanza tiene ≥3 lineamientos completos | Revisión del archivo guardado |

**Prueba de aceptación rápida:** Pide al primer usuario (Can use) que abra la aplicación desde su cuenta. Debe poder ejecutarla pero **no** ver la opción "Editar" en el menú de la app. El segundo usuario (Co-owner) sí debe ver la opción "Editar".

---

## Solución de Problemas

### Problema 1: El Monitor no registra eventos al navegar la aplicación

**Síntomas:** Se abre la ventana del Monitor pero la tabla de eventos permanece vacía aunque se navega por las pantallas de la app.

**Causa:** La sesión de la aplicación no está vinculada correctamente al Monitor. Esto ocurre cuando se abre la app en una pestaña diferente a la que el Monitor generó automáticamente, o cuando el navegador bloquea ventanas emergentes.

**Solución:**
1. Cierra la pestaña del Monitor y la sesión de la app.
2. Verifica que el navegador no esté bloqueando ventanas emergentes (revisa el ícono de bloqueo en la barra de direcciones).
3. Regresa a `make.powerapps.com` → **Aplicaciones** → **···** junto a tu app → **Monitor**.
4. Permite que se abra la nueva pestaña automáticamente.
5. Usa el botón **"Play published app"** o **"Play authored version"** dentro de la interfaz del Monitor para iniciar la sesión vinculada.
6. Navega por la app y confirma que los eventos aparecen en la tabla del Monitor.

---

### Problema 2: No se visualizan métricas en Power Platform Admin Center

**Síntomas:** Al navegar a Analytics > Power Apps en el Admin Center, la sección muestra "No hay datos disponibles" o gráficos vacíos para la aplicación.

**Causa:** Las analíticas de Power Platform tienen un retraso de procesamiento de 24 a 48 horas. Si la aplicación fue publicada o utilizada por primera vez el mismo día del laboratorio, los datos aún no están disponibles. También puede ocurrir si el filtro de entorno no coincide.

**Solución:**
1. Verifica que el filtro de entorno en la parte superior del panel esté configurado en **LabEnvironment-PowerApps** (no en "Default" u otro entorno).
2. Cambia el rango de fechas a **Últimos 30 días** (no "Últimos 7 días").
3. Si los datos siguen vacíos, confirma que la aplicación fue ejecutada al menos una vez por un usuario (no solo editada en Studio).
4. Documenta en tu verificación que las métricas no están disponibles aún por el retraso de procesamiento — esto es un comportamiento esperado para apps recién publicadas.
5. Como alternativa, el instructor puede mostrar un ejemplo con una app que tenga más de 48 horas de actividad registrada.

---

## Limpieza

> 📝 **Nota:** Al ser este el laboratorio final del curso, la limpieza es opcional y depende de las instrucciones del instructor.

| Acción | Comando/Procedimiento | ¿Obligatorio? |
|--------|----------------------|---------------|
| Mantener la app compartida | No eliminar los permisos otorgados (sirven como evidencia) | Recomendado |
| Guardar documento de gobernanza | Almacenar en OneDrive o entregar al instructor | Sí |
| Cerrar sesión del Monitor | Cerrar la pestaña del Monitor en el navegador | Sí |
| Cerrar Power Apps Studio | Guardar cambios pendientes y cerrar | Sí |

Si el instructor solicita limpiar el entorno:

1. En `make.powerapps.com` → **Aplicaciones** → **···** → **Compartir** → Eliminar los usuarios agregados.
2. Eliminar el documento de gobernanza local si ya fue entregado.

---

## Resumen

En este laboratorio final aplicaste de forma integrada las competencias del curso completo:

| Fase | Competencia Demostrada |
|------|----------------------|
| Compartición y permisos | Gestión de accesos diferenciados (Can use / Can edit) mediante el panel de compartición |
| Control de versiones | Navegación del historial de versiones e identificación de opciones de restauración |
| Monitoreo y pruebas | Uso de App Checker y Monitor para validar la calidad de la aplicación |
| Analíticas | Consulta de métricas de uso en Power Platform Admin Center |
| Gobernanza | Documentación de políticas de nomenclatura, compartición, cambios y revisión de accesos |

### Conceptos Clave Reforzados

- **Guardar ≠ Publicar:** Las versiones guardadas son invisibles para los usuarios finales hasta que se publican.
- **Compartir app ≠ Compartir datos:** Los permisos en la fuente de datos (SharePoint, Dataverse) deben gestionarse por separado.
- **Gobernanza proactiva:** Documentar reglas antes de escalar una app previene problemas de seguridad y mantenimiento.
- **Monitoreo continuo:** Las herramientas de Monitor y Analytics permiten detectar problemas antes de que los usuarios los reporten.

### Recursos Adicionales

- [Compartir una aplicación de lienzo](https://learn.microsoft.com/es-es/power-apps/maker/canvas-apps/share-app)
- [Usar Monitor para solucionar problemas](https://learn.microsoft.com/es-es/power-apps/maker/monitor-overview)
- [Analíticas de Power Apps en Admin Center](https://learn.microsoft.com/es-es/power-platform/admin/analytics-powerapps)
- [Restaurar una versión anterior](https://learn.microsoft.com/es-es/power-apps/maker/canvas-apps/restore-an-app)
- [Guía de gobernanza de Power Platform](https://learn.microsoft.com/es-es/power-platform/guidance/adoption/admin-best-practices)

---
