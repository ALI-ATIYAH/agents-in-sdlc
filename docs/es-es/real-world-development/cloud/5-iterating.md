---
title: "Ejercicio 5: Itera sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le pediste que añadiera documentación al código, creara una funcionalidad de juegos relacionados para que el equipo de diseño pudiera iterar y añadiera mejoras de accesibilidad, incluidos los modos de contraste alto y claro. Vamos a revisar los cambios del código y, si hace falta, enviar comentarios a Copilot para que mejore el resultado.

### Escenario

Como ya hemos indicado varias veces, la IA generativa no cambia los fundamentos del diseño de software ni de DevOps. Siempre debemos revisar el código generado y seguir nuestro proceso normal de DevOps. Revisaremos las sugerencias de GitHub Copilot para la documentación, la funcionalidad de juegos relacionados y las mejoras de accesibilidad antes de activar la revisión para el resto del equipo.

## Seguridad y agente en la nube de GitHub Copilot

Como el agente en la nube de Copilot realiza tareas de forma asíncrona y sin supervisión, se han establecido ciertas medidas de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que usará para su código.
- El agente en la nube se ejecuta en GitHub Actions, donde crea un entorno independiente y efímero para trabajar.
- Una persona debe aprobar todos los flujos de trabajo de GitHub Actions antes de que se ejecuten.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empezaremos explorando la primera solicitud de incorporación de cambios (PR) generada por el agente en la nube de GitHub Copilot: la que añade documentación al código. Para ello, usaremos la interfaz estándar de PR de GitHub.com.

> [!NOTE]
> Al explorar la PR, puede que veas un aviso que indica que el firewall ha bloqueado a GitHub Copilot. Es **normal**: Copilot tiene acceso limitado a recursos externos de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el firewall del agente en la nube de Copilot][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes.
3. Abre la solicitud de incorporación de cambios titulada, por ejemplo, **Add missing documentation** o algo similar.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la solicitud tendrá la marca **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos; puedes descansar o repasar lo que has aprendido hasta ahora.

4. Cuando la solicitud esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluye los nuevos comentarios TSDoc y otros elementos de documentación. Los cambios concretos pueden variar.

   Mientras revisas los cambios, busca comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios. Estas indicaciones proceden de los archivos de instrucciones personalizadas que revisaste al principio del ejercicio 2.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que avisa de que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprueba y ejecuta los flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud de incorporación de cambios. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. El proceso puede tardar unos minutos.

## Pide cambios a GitHub Copilot

La colaboración con Copilot en una solicitud de incorporación de cambios no es un proceso unidireccional. Puedes mencionar a Copilot en comentarios —igual que a otros miembros del equipo— en la PR o en comentarios insertados en el código. Copilot verá esos comentarios e iniciará otra sesión para resolverlos. Como los resultados son no deterministas, no podemos sugerir un mensaje específico. Puedes pedirle, por ejemplo:

- que añada encabezados de comentario al principio de cada archivo de código con una breve descripción de su función.
- que añada comentarios TSDoc a los archivos TypeScript y Astro.
- que cree un archivo README con una descripción de la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier usuario. Puedes usar una de las ideas anteriores o pedir otro cambio de documentación que quieras incluir en el código.
2. Selecciona **View Session** para observar cómo trabaja Copilot. Verás que inicia una sesión nueva para realizar las actualizaciones.
3. Puedes seleccionar **Back to pull request** para volver a la solicitud de incorporación de cambios.

   ![Vuelta a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine los cambios, verás una confirmación nueva en la solicitud.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que el resultado te satisfaga. Cuando estés conforme, convierte la PR de borrador en una PR lista para revisión y combínala en la rama principal.

![Convierte la PR en lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Volvamos a la PR que Copilot creó para resolver la incidencia sobre los juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Show related games on the game details page**.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador que avisa de que hay flujos de trabajo pendientes de aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

   ![Aprueba y ejecuta los flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud de incorporación de cambios. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. El proceso puede tardar unos minutos.
9. **Opcional:** puedes cambiar a esta rama en tu Codespace y probar manualmente la funcionalidad de juegos relacionados. Ve al Codespace, abre el terminal y ejecuta estos comandos (sustituye `<branch-name>` por el nombre de la rama que creó Copilot, por ejemplo, **copilot/fix-8**):

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot ha creado la funcionalidad de juegos relacionados. Como antes, puedes trabajar de forma iterativa con el agente en la nube de Copilot para solicitar cambios. Por ejemplo, puedes pedirle que modifique cuántos juegos relacionados se muestran o que añada encabezados y comentarios TSDoc (recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas). Como antes, puedes enviar estas solicitudes con un comentario nuevo en la pestaña **Conversation**; Copilot lo verá e iniciará otra sesión.

## Revisa las mejoras de accesibilidad

Por último, revisaremos las mejoras de accesibilidad implementadas con el agente personalizado de accesibilidad. La PR debería incluir el modo de contraste alto que asignaste en el ejercicio 3 y el modo claro que solicitaste desde el centro de control en el ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la solicitud de incorporación de cambios tendrá la marca **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación y presta atención a:
   - los componentes de interfaz para alternar entre modos.
   - el uso del almacenamiento local para conservar las preferencias del usuario.
   - los cambios de CSS o de estilo para los modos de contraste alto y claro.
   - los atributos de accesibilidad (etiquetas ARIA, navegación con el teclado, etc.).
   - el código JavaScript/TypeScript que gestiona el cambio de modo.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que avisa de que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprueba y ejecuta los flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud de incorporación de cambios. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. El proceso puede tardar unos minutos.
10. **Opcional:** puedes cambiar a esta rama en tu Codespace y probar manualmente las mejoras de accesibilidad. Ve al Codespace, abre el terminal y ejecuta los comandos siguientes (sustituye `<branch-name>` por el nombre de la rama que creó Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los modos de contraste alto y claro en el navegador para comprobar que funcionan según lo previsto y que la configuración se mantiene al volver a cargar la página.

Observa cómo el agente personalizado de accesibilidad ha guiado a Copilot para implementar estas mejoras siguiendo las prácticas recomendadas de accesibilidad. Si detectas problemas o posibles mejoras de accesibilidad, puedes mencionar a **@copilot** en un comentario y pedirle que haga cambios, como hiciste con las PR anteriores.

## Ejercicio opcional: sigue delegando

El agente en la nube es más útil cuando puedes asignarle incidencias reales del backlog y ocuparte de otras tareas. Para crear este hábito, registra más incidencias en tu repositorio y asígnaselas a Copilot. Por ejemplo:

- crear un formulario de interés para patrocinadores en la página de detalles del juego.
- añadir paginación a la página de la lista de juegos.
- añadir validación de entradas y gestión de errores a los ayudantes de acceso a datos.

## Resumen

Has completado el itinerario sobre el agente en la nube. En estas lecciones:

- **Inspeccionaste los archivos de instrucciones personalizadas del repositorio** para observar su efecto en los resultados del agente en la nube.
- **Asignaste incidencias al agente en la nube de Copilot** y observaste cómo configuraba su entorno, planificaba y trabajaba de forma asíncrona.
- **Creaste y usaste un agente personalizado** de accesibilidad para añadir los modos de contraste alto y claro.
- **Usaste la página de agentes de Copilot como centro de control** para supervisar y guiar la sesión de accesibilidad mientras estaba en curso.
- **Revisaste e iteraste sobre las solicitudes de incorporación de cambios del agente en la nube**, mencionaste a `@copilot` para solicitar cambios y aprobaste flujos de trabajo.

## Repaso y siguientes pasos

Has completado el itinerario del agente en la nube. Si quieres seguir explorando, los otros itinerarios complementan lo que has practicado:

- 🖥️ **[Itinerario de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración con MCP directamente desde el IDE.
- 💻 **[Itinerario de CLI](../../cli/)**: entrega desde el terminal dos cambios revisados con los modos Plan y Autopilot, las habilidades de agente, un agente de QA personalizado, Playwright MCP, Agent Merge y el puente opcional `/delegate` con el agente en la nube.

En tu propio repositorio, prueba estas ideas:

- Asigna al agente en la nube una incidencia de refactorización o cobertura de pruebas.
- Crea un agente personalizado para otro ámbito.
- Configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asigna incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de trabajo de configuración del agente en la nube de Copilot][cloud-agent-best-practices]
- [Configura el firewall del agente en la nube de Copilot][agent-firewall]

---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
