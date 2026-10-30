---
title: "Ejercicio 2 - Añade funcionalidad con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir modificar varios archivos y realizar operaciones como ejecutar pruebas. El flujo de trabajo habitual de un desarrollador consiste en buscar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta, volver a modificarlo… y así sucesivamente.

Para eso sirve el modo de agente de Copilot.

El modo de agente de Copilot está diseñado para trabajar de forma más autónoma en el IDE. Se comporta de forma parecida a un desarrollador: primero explora la estructura del proyecto, realiza las actualizaciones necesarias, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Veamos cómo usar el modo de agente para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo de agente y el agente en la nube ofrecen experiencias distintas. El modo de agente trabaja en el IDE para facilitar ciclos de respuesta e interacción rápidos. El agente en la nube está diseñado para actuar como compañero de equipo y trabajar de forma asíncrona con incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás:

- cómo el modo de agente de Copilot explora el proyecto, identifica los archivos pertinentes y coordina los cambios.
- cómo el modo de agente de GitHub Copilot implementa funcionalidades en la interfaz y la capa de datos.
- cómo revisar los cambios y las pruebas generados por el modo de agente de Copilot antes de combinarlos en el código.

## Escenario

A medida que aumenta la lista de juegos, quieres que los usuarios puedan filtrarlos por categoría y editorial. En el ejercicio anterior añadiste un ayudante de editoriales. Ahora completarás el resto del trabajo de la capa de datos, la interfaz de usuario y las pruebas con el modo de agente de Copilot.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, exploremos el sitio web de Tailspin Toys para conocer sus funciones actuales.

El sitio web es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar juegos y consultar sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local desde su frontmatter mediante Drizzle ORM; no hay una API de backend independiente ni un marco de trabajo de interfaz del lado del cliente. Los ayudantes reutilizables para acceder a los datos están en `src/lib/`, y la interactividad se implementa con pequeños elementos `<script>` de Astro que usan las API DOM estándar.

### Inicia el sitio web

Para facilitar la ejecución del sitio, se ha preparado la tarea `npm run dev`, que inicia el único servidor de desarrollo de Astro. Puedes ejecutarlo en tu Codespace de GitHub siguiendo estos pasos:

1. Vuelve a tu Codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal en el Codespace con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Actualiza `main` y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta este comando para iniciar el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, aparecerá un mensaje con la URL, similar al siguiente:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir un navegador en `http://localhost:4321`, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando en el terminal la dirección del servidor Astro `http://localhost:4321`.

> [!NOTE]
> En un Codespace, al seleccionar el enlace localhost en el terminal se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Se trata de un túnel privado a tu Codespace, que ahora aloja el servidor web.

### Explora el sitio web

Con el sitio en ejecución, explora sus funciones principales:

- **Página principal**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, accedes a una página con más información, como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu Codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían en cada ejercicio; los pasos correspondientes lo indican cuando es necesario.

La implementación inicial del sitio funciona, pero queremos añadir más funciones. Empecemos por revisar el backlog. Al crear el proyecto a partir de la plantilla, se generó automáticamente un backlog de incidencias. Pide a GitHub Copilot que te muestre esas incidencias.

1. Selecciona **Agent** en el selector de agentes de la vista Chat. El agente planifica e implementa cambios de forma autónoma en varios archivos, ejecuta comandos de terminal y usa herramientas.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no prefieren un modelo sobre otro. Al crear el taller usamos Claude Sonnet 4.5 y por eso lo mencionamos en las instrucciones. Esperamos que las sugerencias de código sean relativamente coherentes y ofrezcan una buena experiencia. Sin embargo, como los modelos de lenguaje grandes (LLM) son probabilísticos, es posible que las sugerencias difieran de los ejemplos del taller. Es normal y esperable.

> [!NOTE]
> Debido al carácter probabilístico de los LLM, Copilot puede usar un comando MCP diferente, pero debería poder completar la tarea.

3. Pide a Copilot que te muestre el backlog de incidencias con este mensaje:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado las incidencias según lo útiles que podrían ser para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere el código, revisa el archivo de instrucciones que dará contexto a Copilot. Usarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidad al sitio web.

1. En tu Codespace, ve a `.github/instructions/ui.instructions.md`.
2. Observa las indicaciones generales para añadir funcionalidad, entre ellas:
   - una descripción general de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a archivos de instrucciones específicos para distintos tipos de archivo, como:
     - Astro.
     - Tailwind CSS.
     - Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden incluir referencias a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Así se pueden reutilizar las instrucciones, dividir conjuntos complejos en partes más manejables e incluir ejemplos y plantillas.

## Implementa el filtrado

Para completar el filtrado, tendrás que hacer al menos tres cambios en la aplicación:

- añadir o ajustar la lógica de filtrado restante en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento del filtrado.
- actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de combinar los cambios. El modo de agente de Copilot puede encargarse de estas tareas. Añadamos la funcionalidad.

1. Puedes continuar en la conversación actual con Copilot o iniciar otra seleccionando **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de cómo seleccionar el archivo de instrucciones de interfaz](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el selector de agentes de la vista Chat.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad a partir de la incidencia relacionada del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos asociados a la funcionalidad, tanto los ayudantes de la capa de datos y la interfaz como las pruebas. Después, modifica los archivos y ejecuta las pruebas.

   ![Captura de Copilot explorando archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas: explorar el proyecto, modificar archivos y ejecutar pruebas. Según la complejidad de la tarea y del código, puede tardar unos minutos. Durante el proceso quizá veas los botones **Keep** y **Undo** en el editor. Cuando Copilot termine, aparecerá uno de esos botones para cada cambio; no hace falta que los selecciones mientras el agente sigue trabajando.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Captura del panel de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que Copilot haga pausas y que algunas pruebas fallen durante el proceso. No pasa nada. Alternará entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Captura de una sesión completa con el modo de agente de Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Revisa el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código que genera Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio en ejecución y explora la nueva funcionalidad.
11. Cuando hayas comprobado que funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio has aprendido a usar el modo de agente de GitHub Copilot para añadir funciones al sitio web de Tailspin Toys. Hemos visto cómo:

- el modo de agente de GitHub Copilot puede implementar funciones en la interfaz y la capa de datos.
- el modo de agente de Copilot puede explorar el proyecto, identificar los archivos pertinentes y coordinar los cambios.
- revisar los cambios y las pruebas generados por el modo de agente de Copilot antes de combinarlos en el código.

Ahora [probaremos la funcionalidad con el servidor Playwright MCP][next-lesson] y abriremos una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementa la paginación

A medida que aumente la lista de juegos, será necesario añadir paginación. Aplica lo aprendido en este ejercicio y pide a Copilot que la implemente en el sitio. Ten en cuenta lo siguiente:

- sigue las prácticas recomendadas y usa los archivos de instrucciones existentes.
- decide cómo quieres implementar la paginación: por ejemplo, si el usuario podrá elegir el tamaño de página o si será fijo.
- al redactar el mensaje, proporciona a Copilot las indicaciones necesarias para que implemente lo que quieres.
- puede que tengas que iterar con GitHub Copilot, solicitar cambios y aportar contexto. ¡Es normal trabajar así con Copilot!

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo se usan][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
