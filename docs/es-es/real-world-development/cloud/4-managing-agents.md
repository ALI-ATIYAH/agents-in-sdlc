---
title: "Ejercicio 4 - Supervisa y gestiona los agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores pediste al agente en la nube de Copilot que se ocupara de tres tareas diferentes para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente en la nube está diseñado para trabajar de forma autónoma y asíncrona, es importante poder supervisar estas tareas.

Dispones de varias herramientas para gestionar las tareas asignadas al agente en la nube, incluida la [página de agentes][agents-page] de GitHub.com. Desde este centro puedes ver todas las tareas del agente que tienen solicitudes de incorporación de cambios (PR) abiertas. También puedes explorar las operaciones realizadas e incluso guiar una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- guiarás una sesión en curso para solicitar una funcionalidad adicional.

## Escenario

Después de asignar al agente la creación de un modo de contraste alto, el equipo decidió que sería buena idea añadir también un modo claro. Como ya se estaba actualizando el estilo del sitio y añadiendo la funcionalidad para cambiar de modo, parecía lógico incluirlo. Vas a orientar el trabajo del agente para que añada tanto el modo claro como el de contraste alto.

## Revisa las tareas del agente en la nube de Copilot

Veamos el estado actual de todas las tareas asignadas al agente en la nube de Copilot.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en el panel principal y en el panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código.
   - generar API para modificar juegos.
   - añadir un modo de contraste alto al sitio web.
3. Selecciona una de las tareas en curso. Revisa las operaciones que ha realizado Copilot, que pueden incluir:
   - obtener el código del repositorio.
   - preparar el entorno para Copilot.
   - configurar servidores MCP.
   - realizar varios pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos concretos varían según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Observa también el panel de la solicitud de incorporación de cambios (PR), que aparece a la derecha. Desde allí puedes consultar la PR y los archivos modificados para seguir el trabajo.

## Guía al agente en la nube

Ahora que has visto las tareas activas, pidamos a Copilot que incluya el modo claro mientras trabaja en el modo de contraste alto.

1. Selecciona la sesión relacionada con el modo de contraste alto. El título exacto dependerá del nombre que le haya dado Copilot y del estado del trabajo.

   ![Sesión de accesibilidad en el centro de control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado de configurar el entorno y ha empezado a trabajar. Lo sabrás cuando empiecen a aparecer mensajes similares a los que se muestran a continuación.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, añade este mensaje:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Captura de la tarea del agente en la nube en la página de agentes con el cuadro de diálogo para guiar la sesión activa de Copilot resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el mensaje.
5. Observa que Copilot lo confirma y lo incorpora a su trabajo.

## Deja que Copilot trabaje

Como antes, Copilot empezará a trabajar en la tarea actualizada. Incorporará la nueva solicitud a su flujo cuando termine el paso en el que estaba trabajando al recibir el mensaje.

El proceso tardará varios minutos. Es un buen momento para hacer una pausa y reflexionar sobre todo lo que has aprendido y explorado hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro para supervisar y guiar las tareas del agente en la nube de GitHub Copilot. Desde allí puedes seguir las tareas activas y completadas, revisar el trabajo realizado y redirigir una tarea en curso para ajustar su alcance o añadir indicaciones.

Has explorado estos conceptos:

- exploraste el centro de control y la página de agentes para supervisar las tareas del agente en la nube.
- redirigiste una sesión en curso para solicitar funcionalidad adicional.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Itera sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
