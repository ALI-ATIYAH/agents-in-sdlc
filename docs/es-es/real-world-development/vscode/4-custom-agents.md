---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para tareas o ámbitos concretos del flujo de trabajo de desarrollo. Al definir los agentes en archivos Markdown en la carpeta `.github/agents` del repositorio, puedes proporcionar a Copilot instrucciones específicas, prácticas recomendadas, patrones de programación y conocimientos del dominio para ayudarle a realizar determinados tipos de trabajo de forma más eficaz. Los equipos pueden convertir sus conocimientos en agentes reutilizables: por ejemplo, un agente de accesibilidad que aplique las directrices de [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones de pruebas coherentes.

Los agentes personalizados se definen en archivos Markdown de la carpeta `.github/agents` del proyecto o, globalmente, en `~/.copilot/agents`. Cada archivo tiene un frontmatter YAML con, al menos, los campos `name` y `description`, seguido de un mensaje en Markdown que define el comportamiento, los conocimientos y las instrucciones del agente.

### Agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] tienen algunos aspectos en común: ambos se definen principalmente mediante archivos Markdown e indican a una IA cómo realizar ciertas operaciones. La forma más sencilla de distinguirlos es que un **agente personalizado** es el trabajador y las **habilidades** son sus herramientas.

Los agentes personalizados tienen su propia ventana de contexto y están diseñados para coordinar habilidades e incluso otros agentes como parte de su trabajo. En este taller, el agente personalizado de accesibilidad revisará y actualizará el sitio siguiendo las directrices de accesibilidad. Para ello, podría recurrir a habilidades como una que aplique un flujo de trabajo de solicitudes de incorporación de cambios o que ejecute y gestione pruebas.

> [!NOTE]
> No hay una única forma «correcta» de crear un agente personalizado. Como ocurre con cualquier uso de la IA, prueba distintas opciones e itera para encontrar la que mejor se adapte a tu entorno y tus escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys se compromete a que todas las personas puedan usar su plataforma de financiación colectiva, independientemente de sus capacidades visuales o preferencias. Los comentarios recientes de los usuarios indican que algunas personas tienen dificultades para leer el tema oscuro actual debido al bajo contraste entre el texto y los colores de fondo. Para abordar este problema de accesibilidad, el equipo de diseño ha solicitado un modo de contraste alto que se pueda activar y desactivar.

La accesibilidad es importante, así que quieres implementar esta funcionalidad lo antes posible. Usarás un agente personalizado para generar la funcionalidad.

En este ejercicio:

- revisarás un agente personalizado de accesibilidad existente.
- usarás el agente de accesibilidad en Copilot Chat para implementar un modo de contraste alto.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revisaremos su contenido para entender cómo guiará a Copilot.

Vuelve a tu Codespace y abre un terminal. Crea una rama nueva a partir de `main` para el trabajo de accesibilidad (así mantendrás separada la PR de filtrado del ejercicio 3):

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Observa los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> Los campos `name` y `description` son obligatorios en el frontmatter de los agentes personalizados.

3. Lee las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio web accesible.
   - las prácticas recomendadas de accesibilidad.
   - ejemplos de código HTML, CSS y JavaScript.
   - errores y problemas habituales.

## Usa el agente personalizado en Copilot Chat

VS Code muestra todos los agentes personalizados definidos en `.github/agents` en el selector de agentes situado en la parte inferior de la vista Copilot Chat. Puedes seleccionar un agente personalizado para limitar la sesión de chat a sus instrucciones y herramientas.

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu Codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían en cada ejercicio; los pasos correspondientes lo indican cuando es necesario.

1. Selecciona **Agent** en el selector de agentes de la vista Chat, si aún no está seleccionado.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Abre el selector de agentes situado en la parte inferior de la vista de chat. Muestra el agente activo; de forma predeterminada, es **default**.
3. Selecciona **Accessibility agent** en la lista de agentes disponibles.
4. Envía este mensaje al agente de accesibilidad:

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat empezará a trabajar: explorará el código, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en el chat con la ruta del archivo y las diferencias para que puedas revisarla.
6. A medida que se apliquen las ediciones, se actualizará el indicador **Files changed** para mostrar los archivos que ha modificado el agente.

> [!NOTE]
> Este proceso puede tardar varios minutos. Copilot está haciendo cambios reales en el repositorio: editará archivos existentes, como componentes Astro, CSS y las pruebas relacionadas.

Revisarás y guiarás el trabajo en curso en el siguiente ejercicio.

## Resumen y siguientes pasos

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes de IA especializados para tareas y ámbitos concretos. Los agentes personalizados te permiten convertir los conocimientos y estándares de tu equipo en agentes reutilizables que orientan el trabajo de Copilot.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados.
- cómo se usa un agente personalizado en el modo de agente de Copilot Chat.

A continuación, [supervisarás y guiarás el trabajo del agente][next-lesson]: revisarás los cambios a medida que se realizan y añadirás un selector de modo claro a la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crea agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados de awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Prueba la funcionalidad con el servidor Playwright MCP][previous-lesson] | [Siguiente lección: Supervisa y gestiona los agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
