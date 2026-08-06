# Demostración / Caso Aplicado — Exploración del Ecosistema Power Platform

## Metadatos del Laboratorio

| Campo | Valor |
|-------|-------|
| **Duración** | 6 minutos |
| **Complejidad** | Fácil |
| **Nivel Bloom** | Aplicar |
| **Laboratorios previos requeridos** | Ninguno (laboratorio inicial de la serie) |

## Descripción General

En este laboratorio inicial accederás por primera vez al portal de Microsoft Power Platform, verificarás tu entorno de trabajo y explorarás las secciones principales del ecosistema. A través del análisis de un caso de negocio ficticio (Distribuidora Norte S.A.), identificarás qué componente de Power Platform resuelve cada necesidad empresarial y documentarás tus hallazgos en un archivo que servirá como referencia para los laboratorios posteriores.

## Objetivos de Aprendizaje

Al completar este laboratorio serás capaz de:

- [ ] Identificar los componentes principales de Microsoft Power Platform (Power Apps, Power Automate, Power BI, Power Pages, Copilot Studio) y describir su función dentro del ecosistema
- [ ] Distinguir el rol específico de Power Apps frente a Power Automate y Power BI mediante el análisis de un caso de negocio real
- [ ] Navegar por la interfaz principal de Power Platform (make.powerapps.com) identificando el entorno de trabajo, el selector de entornos y las secciones principales
- [ ] Explorar las capacidades de Copilot en Power Platform describiendo al menos dos escenarios donde Copilot puede acelerar el desarrollo

## Prerrequisitos

### Conocimientos Previos

| Conocimiento | Nivel |
|---|---|
| Navegación web básica (pestañas, URLs, formularios) | Requerido |
| Uso básico de Microsoft 365 (OneDrive, inicio de sesión) | Requerido |
| Concepto general de aplicaciones empresariales | Recomendado |

### Acceso y Credenciales

| Recurso | Detalle |
|---|---|
| Cuenta Microsoft 365 | Con licencia Power Apps habilitada (proporcionada por el instructor) |
| Entorno Power Platform | `LabEnvironment-PowerApps` (preconfigurado por el administrador) |
| Navegador web | Google Chrome 124+ o Microsoft Edge 124+ |
| Conexión a internet | Mínimo 10 Mbps de descarga |

## Entorno del Laboratorio

### Hardware Mínimo

| Componente | Especificación |
|---|---|
| Procesador | Intel Core i5 8.ª gen. o AMD Ryzen 5 3000+ (64 bits) |
| RAM | 8 GB mínimo |
| Resolución de pantalla | 1366×768 mínimo (1920×1080 recomendado) |
| Almacenamiento libre | 2 GB para caché del navegador |

### Software Requerido

| Software | Versión |
|---|---|
| Google Chrome o Microsoft Edge | 124.x o superior |
| Microsoft 365 (servicio en la nube) | Abril 2024 |
| Power Apps Studio (web) | 3.24044.18+ |

### Verificación Inicial del Entorno

Antes de comenzar, confirma que puedes acceder a los siguientes servicios desde tu navegador:

1. Abre una pestaña y navega a `https://make.powerapps.com` — debe cargar sin errores de autenticación.
2. Abre otra pestaña y navega a `https://www.office.com` — debes ver tu panel de Microsoft 365.

---

## Instrucciones Paso a Paso

### Paso 1: Iniciar Sesión y Seleccionar el Entorno de Trabajo

**Objetivo:** Autenticarse en el portal de Power Platform y confirmar que el entorno `LabEnvironment-PowerApps` está disponible y activo.

**Instrucciones:**

1. Abre tu navegador (Google Chrome o Microsoft Edge).
2. Navega a la URL:
   ```
   https://make.powerapps.com
   ```
3. Ingresa las credenciales de Microsoft 365 proporcionadas por tu instructor:
   - **Usuario:** `[tu_usuario]@[tenant].onmicrosoft.com`
   - **Contraseña:** la asignada por el administrador
4. Una vez autenticado, observa la esquina **superior derecha** de la página. Localiza el **selector de entornos** (aparece como un menú desplegable con el nombre del entorno actual).
5. Haz clic en el selector de entornos.
6. En la lista desplegable, selecciona **`LabEnvironment-PowerApps`**.
7. Espera a que la página se recargue mostrando el entorno seleccionado.

**Resultado Esperado:**

El portal muestra la página de inicio de Power Apps con el entorno `LabEnvironment-PowerApps` visible en la esquina superior derecha. La barra de navegación izquierda muestra las secciones: Inicio, Crear, Aprender, Aplicaciones, Tablas, Flujos, entre otras.

**Verificación:**

- ✅ El nombre `LabEnvironment-PowerApps` aparece claramente en el selector de entornos.
- ✅ No se muestran mensajes de error de licencia ni de permisos.
- ✅ La sección "Aplicaciones" del menú lateral izquierdo es accesible (haz clic para confirmar).

---

### Paso 2: Explorar las Secciones Principales del Portal

**Objetivo:** Familiarizarse con la estructura de navegación del portal make.powerapps.com identificando las áreas correspondientes a cada componente de Power Platform.

**Instrucciones:**

1. En el menú de navegación lateral izquierdo, haz clic en **"Inicio"** (ícono de casa). Observa:
   - La sección de creación rápida con opciones como "Empezar con datos", "Empezar con un diseño de página" y "Empezar con Copilot".
   - Las aplicaciones recientes (estará vacía si es tu primer acceso).

2. Haz clic en **"Aplicaciones"** en el menú lateral. Observa:
   - Esta sección lista todas las aplicaciones Canvas y Model-driven del entorno.
   - Nota los filtros disponibles: "Todas", "Soy propietario", "Compartidas conmigo".

3. Haz clic en **"Flujos"** en el menú lateral (o navega a `https://make.powerautomate.com`). Observa:
   - Esta es la sección de Power Automate integrada.
   - Verifica que el entorno sigue siendo `LabEnvironment-PowerApps` en la esquina superior derecha.

4. Haz clic en **"Tablas"** en el menú lateral. Observa:
   - Esta sección muestra las tablas de Dataverse disponibles en el entorno.
   - Nota la diferencia entre tablas "Estándar", "Personalizadas" y "Todas".

5. Abre una nueva pestaña del navegador y navega al **Centro de Administración de Power Platform**:
   ```
   https://admin.powerplatform.microsoft.com
   ```
6. En el menú lateral del Centro de Administración, haz clic en **"Entornos"**. Localiza `LabEnvironment-PowerApps` en la lista y confirma su estado como "Listo" (Ready).

**Resultado Esperado:**

Has navegado exitosamente por las cuatro secciones principales (Inicio, Aplicaciones, Flujos, Tablas) y has verificado el entorno en el Centro de Administración. Cada sección corresponde a un componente del ecosistema:

| Sección del Portal | Componente Power Platform |
|---|---|
| Aplicaciones | Power Apps |
| Flujos | Power Automate |
| Tablas | Dataverse |
| Centro de Administración | Gobernanza y gestión |

**Verificación:**

- ✅ Puedes acceder a las cuatro secciones sin errores de permisos.
- ✅ El entorno `LabEnvironment-PowerApps` aparece en el Centro de Administración con estado "Listo".
- ✅ La sección "Tablas" muestra al menos las tablas estándar de Dataverse (Cuenta, Contacto, etc.).

---

### Paso 3: Explorar Copilot en Power Apps

**Objetivo:** Identificar las capacidades de asistencia de IA (Copilot) dentro de Power Apps y describir escenarios donde puede acelerar el desarrollo.

**Instrucciones:**

1. Regresa a la pestaña de `https://make.powerapps.com` (asegúrate de que el entorno sigue siendo `LabEnvironment-PowerApps`).
2. En el menú lateral izquierdo, haz clic en **"Crear"**.
3. Observa la sección principal de la página "Crear". Localiza la opción que dice **"Describe la aplicación que deseas crear"** o **"Start with Copilot"** (el texto puede variar según el idioma configurado).
4. En el campo de texto de Copilot, **NO presiones Enter todavía**. Simplemente escribe el siguiente texto como ejemplo de exploración:
   ```
   Una aplicación para gestionar el inventario de productos con nombre, categoría, cantidad y precio
   ```
5. Observa las sugerencias que aparecen debajo del campo. Copilot puede mostrar:
   - Una estructura de tabla sugerida.
   - Opciones para refinar la descripción.
   - Un botón para generar la aplicación.
6. **NO generes la aplicación en este momento.** El objetivo es solo explorar la interfaz de Copilot. Haz clic en otro lugar de la página o presiona **Escape** para salir del campo.
7. Adicionalmente, observa si existe una opción de **"Empezar con datos"** que permite conectar a Excel, SharePoint u otras fuentes. Esto complementa la experiencia de Copilot.

**Resultado Esperado:**

Has identificado la interfaz de Copilot en Power Apps. La funcionalidad permite describir en lenguaje natural lo que necesitas y la IA genera una estructura de datos y una aplicación base. Dos escenarios claros donde Copilot acelera el desarrollo:

1. **Generación rápida de prototipos:** Describir una app en texto y obtener una versión funcional en segundos, eliminando la configuración manual de tablas y pantallas.
2. **Creación automática de esquemas de datos:** Copilot sugiere columnas y tipos de datos basándose en la descripción, reduciendo errores de diseño inicial.

**Verificación:**

- ✅ El campo de entrada de Copilot es visible y acepta texto.
- ✅ Puedes identificar al menos la opción de "Describe la aplicación que deseas crear" en la página Crear.
- ✅ No se generó ninguna aplicación (solo exploración).

---

### Paso 4: Analizar el Caso de Negocio — Distribuidora Norte S.A.

**Objetivo:** Aplicar el conocimiento del ecosistema Power Platform para identificar qué componente resuelve cada necesidad de un caso de negocio ficticio.

**Instrucciones:**

Lee el siguiente caso de negocio y luego completa el análisis:

> **Caso: Distribuidora Norte S.A.**
>
> Distribuidora Norte S.A. es una empresa mediana de distribución de productos de oficina con 45 empleados. Actualmente enfrentan los siguientes problemas:
>
> **Problema 1:** Los vendedores en campo registran pedidos en papel y los entregan al final del día. Esto causa retrasos de 24 horas en el procesamiento.
>
> **Problema 2:** Cuando un producto llega a stock crítico (menos de 10 unidades), nadie se entera hasta que un cliente lo solicita y no hay existencias.
>
> **Problema 3:** El gerente general solicita reportes semanales de ventas por categoría, pero el equipo de administración tarda 2 días en compilar los datos manualmente desde hojas de Excel.
>
> **Problema 4:** Los clientes frecuentes preguntan constantemente por el estado de sus pedidos y deben llamar por teléfono para obtener información.

1. Abre una nueva pestaña y navega a **OneDrive**:
   ```
   https://onedrive.live.com
   ```
   O accede desde `https://www.office.com` > ícono de OneDrive.

2. En OneDrive, crea una nueva carpeta llamada:
   ```
   Lab_PowerApps
   ```

3. Dentro de la carpeta `Lab_PowerApps`, crea un nuevo archivo de texto o documento de Word. Para crear un documento Word en línea:
   - Haz clic en **"+ Nuevo"** > **"Documento de Word"**.
   - Nómbralo: `Analisis_Caso_PowerPlatform`

4. En el documento, escribe el siguiente análisis (puedes copiar la estructura y completar con tus propias palabras):

```
ANÁLISIS DE CASO: DISTRIBUIDORA NORTE S.A.
============================================
Fecha: [fecha actual]
Estudiante: [tu nombre]
Entorno: LabEnvironment-PowerApps

COMPONENTES DE POWER PLATFORM IDENTIFICADOS:
---------------------------------------------

1. Power Apps
   - Función: Creación de aplicaciones empresariales de bajo código
   - Problema que resuelve: Problema 1
   - Solución propuesta: Aplicación móvil para que los vendedores
     registren pedidos en tiempo real desde el campo.

2. Power Automate
   - Función: Automatización de flujos de trabajo y procesos repetitivos
   - Problema que resuelve: Problema 2
   - Solución propuesta: Flujo automatizado que monitorea el stock
     y envía notificación por correo/Teams cuando un producto
     baja de 10 unidades.

3. Power BI
   - Función: Análisis de datos y visualización de información
   - Problema que resuelve: Problema 3
   - Solución propuesta: Dashboard automático conectado a los datos
     de ventas que muestra reportes por categoría en tiempo real.

4. Power Pages
   - Función: Creación de sitios web externos orientados a datos
   - Problema que resuelve: Problema 4
   - Solución propuesta: Portal web donde los clientes pueden
     consultar el estado de sus pedidos con su número de orden.

5. Copilot Studio
   - Función: Creación de agentes conversacionales con IA
   - Problema que resuelve: Problema 4 (complemento)
   - Solución propuesta: Chatbot integrado en el portal web que
     responde preguntas frecuentes sobre estado de pedidos.

COMPONENTES TRANSVERSALES:
--------------------------
- Dataverse: Almacena todos los datos (pedidos, productos, clientes)
  de forma centralizada y segura.
- Conectores: Integran Power Platform con Outlook, Teams, Excel
  y sistemas externos de la empresa.

ESCENARIOS DE COPILOT EN POWER PLATFORM:
-----------------------------------------
Escenario 1: Generar la aplicación de registro de pedidos describiendo
en lenguaje natural las pantallas y campos necesarios.

Escenario 2: Crear automáticamente el flujo de notificación de stock
describiendo la condición y la acción deseada a Copilot en Power Automate.

CONFIGURACIÓN DEL ENTORNO VERIFICADA:
--------------------------------------
- Entorno activo: LabEnvironment-PowerApps [✓]
- Credenciales funcionando: [✓]
- Acceso a Aplicaciones: [✓]
- Acceso a Flujos: [✓]
- Acceso a Tablas (Dataverse): [✓]
- Centro de Administración accesible: [✓]
```

5. Guarda el documento (se guarda automáticamente en OneDrive si usas Word Online).

**Resultado Esperado:**

El documento `Analisis_Caso_PowerPlatform` está guardado en la carpeta `Lab_PowerApps` de OneDrive. Contiene el mapeo de los cinco componentes de Power Platform con los problemas del caso de negocio, los escenarios de Copilot y la confirmación de la configuración del entorno.

**Verificación:**

- ✅ La carpeta `Lab_PowerApps` existe en OneDrive.
- ✅ El documento `Analisis_Caso_PowerPlatform` está guardado y es accesible.
- ✅ El documento incluye los 5 componentes de Power Platform con su función y problema asociado.
- ✅ Se identifican al menos 2 escenarios de uso de Copilot.

---

## Validación y Pruebas Finales

Completa la siguiente lista de verificación para confirmar que el laboratorio se realizó exitosamente:

| # | Criterio de Validación | Estado |
|---|---|---|
| 1 | Inicio de sesión exitoso en make.powerapps.com | ☐ |
| 2 | Entorno `LabEnvironment-PowerApps` seleccionado y visible | ☐ |
| 3 | Navegación exitosa por secciones: Inicio, Aplicaciones, Flujos, Tablas | ☐ |
| 4 | Centro de Administración accesible y entorno verificado como "Listo" | ☐ |
| 5 | Interfaz de Copilot explorada en la sección "Crear" | ☐ |
| 6 | Carpeta `Lab_PowerApps` creada en OneDrive | ☐ |
| 7 | Documento `Analisis_Caso_PowerPlatform` guardado con contenido completo | ☐ |

---

## Solución de Problemas

### Problema 1: El entorno `LabEnvironment-PowerApps` no aparece en el selector de entornos

**Síntomas:** Al hacer clic en el selector de entornos en la esquina superior derecha de make.powerapps.com, solo aparece el entorno "Default" o "(personal)" y no se lista `LabEnvironment-PowerApps`.

**Causa:** El administrador del tenant no ha asignado un rol de seguridad al usuario dentro del entorno, o la asignación de licencia aún no se ha propagado (puede tardar hasta 15 minutos después de la asignación).

**Solución:**
1. Cierra sesión completamente de make.powerapps.com (clic en tu avatar > "Cerrar sesión").
2. Cierra todas las pestañas del navegador.
3. Borra la caché del navegador: `Ctrl + Shift + Delete` > selecciona "Cookies" e "Imágenes en caché" > Borrar datos.
4. Vuelve a iniciar sesión en `https://make.powerapps.com`.
5. Si el problema persiste, solicita al instructor que verifique en el Centro de Administración (`admin.powerplatform.microsoft.com` > Entornos > `LabEnvironment-PowerApps` > Configuración > Usuarios) que tu cuenta tiene asignado al menos el rol **"Environment Maker"**.

---

### Problema 2: La sección "Crear" no muestra la opción de Copilot

**Síntomas:** Al navegar a la sección "Crear" en make.powerapps.com, no aparece el campo de texto para describir una aplicación con Copilot. Solo se muestran las opciones tradicionales como "Aplicación en blanco" o "Desde datos".

**Causa:** La funcionalidad de Copilot puede estar deshabilitada a nivel de tenant por políticas del administrador, o el idioma/región configurados en el perfil del usuario no están entre los soportados para Copilot (actualmente disponible principalmente en inglés de EE.UU.).

**Solución:**
1. Verifica que el idioma del portal esté configurado en inglés: haz clic en el ícono de engranaje (⚙️) en la esquina superior derecha > "Idioma" > selecciona **"English (United States)"**.
2. Recarga la página (`F5` o `Ctrl + R`).
3. Si aún no aparece, navega directamente a la URL:
   ```
   https://make.powerapps.com/environments/[environment-id]/home
   ```
4. Si la funcionalidad sigue sin aparecer, documenta en tu archivo de análisis que Copilot no está disponible en tu tenant y describe los escenarios de forma teórica basándote en la documentación oficial. Esto no afecta la calificación del laboratorio.

---

## Limpieza

Este laboratorio es el punto de partida de la serie. **No elimines ningún recurso creado:**

- ✅ **Mantén** la carpeta `Lab_PowerApps` en OneDrive — se utilizará en laboratorios posteriores.
- ✅ **Mantén** el documento `Analisis_Caso_PowerPlatform` — servirá como referencia.
- ✅ **Mantén** el entorno `LabEnvironment-PowerApps` seleccionado como tu entorno activo para los próximos laboratorios.
- ⚠️ **No crees aplicaciones ni flujos adicionales** en este laboratorio. La creación de recursos comenzará en el Lab 03.

---

## Resumen

En este laboratorio has completado las siguientes actividades fundamentales:

| Actividad | Componente Asociado |
|---|---|
| Autenticación y selección de entorno | Gobernanza / Admin Center |
| Exploración de secciones del portal | Power Apps, Power Automate, Dataverse |
| Identificación de Copilot | IA en Power Platform |
| Análisis de caso de negocio | Todos los componentes |
| Documentación en OneDrive | Microsoft 365 |

**Conceptos clave reforzados:**
- Power Platform es un ecosistema de cinco pilares integrados, no herramientas aisladas.
- Cada componente tiene un rol específico: Power Apps (aplicaciones), Power Automate (automatización), Power BI (análisis), Power Pages (sitios web), Copilot Studio (agentes IA).
- Dataverse y los conectores son la infraestructura transversal que une todo el ecosistema.
- Los entornos permiten organizar y aislar recursos según su propósito.
- Copilot acelera el desarrollo permitiendo describir soluciones en lenguaje natural.

### Recursos Adicionales

- [Documentación oficial de Power Platform](https://learn.microsoft.com/es-es/power-platform/)
- [Introducción a Copilot en Power Apps](https://learn.microsoft.com/es-es/power-apps/maker/canvas-apps/ai-overview)
- [Administración de entornos](https://learn.microsoft.com/es-es/power-platform/admin/environments-overview)
- [Descripción general de Dataverse](https://learn.microsoft.com/es-es/power-apps/maker/data-platform/data-platform-intro)

---
