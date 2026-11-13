---
title: "Lección 8: Crea y fusiona la PR de la función"
description: "Revisa en conjunto el filtrado, las instrucciones, la habilidad, el perfil de control de calidad y las pruebas; después, crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones, la actualización de la habilidad, el perfil de control de calidad y las pruebas están guardados en una rama. Es hora de revisarlos en conjunto y abrir una solicitud de cambios. Fusionaste por tu cuenta la PR de valoración por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la función, las actualizaciones de instrucciones, la habilidad y el agente de control de calidad en varias PR. Para simplificar el taller, has mantenido todo el trabajo de filtrado y calidad en una conversación y rama, y lo incluirás en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- inspeccionarás todos los cambios de la función y las pruebas de verificación.
- crearás la PR del filtrado.
- activarás Agent Merge solo después de revisar los cambios y confirmarás que la PR se ha fusionado.

## Situación

Durante el proceso de creación del filtrado, has usado Copilot para planificar, implementar y verificar una función. Ahora Tailspin Toys quiere automatizar el trabajo pendiente de la PR y mantener el control del desarrollador sobre la autorización de fusión.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de cambios. Cuando lo activas, Copilot resuelve los bloqueos de la PR —corrige comprobaciones de integración continua (CI) que hayan fallado, responde a comentarios de revisión y vuelve a aplicar los cambios necesarios— y, después, activa la fusión automática de GitHub si el repositorio lo permite.

Hasta ahora seleccionabas **Merge pull request** personalmente. Agent Merge puede encargarse de esta parte, pero debes revisar el trabajo y confirmar que está listo antes de activarlo.

## Usa Agent Merge para gestionar la PR

Ya tienes todo el código. Revisémoslo, creemos la PR y dejemos que Agent Merge se ocupe del resto.

1. Vuelve a Codespace.
2. Abre el cuadro de diálogo del agente escribiendo `/agent`.
3. Selecciona **Default** en la lista de opciones y pulsa <kbd>Enter</kbd>.
4. Crea una PR nueva con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todo termine correctamente, realizará la fusión.
7. Comprueba que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de fusión, la configuración del repositorio ni los permisos que falten. Si se bloquea, revisa el motivo indicado y completa manualmente la fusión revisada cuando el repositorio lo permita.

## Resumen y siguientes pasos

Has automatizado varios aspectos del proceso de desarrollo: generar código, probarlo y validarlo, y gestionar las solicitudes de cambios. En esta lección:

- aprendiste qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- inspeccionaste todos los cambios de la función y las pruebas de verificación.
- creaste la PR del filtrado.
- activaste Agent Merge solo después de revisar los cambios y confirmaste que la PR se había fusionado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional al agente en la nube.

## Recursos

- [Gestionar solicitudes de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
