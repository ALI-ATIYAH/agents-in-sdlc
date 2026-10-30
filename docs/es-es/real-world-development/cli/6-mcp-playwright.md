---
title: "Lección 6 - Valida la funcionalidad con Playwright MCP"
description: "Inspecciona o configura Playwright MCP en Copilot CLI y úsalo para explorar la funcionalidad de filtrado en un navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como ya hemos destacado, programar no consiste únicamente en escribir código. También necesitamos trabajar con datos y servicios externos, e incluso poner automatizaciones adicionales a disposición de Copilot. Para ello sirven los servidores MCP: permiten que Copilot vaya más allá de las funciones integradas en la CLI y acceda a más herramientas y servicios.

En esta lección:

- entenderás qué es el Model Context Protocol (MCP) y cómo lo usa Copilot CLI.
- añadirás el servidor Playwright MCP si aún no está disponible.
- pedirás al agente que controle un navegador y explore la funcionalidad de filtrado.

## Escenario

Las pruebas unitarias y de extremo a extremo son importantes, pero para validar cambios en la interfaz de usuario hay que interactuar con ella. Quieres que Copilot pueda usar el sitio web como lo haría una persona y automatizar así la validación de los cambios, lo que aporta más confianza en que funcionan según lo previsto.

## ¿Qué es el Model Context Protocol (MCP)?

[Model Context Protocol (MCP)][mcp-blog-post] ofrece a los agentes de IA una forma de comunicarse con herramientas y servicios externos. MCP permite que los agentes de IA accedan a información actualizada y realicen acciones en tu nombre.

Los agentes acceden a estas herramientas y recursos a través de un servidor MCP, que actúa como puente entre el agente de IA y las herramientas y servicios externos. Cada servidor MCP representa un conjunto distinto de herramientas y recursos a los que puede acceder el agente.

Algunos servidores MCP populares son:

- **[GitHub MCP Server][github-mcp]**: proporciona acceso a las API para gestionar repositorios, incidencias y solicitudes de incorporación de cambios de GitHub.
- **[Playwright MCP Server][playwright-mcp-server]**: ofrece funciones de automatización del navegador con Playwright.

Hay muchos otros servidores MCP disponibles. GitHub aloja un [registro de MCP][mcp-registry] para facilitar el descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, comprueba quién lo publica y ten en cuenta las implicaciones de seguridad.

## Añade el servidor Playwright MCP

Si el servidor Playwright MCP aún no está disponible, añádelo para que Copilot pueda interactuar con el sitio web como lo haría una persona.

> [!NOTE]
> Si Playwright MCP ya está configurado en Copilot CLI, omite estos pasos y continúa con la sección siguiente.

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo para añadir un servidor MCP con este comando en Copilot CLI:

   ```plaintext
   /mcp add
   ```

3. Como nombre, escribe `playwright` y pulsa <kbd>Tab</kbd>.
4. Comprueba que el tipo de servidor sea **STDIO**; para ello, pulsa <kbd>Enter</kbd> y luego <kbd>Tab</kbd>.
5. Pega lo siguiente en el cuadro de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el servidor MCP.
7. Pulsa <kbd>Esc</kbd> para salir del cuadro de diálogo de MCP.

## Pide a Copilot que explore la funcionalidad con Playwright

Antes comprobaste manualmente que la funcionalidad se comporta como esperas. Ahora usaremos el servidor Playwright MCP disponible en Copilot para hacer lo mismo.

1. Usa el siguiente mensaje para pedir a Copilot que valide la funcionalidad con el servidor Playwright MCP:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> No es necesario pedir expresamente a Copilot que use el servidor MCP, ya que normalmente lo detectará por sí mismo. Sin embargo, indicarle qué debe usar puede ayudar a obtener resultados más coherentes y ahorrar algunos tokens.

2. Observa los pasos que Copilot realiza en el navegador para comprobar que la funcionalidad funciona.
3. Lee el informe y asegúrate de que todo se comporta según lo previsto.

Copilot iniciará el servidor, usará Playwright para interactuar con el sitio web, detendrá el servidor y te entregará un informe.

## Resumen y siguientes pasos

¡Enhorabuena! Has usado el servidor Playwright MCP para explorar la funcionalidad en un navegador real desde Copilot CLI. En resumen:

- aprendiste qué es Model Context Protocol (MCP) y cómo lo usa Copilot CLI.
- añadiste el servidor Playwright MCP si aún no estaba disponible.
- pediste al agente que controlara un navegador y explorara la funcionalidad.

A continuación, [crearás un agente personalizado de QA][next-lesson] que combinará la habilidad y las herramientas del navegador en un rol especializado.

## Recursos

- [¿Qué es MCP y por qué habla todo el mundo de ello?][mcp-blog-post]
- [Servidor Playwright MCP de Microsoft][playwright-mcp-server]
- [Añade servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
