---
title: "Ejercicio 2 - Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Probablemente haya pocas organizaciones que no tengan problemas con la deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidades que llevan mucho tiempo en el backlog por falta de tiempo para implementarlas. El agente en la nube de GitHub Copilot puede realizar tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, crea una solicitud de incorporación de cambios (PR) en borrador para que la revise un desarrollador. Así puedes delegar tareas tediosas, acelerar el desarrollo y dedicarte a cuestiones de mayor alcance.

Explorarás estas posibilidades con el agente en la nube de Copilot:

- personalizar el entorno de generación de código.
- asegurarte de que las operaciones se realicen de forma segura.
- comprender la importancia de definir incidencias con un alcance claro.
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere resolver. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: comprobarás que falta por completo. Como primer paso, quieren añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la exploración de juegos. Quiere que la página de detalles de cada juego muestre otros títulos de la misma categoría para que los visitantes puedan seguir explorando. Por ahora no necesita una implementación refinada; solo algo que permita hacer pruebas de aceptación de la experiencia. Este trabajo es un bloqueo, aunque hay otras incidencias de mayor prioridad.

Este tipo de tareas puede quedar relegado con facilidad, por lo que es ideal asignarlo al agente en la nube de Copilot. El agente puede trabajar en ellas de forma asíncrona mientras tú te ocupas de otras tareas y, después, puedes revisar su trabajo y confirmar que todo está como esperabas.

## Presentación del agente en la nube de GitHub Copilot

El [agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a un desarrollador. También puedes iniciar este trabajo de distintas maneras, por ejemplo, [asignando una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Después de asignarla, Copilot crea una solicitud de incorporación de cambios en borrador para seguir el progreso, configura un entorno y empieza a trabajar. Puedes consultar la sesión de Copilot mientras sigue en curso o una vez terminada. Cuando la solución esté lista para que la revises, Copilot te mencionará en la solicitud de incorporación de cambios.

## La importancia de definir bien el alcance de las instrucciones

GitHub Copilot no hace magia: no puedes saltarte el trabajo de pensar. Incluso las operaciones que parecen sencillas pueden ser complejas al examinarlas en detalle, así que [define cuidadosamente el alcance de las tareas para el agente en la nube de Copilot][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. La llegada de la IA generativa no cambia los fundamentos del desarrollo de software.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has conocido las instrucciones personalizadas y cómo guían a Copilot. Si ya completaste el ejercicio práctico sobre instrucciones personalizadas, habrás visto `.github/copilot-instructions.md` en acción. Si no, este es un buen momento para echarle un vistazo. Antes de asignar trabajo al agente en la nube de Copilot, revisa los archivos de instrucciones que ya incluye este repositorio, sin modificarlos, para que luego puedas observar su efecto.

Abre estos archivos en la interfaz web de GitHub o en un Codespace que ya tengas en ejecución:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, en especial las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en el frontmatter `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente en la nube la incidencia `Code lacks documentation` en la sección siguiente, observa si la solicitud de incorporación de cambios incluye comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios. Estas indicaciones proceden de los archivos de instrucciones que ya has revisado. Volveremos a señalarlo al revisar las PR en un ejercicio posterior.

## Configura el entorno de desarrollo para el agente en la nube de Copilot

La creación de código suele requerir un entorno concreto y la ejecución de algunos scripts de configuración para garantizar que todo esté listo, sea quien sea el responsable. Lo mismo ocurre al asignar tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente en la nube usa [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo mediante un [flujo de trabajo de configuración especial][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece a trabajar. Así puede acceder a las herramientas y dependencias de desarrollo necesarias. Este flujo se ha configurado por adelantado para facilitar el taller y ayudar a aprender. Comprueba que Copilot tenga Node.js, las dependencias del proyecto, los navegadores necesarios para las pruebas de extremo a extremo y la base de datos SQLite migrada y precargada para la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Contiene un solo trabajo, `copilot-setup-steps`, que se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- El desencadenador `workflow_dispatch` permite ejecutar manualmente el flujo de trabajo desde la pestaña **Actions** del repositorio. Puedes comprobar que se ejecute correctamente sin esperar a que Copilot lo inicie.

## Añade documentación

Todo el mundo entiende la importancia de la documentación, pero muchos proyectos tienen información obsoleta o carecen de documentación. Este tipo de deuda técnica suele quedar sin resolver, lo que reduce la productividad y dificulta mantener el código o incorporar nuevos desarrolladores al equipo. Copilot destaca en la generación de documentación, así que esta incidencia es ideal para asignársela al agente en la nube. Trabajará en segundo plano para crear la documentación necesaria. En un ejercicio posterior revisarás su trabajo.

1. En una nueva pestaña del navegador, ve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue** para crear una incidencia en blanco.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

   ![Asignación de una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

   ![Detalles de la asignación a Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la solicitud de incorporación de cambios (PR) que se acaba de generar. Tendrá un título parecido a `[WIP]: Code lacks documentation`. Si aún no aparece, espera un momento y actualiza la ventana del navegador.
12. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> La lista de tareas de Copilot puede tardar varios minutos en aparecer en la PR. Copilot está preparando su entorno (ejecuta el flujo de trabajo anterior), analiza el proyecto y determina la mejor forma de abordar el problema.

13. Revisa la lista y las tareas que va a completar.
14. Desplázate por la cronología de la solicitud de incorporación de cambios. Deberías ver una actualización que indica que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

   ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la ventana para ver el indicador actualizado.

16. Observa que puedes recorrer la sesión en directo y ver cómo Copilot resuelve el problema: explora el código y comprende el estado del proyecto, se detiene a pensar y decidir el plan adecuado, y crea código.

El proceso puede tardar varios minutos. Uno de los objetivos principales del agente en la nube de Copilot es realizar tareas de forma asíncrona para que puedas centrarte en otras. Aprovecharemos esa función asignándole otra tarea y, mientras tanto, nos ocuparemos de desarrollar una funcionalidad para la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

Una de las ventajas del agente en la nube de GitHub Copilot es que permite dividir el trabajo: tú puedes centrarte en unas tareas mientras el agente se ocupa de otras. Aunque añadir una sección de juegos relacionados para el equipo de diseño no lleve mucho tiempo, puedes dedicar ese tiempo a otras tareas. ¡Asignemos el trabajo al agente en la nube de Copilot!

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue** para usar la plantilla en blanco.
5. Escribe `Show related games on the game details page` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

   ![Asignación de una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, verás un emoji 👀 en el primer comentario de la incidencia. Indica que Copilot ya está trabajando en ella.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en tu segunda solicitud. El agente en la nube de Copilot trabaja de forma similar a un ingeniero de software, por lo que no es necesario supervisarlo continuamente; puedes revisar su trabajo cuando termine. Ahora vamos a crear y usar agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado el [agente en la nube de GitHub Copilot][copilot-agents]. Puedes asignarle incidencias para que las resuelva de forma asíncrona. Copilot puede ayudarte con deuda técnica, nuevas funcionalidades o migraciones de código entre marcos de trabajo.

Has explorado estos conceptos:

- personalizar el entorno de generación de código.
- asegurarte de que las operaciones se realicen de forma segura.
- comprender la importancia de definir incidencias con un alcance claro.
- asignar incidencias a Copilot.

Mientras el agente en la nube trabaja en segundo plano, puedes crear y usar agentes personalizados. El [agente en la nube de Copilot también puede usar servidores MCP][cloud-agent-mcp] e instrucciones personalizadas, que ya hemos explorado en módulos anteriores.

## Recursos

- [Acerca del agente en la nube de Copilot][copilot-agents]
- [Asigna incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de trabajo de configuración del agente en la nube de Copilot][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
