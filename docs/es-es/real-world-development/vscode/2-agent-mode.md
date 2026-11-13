---
title: "Ejercicio 2: Añade funcionalidad con el modo agente de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y tareas como ejecutar pruebas. Normalmente, el flujo de trabajo de un desarrollador consiste en localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta, hacer otra actualización... La lista continúa.

Para eso sirve el modo agente de Copilot.

El modo agente de Copilot está diseñado para trabajar con más autonomía en el IDE. Se comporta de forma parecida a un desarrollador: primero explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Veamos cómo usarlo para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque sus nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en tu IDE para permitir ciclos rápidos de comentarios e interacción. El agente en la nube actúa como un compañero de programación y trabaja de forma asíncrona, como un miembro del equipo, mediante incidencias y solicitudes de cambios.

En este ejercicio aprenderás:

- cómo el modo agente de Copilot puede explorar el proyecto, identificar archivos pertinentes y coordinar cambios.
- cómo puede implementar funciones que abarcan la interfaz de usuario y la capa de datos.
- cómo revisar los cambios y las pruebas generados por el modo agente antes de fusionarlos en el código.

## Situación

El catálogo de juegos está creciendo y quieres permitir que los usuarios filtren por categoría y editorial. Ya has creado un asistente de editoriales en el ejercicio anterior. Ahora completarás el resto de la capa de datos, la interfaz y las pruebas con el modo agente de Copilot.

## Ejecuta el sitio web Tailspin Toys

Antes de hacer cambios, exploremos el sitio web de Tailspin Toys para conocer su funcionalidad actual.

El sitio es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar los juegos y sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM en su frontmatter; no hay una API de backend independiente ni un framework de interfaz del lado del cliente. Los asistentes de acceso a datos reutilizables están en `src/lib/`, y la interactividad se añade mediante un pequeño elemento `<script>` de Astro con ámbito limitado y las API estándar del DOM.

### Inicia el sitio web

Para facilitar el inicio del sitio, hay una tarea `npm run dev` que ejecuta el único servidor de desarrollo de Astro. Para iniciarlo en GitHub Codespaces:

1. Vuelve a tu codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal en el codespace con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Actualiza `main` y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, verás un mensaje con la URL parecido al siguiente:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir un navegador con `http://localhost:4321`, ciérralo seleccionando **x**.

5. Mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, seleccionar un enlace `localhost` en el terminal redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado a tu codespace, que ahora aloja el servidor web.

### Explora el sitio web

Con el sitio en ejecución, puedes explorar sus funciones principales:

- **Página de inicio**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. La selección del modo y el modelo varía según el ejercicio; cada paso lo indica cuando es necesario.

La implementación inicial del sitio funciona, pero queremos añadir capacidades. Empecemos por revisar el backlog. Al crear el proyecto desde la plantilla, se generó automáticamente un backlog de incidencias. Pide a GitHub Copilot que te muestre esas tareas.

1. Selecciona **Agent** en la lista de agentes de la vista Chat. El agente puede planificar e implementar cambios en varios archivos de forma autónoma, ejecutar comandos de terminal y usar herramientas.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no están recomendando ningún modelo. Al prepararlo se usó Claude Sonnet 4.5 y por eso se menciona en las instrucciones. El objetivo es que las sugerencias de código sean relativamente coherentes y la experiencia, satisfactoria. Sin embargo, como los modelos de lenguaje grandes son probabilísticos, las sugerencias pueden diferir de los ejemplos del taller. Es normal y esperable.

> [!NOTE]
> Debido al carácter probabilístico de los modelos de lenguaje grandes, Copilot podría usar otro comando MCP y aun así completar la tarea.

3. Pide a Copilot que te muestre el backlog con este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para permitir que ejecute el comando y enumere las incidencias.
5. Revisa la lista generada.

Fíjate en que Copilot ha priorizado las tareas según lo que considera más útil para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que le proporcionará contexto. Usarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio web.

1. En tu codespace, ve a `.github/instructions/ui.instructions.md`.
2. Revisa las directrices generales para añadir funcionalidad. Incluyen:
   - una descripción general de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a archivos de instrucciones específicos para distintos tipos de archivo, como:
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> Los archivos de instrucciones pueden incluir referencias a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones, lo que permite reutilizar instrucciones, dividir conjuntos complejos en partes más manejables e incluir ejemplos y plantillas.

## Implementa el filtrado

Para completar el filtrado, la aplicación necesita al menos tres cambios:

- añadir o ajustar la lógica restante del filtrado en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento del filtrado.
- actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Además, hay que ejecutar las pruebas y comprobar que se superan antes de fusionar los cambios. El modo agente de Copilot puede realizar estas tareas. Vamos a añadir la funcionalidad.

1. Continúa la conversación actual con Copilot o inicia otra seleccionando **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** para usar el archivo de instrucciones de interfaz.

   ![Captura de cómo seleccionar el archivo de instrucciones de interfaz de usuario](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en la lista de agentes de la vista Chat.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que sigue seleccionado el modelo **Claude Sonnet 4.5**.
5. Pide a Copilot que implemente la función según la incidencia del backlog con este prompt:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la función, incluidos los asistentes de la capa de datos, la interfaz y las pruebas. Después, empieza a modificar los archivos y a ejecutar las pruebas.

   ![Captura de Copilot mientras explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. Según la complejidad de la tarea y del código, puede tardar varios minutos. Durante el proceso, podrían aparecer los controles **Keep** y **Undo** en el editor. Cuando termine, podrás aceptar o deshacer todos los cambios; no tienes que seleccionar esos controles mientras el agente trabaja.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de un cuadro de diálogo de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas e incluso que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Captura de una sesión de Copilot Chat completada con el modo agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio has aprendido a usar el modo agente de GitHub Copilot para añadir funciones al sitio Tailspin Toys. Has visto cómo:

- el modo agente de GitHub Copilot puede implementar funciones en la interfaz y en la capa de datos.
- el modo agente puede explorar el proyecto, identificar archivos pertinentes y coordinar cambios.
- revisar los cambios y las pruebas generados antes de fusionarlos en el código.

Ahora [probaremos la función con el servidor MCP de Playwright][next-lesson] y abriremos una solicitud de cambios.

### Ejercicio extra: implementar la paginación

A medida que crezca la lista de juegos, será necesario habilitar la paginación. Aplica lo aprendido en este ejercicio y pide a Copilot que la implemente en el sitio. Al planificar el cambio:

- sigue las prácticas recomendadas existentes, incluidos los archivos de instrucciones del proyecto.
- decide cómo quieres implementar la paginación y si el usuario podrá elegir el tamaño de página.
- proporciona a Copilot la orientación necesaria para obtener la implementación que necesitas.
- puede que tengas que iterar con Copilot y pedirle cambios o contexto adicional. Es normal trabajar así con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo usarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
