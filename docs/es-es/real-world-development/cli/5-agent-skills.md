---
title: "Lección 5 - Personaliza y usa la habilidad quality-checks"
description: "Explora la habilidad quality-checks existente, personaliza el formato de su informe y úsala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar no consiste únicamente en escribir código. Ya hemos comprobado manualmente que funciona y hemos usado archivos de instrucciones para garantizar que sigue nuestros estándares. Pero ¿qué pasa con las pruebas, lint y los demás componentes de la integración continua (CI)?

Las **habilidades de agente** son la herramienta adecuada para este tipo de tareas. Ayudan a Copilot a entender cómo realizar correctamente operaciones como estas.

En esta lección:

- explorarás la habilidad existente `quality-checks`.
- personalizarás el formato de sus resultados.
- volverás a cargar la habilidad y la ejecutarás.

## Escenario

Tailspin Toys usa la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente reúnen instrucciones reutilizables para tareas, scripts ejecutables y recursos auxiliares que el agente carga cuando los necesita. En esencia, son carpetas con el nombre de la habilidad y un archivo Markdown llamado `SKILL.md`. El Markdown incluye frontmatter con un nombre y una descripción que indican el propósito de la habilidad, además de información general sobre sus funciones y pautas sobre cuándo usarla. La carpeta también puede incluir subcarpetas con scripts y otros recursos que la habilidad utilice.

> [!NOTE]
> Las carpetas y los archivos adicionales son opcionales. La habilidad `quality-checks` de Tailspin Toys solo contiene `SKILL.md` porque usa los comandos existentes del proyecto.

Las habilidades pueden estar en la carpeta `.github/skills` del proyecto para que el equipo pueda compartirlas y reutilizarlas, o en la carpeta de habilidades del usuario, `~/.copilot/skills`.

## Explora la habilidad

Exploremos la habilidad `quality-checks` que creó el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos.

1. Vuelve a tu Codespace y abre `.github/skills/quality-checks/SKILL.md` en el editor de Codespaces.
2. Lee el `name` y la `description` de la parte superior. La descripción ayuda a Copilot a saber cuándo debe usar la habilidad.
3. Lee las instrucciones y observa cómo guían a Copilot durante las pruebas y el proceso de lint.
4. Observa que la habilidad todavía no incluye una sección **Results output formatting**.

## Ejecuta la habilidad antes de cambiarla

Las habilidades se pueden invocar directamente desde Copilot CLI o mediante lenguaje natural. Pidamos a Copilot que ejecute las tres comprobaciones de la habilidad.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Envía este mensaje:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Lee el informe final.

## Personaliza el informe

Queremos mejorar el informe para que indique qué se ha ejecutado, si se ha superado y qué han comunicado realmente las herramientas. Actualicemos la habilidad para generar ese informe.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade esta sección al final del archivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. El archivo se guarda automáticamente.

## Ejecuta la habilidad actualizada

Ahora que hemos hecho el cambio, comprobemos cómo funciona. Copilot CLI puede volver a cargar las habilidades editadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Envía exactamente el mismo mensaje:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Lee el informe y compáralo con el primero.

## Resumen y siguientes pasos

Has personalizado y usado una habilidad de agente existente. En esta lección:

- exploraste la habilidad `quality-checks` para pruebas unitarias, lint y comprobaciones de tipos.
- personalizaste el formato de sus resultados.
- volviste a cargar y ejecutaste la habilidad.

Este cambio acompañará al filtrado en la PR de la funcionalidad. A continuación, permitirás que Copilot interactúe directamente con el sitio [mediante el servidor Playwright MCP][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad sirven de referencia; no son tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y un script de exportación integrado: `drawio`][drawio-example]
- [Pruebas del navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
