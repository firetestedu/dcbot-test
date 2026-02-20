

# Entregable Unidad 1: Dashboard Moderador de Discord

Este documento contiene los elementos solicitados para la entrega de la **Unidad 1** del proyecto integrador (Adaptación de un Bot de Discord).

## 1. Documento de formalización de proyecto

Este punto se cubre llenando y firmando los documentos en PDF proporcionados por el docente (`Proyecto Integrador.pdf` y `Responsabilidades por Rol.pdf`).

* **Nombre sugerido para el proyecto:** Dashboard de Monitoreo y Gestión en Tiempo Real para Bot de Discord.
* *(Nota: Asegúrense de subir los PDF firmados junto con este documento).*

## 2. Boceto de Interfaces

El siguiente boceto ilustra la propuesta visual del Panel de Control principal, enfocado en la gestión de la comunidad. Esta ventana será desarrollada en la Unidad 1.

![Mockup del Dashboard](https://media.discordapp.net/attachments/1120559719094960209/1474515053179965554/dashboard_mockup_1771621155035.png?ex=699a20a7&is=6998cf27&hm=31aad5dc3d576addff6fb3037b857df05656ff87bde6fd50b3b55d5b5dd7649a&=&format=webp&quality=lossless)

## 3. Usuarios

El sistema está diseñado para ser utilizado principalmente por los miembros del equipo de administración de una comunidad en Discord. Se identifican dos tipos de usuarios que interactuarán con este Dashboard:

1. **Administrador Principal (Owner):** Tiene acceso total al panel. Puede ver todas las estadísticas globales, modificar la configuración sensible del bot, configurar el "automod", y ver el historial completo (Logs) del servidor.
2. **Moderador:** Tiene acceso enfocado a la pantalla mostrada en el boceto. Su tarea principal es revisar la lista de miembros de Discord, leer las infracciones que ha cometido cada usuario, y utilizar el panel lateral para aplicar advertencias (*Warnings*), expulsiones (*Kicks*) o Baneos.

## 4. Eventos detectados

De acuerdo a la interacción del Moderador con la interfaz gráfica, se implementarán los siguientes eventos clave:

* **`onKeyTyped` (Evento de Teclado):** Al escribir en la barra de búsqueda, se filtran dinámicamente las filas de la tabla de usuarios.
* **`onRowSelected` (Evento de Selección):** Hacer clic en un usuario específico dentro de la tabla abre el panel lateral derecho para ver su detalle o amonestarlo.
* **`onClick` / `onAction` (Eventos de Botón):**
    * Clic en botones de navegación lateral para cambiar de pantalla.
    * Clic en el botón "Submit" del panel lateral para confirmar y enviar una advertencia (*Warning*).
* **`onValidationFail` (Evento de Validación del Sistema):** Si el moderador intenta aplicar un castigo pero deja vacío el campo "Razón", la interfaz generará una alerta visual.

## 5. Complementos a implementar (Componentes GUI)

Para construir la interfaz gráfica ilustrada en el boceto y manejar los eventos descritos, se utilizarán los siguientes componentes principales:

* **`TableView` / `DataGrid`:** Para mostrar la lista estructurada de usuarios, sus IDs y número de advertencias.
* **`TextField`:** Para campos de entrada de texto cortos, como la barra de búsqueda o el campo de "Reason" (Razón).
* **`ComboBox` / `ChoiceBox`:** Lista desplegable para que el moderador seleccione cuántos puntos (*Points*) o qué nivel de severidad tendrá la amonestación.
* **`Buttons`:** Para accionar comandos concretos (navegación, "Submit", "Ban", "Kick").
* **`Label` / `Text`:** Para mostrar títulos descriptivos y valores numéricos en las tarjetas de estadísticas ("Total Users", "Active Bans").
* **`Sliding Panel` / `Dialog`:** Un componente contenedor que aparece de un lado (o como ventana modal) para albergar el formulario de castigo sin salir de la vista principal.

## 6. Posibles flujos (Diagrama de Flujo de Eventos)

El siguiente diagrama detalla un flujo de eventos típico: Un moderador decide aplicar una amonestación (*Warning*) a un miembro a través del Dashboard.

```mermaid
flowchart TD
    %% Inicio de Interacción
    A([Inicio: Sesión Moderador]) --> B[Abre Vista 'Usuarios']
    B --> C((Evento:\nEscribir en Buscador))
    C --> D[Filtra Tabla Dinámicamente]
    D --> E((Evento:\nClic en fila de Usuario))
    
    %% Despliegue de Formulario
    E --> F[Abre Panel Lateral 'Aplicar Warning']
    F --> G[Mod: Selecciona Puntos en ComboBox]
    F --> H[Mod: Escribe Razón en TextField]
    G --> I((Evento:\nClic 'Submit'))
    H --> I
    
    %% Validación y Sistema
    I --> J{¿Datos Válidos?}
    J -- No --> K[Evento Sistema:\nMostrar Mensaje 'Razón Requerida']
    K --> F
    
    J -- Sí --> L[Evento Sistema:\nGuardar en BD log y Puntos]
    L --> M[Evento Sistema:\nActualizar Tabla de Usuarios]
    M --> N([Fin: Muestra Toast 'Usuario Amonestado'])
```

```

```
