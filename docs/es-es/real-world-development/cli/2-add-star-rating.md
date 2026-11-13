---
title: "Lección 2: Añade valoraciones por estrellas: un cambio rápido"
description: "Usa Copilot CLI para hacer un pequeño cambio en las tarjetas de juegos, revísalo en un navegador reenviado y fusiónalo como tu primera solicitud de cambios."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ahora que has instalado Copilot CLI y probado una conversación, harás tu primer cambio en el proyecto. Será pequeño: los juegos ya incluyen una valoración por estrellas en sus datos, pero aún no se muestra en las tarjetas de la página principal. Pedirás al agente que la muestre, revisarás el cambio y lo fusionarás como tu primera solicitud de cambios.

En esta lección:

- iniciarás una conversación de Copilot centrada en una rama de función.
- pedirás al agente que haga un pequeño cambio en el proyecto.
- revisarás el cambio con `/diff`.
- ejecutarás la aplicación para confirmar el cambio en un navegador reenviado.
- abrirás y fusionarás tu primera solicitud de cambios.

## Situación

Cada juego de Tailspin Toys puede tener una valoración por estrellas, que ya se muestra en su página de detalles. Sin embargo, las tarjetas de la página principal solo muestran el título, la categoría, el editor y la descripción. Para empezar, pedirás al agente que muestre en cada tarjeta la valoración existente. Es un cambio pequeño y acotado, ideal para tu primera sesión.

## Anatomía de una conversación

Una **conversación** es el espacio donde trabajas con Copilot CLI en una tarea. A diferencia de la aplicación Copilot, una conversación normal de CLI usa el repositorio y la rama de Git que están seleccionados en el terminal, en lugar de crear un worktree específico. Las conversaciones guardadas te permiten volver a la misma conversación más tarde, mientras que los archivos y la rama siguen formando parte del estado normal de Git en el disco.

En una conversación verás tres elementos: tus prompts y las respuestas del agente, la actividad de las herramientas mientras explora y edita archivos, y los cambios que puedes inspeccionar con `/diff`.

## Inicia una conversación y pide el cambio

Vamos a iniciar una conversación nueva para implementar la función.

1. Vuelve a Codespace.
2. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Si aún no está abierto, inicia Copilot con el comando siguiente:

   ```bash
   copilot --yolo
   ```

4. Comprueba que se inicia una sesión nueva con el comando de barra `/new` y pulsa <kbd>Enter</kbd>.
5. Usa el prompt siguiente para pedir el cambio:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot explorará el proyecto, localizará los archivos que muestran los detalles de los juegos y creará el código necesario. ¡Ya has añadido una función con Copilot CLI!

## Revisa el diff

Antes de fusionarlos, hay que revisar todos los cambios generados por IA, incluso los pequeños. Veamos cómo inspeccionarlos desde Copilot CLI.

1. Escribe `/diff` e inspecciona todos los archivos modificados.
2. Confirma que la tarjeta muestra la valoración numérica cuando existe y `No rating yet` cuando `starRating` es `null`.
3. Confirma que las pruebas cubren ambos casos.
4. Revisa los resultados de las comprobaciones que ejecutó Copilot y pídele que corrija los fallos.
5. Cuando hayas terminado la revisión, pulsa <kbd>Esc</kbd> para salir de la vista del diff.

> [!NOTE]
> Copilot, como todas las herramientas de IA generativa, es probabilístico, no determinista; por eso, el código concreto puede variar. Revisa el comportamiento en lugar de esperar una implementación exacta.

## Comprueba los cambios

No basta con leer el código y suponer que funciona. Pidamos a Copilot que inicie el sitio para que podamos examinar la interfaz actualizada en el navegador reenviado por Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Confirma que las tarjetas muestran la valoración sobre cinco.
4. Vuelve a Copilot y pídele que detenga el servidor que ha iniciado:

   ```plaintext
   Stop the development server you started.
   ```

## Abre y fusiona tu primera solicitud de cambios

¡Ya has creado la función! Ahora abre una solicitud de cambios (PR) para integrarla en el proyecto.

1. Pide al agente predeterminado que confirme el cambio:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Escribe `/pr create`. Copilot CLI enviará la confirmación existente al crear la PR y mostrará la URL.
3. Mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la URL que muestra Copilot CLI para abrir la PR.
4. Revisa los archivos modificados y las comprobaciones.
5. Cuando esté todo listo, selecciona **Merge pull request** y confirma la fusión.
6. Vuelve a Codespace y sal de Copilot CLI con `/exit`.
7. Actualiza la rama local `main`:

   ```bash
   git checkout main
   git pull
   ```

## Resumen y siguientes pasos

¡Enhorabuena! Has publicado tu primer cambio con GitHub Copilot CLI. En concreto:

- iniciaste una conversación de Copilot centrada en una rama de función.
- pediste al agente que hiciera un pequeño cambio en las tarjetas de juegos.
- revisaste el cambio con `/diff`.
- ejecutaste la aplicación y confirmaste la valoración por estrellas en un navegador reenviado.
- abriste y fusionaste tu primera solicitud de cambios.

A continuación, [partirás de la incidencia de filtrado y usarás los modos Plan y Autopilot][next-lesson] para crear una función más amplia.

## Recursos

- [Información sobre GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
