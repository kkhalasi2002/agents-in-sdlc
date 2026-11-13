---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil QA y las pruebas; después, crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de instrucciones y de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una sola rama. Es hora de revisarlos juntos y abrir una solicitud de incorporación de cambios. Ya has combinado personalmente la solicitud de incorporación de cambios (PR) de valoraciones por estrellas; esta vez permitirás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la función, las actualizaciones de instrucciones, la actualización de la habilidad y el agente de control de calidad en varias PR. Para simplificar el taller, has mantenido todo el flujo de trabajo de filtrado y calidad en una sesión y rama, y lo incluirás en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- inspeccionarás la PR completa de la función y las pruebas de verificación.
- autorizarás Agent Merge solo después de la revisión y confirmarás que la PR se ha fusionado.

## Escenario

A lo largo del flujo de filtrado, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR y mantener la autorización para combinar bajo el control del desarrollador.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de cambios en la aplicación GitHub Copilot. Al habilitarlo, la sesión lee la solicitud y resuelve lo que la bloquea: corrige comprobaciones de integración continua (CI) con errores, responde a comentarios de revisión y actualiza la base cuando es necesario. La fusión solo se produce cuando GitHub lo permite. El proceso funciona en segundo plano, continúa si se reinicia la aplicación y se desactiva cuando se fusiona la solicitud.

Hasta ahora has seleccionado **Merge pull request** personalmente. Agent Merge puede asumir esa responsabilidad, pero su capacidad para editar código y fusionar sigue necesitando tu autorización explícita. Antes de conceder permiso, revisa las acciones que tiene permitidas y el trabajo realizado.

## Utilizar Agent Merge para gestionar la PR

Con todo el código creado y revisado, deja que Agent Merge gestione el proceso de la PR.

1. Utiliza el selector de agentes para seleccionar **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambia a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción del menú desplegable no lo inicia por sí solo.

El proceso de Agent Merge comienza. Se encargará de:

- Crear la solicitud de incorporación de cambios con un título y una descripción.
- Si iniciaste la sesión desde una incidencia, incluir una referencia a la incidencia relacionada en el cuerpo de la descripción.
- Actualizar la base o resolver posibles conflictos con la rama de destino.
- Supervisar el proceso de CI para garantizar que se superen todas las comprobaciones.
- Supervisar la PR para detectar comentarios de otros desarrolladores o de la revisión de código de Copilot, y aplicar los cambios necesarios para resolverlos.
- Fusionar la PR cuando todo se haya completado correctamente, si lo autorizas.

Dejemos que Agent Merge también fusione la PR cuando se hayan superado todas las comprobaciones.

5. Revisa el diff de la PR y las pruebas de verificación antes de conceder permiso para fusionar.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Comprueba que **Merge pull request** está marcado.
8. Pide explícitamente al agente que fusione la PR:

   ```plaintext
   Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
   ```

9. Supervisa el progreso y resuelve los bloqueos que se indiquen. Antes de continuar, confirma que el estado de la PR es **Merged**. Tener habilitado el permiso de fusión o un agente en ejecución no confirma que la PR se haya fusionado.

> [!IMPORTANT]
> Agent Merge no elude las protecciones del repositorio ni los permisos ausentes. Resuelve esos bloqueos antes de continuar.

## Resumen y pasos siguientes

Has automatizado varias partes del proceso de desarrollo, como la generación, las pruebas y la validación de código, y ahora también el proceso de solicitud de incorporación de cambios. En concreto:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- has examinado la PR completa de la funcionalidad y las pruebas de verificación.
- has autorizado Agent Merge solo después de la revisión y has confirmado que la PR estaba combinada.

A continuación, [utilizarás un lienzo existente y crearás uno de clasificación][next-lesson] para explorar una forma más completa de examinar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app