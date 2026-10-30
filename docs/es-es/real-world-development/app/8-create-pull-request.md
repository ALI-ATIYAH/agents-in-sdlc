---
title: "Lección 8 - Crea y combina la PR de la funcionalidad"
description: "Revisa en conjunto el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; después, autoriza Agent Merge para crear y combinar la segunda PR."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de instrucciones y de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Es hora de revisarlos en conjunto y abrir una solicitud de incorporación de cambios. Ya has combinado personalmente la solicitud de incorporación de cambios (PR) de las valoraciones con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de instrucciones y de la habilidad, y el agente de QA en varias PR. Para agilizar el taller, has mantenido todo el flujo de filtrado y calidad en una sesión y una rama, y todo ese trabajo se incluirá en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- inspeccionarás la PR completa de la funcionalidad y las pruebas de validación.
- autorizarás Agent Merge solo después de la revisión y confirmarás que la PR se ha combinado.

## Escenario

A lo largo del flujo de filtrado, has utilizado Copilot para planificar, implementar y validar una funcionalidad. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR, pero mantener bajo el control del desarrollador la autorización para combinarla.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios en la aplicación GitHub Copilot. Al activarlo, la sesión de la aplicación lee la solicitud y resuelve los bloqueos: corrige las comprobaciones de integración continua (CI) fallidas, responde a los comentarios de revisión y vuelve a aplicar los cambios sobre la rama base cuando sea necesario. Después, combina la solicitud en cuanto GitHub lo permite. Se ejecuta en segundo plano, sigue funcionando aunque se reinicie la aplicación y se desactiva cuando se combina la solicitud.

Hasta ahora seleccionabas **Merge pull request** personalmente. Agent Merge puede encargarse de esa tarea, pero su capacidad para editar código y combinar todavía requiere tu autorización explícita. Revisa las acciones permitidas y el trabajo antes de conceder permiso para combinar.

## Usa Agent Merge para gestionar la PR

Ya has creado y revisado todo el código. Dejemos que Agent Merge gestione el proceso de la PR.

1. Usa el selector de agentes para seleccionar **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción del menú desplegable no lo inicia por sí solo.

Comenzará el proceso de Agent Merge. Hará lo siguiente:

- Crear la solicitud de incorporación de cambios con un título y una descripción.
- Si has iniciado la sesión desde una incidencia, incluir una referencia a la incidencia relacionada en el cuerpo de la descripción.
- Volver a aplicar los cambios sobre la rama de destino o resolver posibles conflictos de combinación.
- Supervisar el proceso de CI para comprobar que se superen todas las comprobaciones.
- Supervisar la PR para detectar comentarios de otros desarrolladores o de la revisión de código de Copilot. Hará cambios para resolverlos.
- Opcionalmente, combinar automáticamente la PR cuando todo se haya completado correctamente.

Pidamos a Agent Merge que combine la PR cuando se hayan superado todas las comprobaciones.

5. Revisa las diferencias de la PR y las pruebas de validación antes de conceder permiso para combinar.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Comprueba que **Merge pull request** esté marcado.
8. Pide explícitamente al agente que combine la solicitud:

   ```plaintext
   Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
   ```

9. Sigue el progreso y resuelve los bloqueos que se indiquen. Antes de continuar, confirma que el estado de la PR sea **Merged**. Tener habilitado el permiso de combinación o un agente en ejecución no significa que la solicitud se haya combinado.

> [!IMPORTANT]
> Agent Merge no omite las protecciones del repositorio ni los permisos que falten. Resuelve esos bloqueos antes de continuar.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: la generación de código, las pruebas y la validación, y ahora también el proceso de la solicitud de incorporación de cambios. En esta lección:

- aprendiste qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- inspeccionaste la PR completa de la funcionalidad y las pruebas de validación.
- autorizaste Agent Merge solo después de la revisión y confirmaste que la PR se había combinado.

A continuación, [usarás un lienzo existente y crearás uno de clasificación][next-lesson] para explorar una forma más completa de examinar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Gestiona incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
