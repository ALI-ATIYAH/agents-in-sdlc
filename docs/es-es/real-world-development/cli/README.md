---
slug: es-es/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Desarrolla, valida y entrega dos cambios en Tailspin Toys mientras aprendes los modos, las personalizaciones, las herramientas MCP y la automatización de solicitudes de incorporación de cambios de Copilot CLI."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** incorpora GitHub Copilot a tu terminal como asistente de programación con agentes. Explora bases de código, genera código, ejecuta comandos y se conecta a herramientas externas, todo desde la línea de comandos, para que puedas mantener el flujo sin cambiar a un editor gráfico.

El taller sigue un único flujo de trabajo continuo en Tailspin Toys:

- Prepararás el proyecto en GitHub Codespaces, instalarás Copilot CLI y te familiarizarás con la herramienta.
- Harás un cambio concreto en las valoraciones con estrellas, lo revisarás en el navegador y combinarás manualmente tu primera solicitud de incorporación de cambios (PR).
- Partirás de la incidencia de filtrado, definirás el enfoque en el modo Plan, lo implementarás con Autopilot y lo revisarás en el modo Interactive.
- Actualizarás las instrucciones del repositorio y las aplicarás al trabajo de filtrado.
- Personalizarás la habilidad existente `quality-checks` y la usarás para ejecutar las comprobaciones del proyecto.
- Añadirás el servidor Playwright Model Context Protocol (MCP) y lo usarás para explorar el filtrado en un navegador.
- Crearás un agente personalizado de control de calidad (QA) para revisar los requisitos, la cobertura y las pruebas de validación.
- Revisarás todos los cambios del filtrado y usarás Agent Merge para combinar su PR.
- Explorarás comandos de barra útiles para el contexto, los modelos, el uso compartido y la delegación opcional a la nube.

Para centrarte en el taller, crearás dos PR: una para las valoraciones con estrellas y otra para el filtrado, que incluirá las actualizaciones de instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas. El trabajo de filtrado y calidad comparte una conversación y una rama para que puedas aprovechar lo aprendido mientras exploras cada herramienta.

## Lecciones

| Lección | Tema | Descripción |
| ------ | ----- | ----------- |
| [0. Requisitos previos][ex0] | Configuración | Crea tu repositorio y Codespace |
| [1. Instala Copilot CLI][ex1] | Instalación | Instala y autentica Copilot CLI, y familiarízate con la herramienta |
| [2. Añade valoraciones con estrellas: una mejora rápida][ex2] | Primer cambio | Muestra las valoraciones existentes y el valor alternativo para datos nulos; después, combina tu primera PR |
| [3. Modos de agente: Plan y Autopilot][ex3] | Modos de agente | Planifica la funcionalidad a partir de su incidencia, créala con Autopilot y revísala con Interactive |
| [4. Guía a Copilot con instrucciones personalizadas][ex4] | Contexto | Explora y actualiza las instrucciones y aplícalas al filtrado |
| [5. Personaliza y usa la habilidad quality-checks][ex5] | Comprobaciones repetibles | Explora la habilidad existente, cambia el formato del informe y ejecútala |
| [6. Valida la funcionalidad con Playwright MCP][ex6] | Observación en el navegador | Configura MCP en la CLI e inspecciona el comportamiento del filtrado |
| [7. Crea y usa un agente de QA][ex7] | Requisitos y cobertura | Crea y selecciona un perfil especializado y reúne las pruebas de validación finales |
| [8. Crea y combina la PR de la funcionalidad][ex8] | Revisión y combinación | Revisa todos los cambios, crea la PR y usa Agent Merge |
| [9. Comandos de barra de GitHub Copilot CLI][ex9] | Funciones de la CLI | Explora el contexto, los modelos, el uso compartido y la delegación opcional al agente en la nube |
| [10. Repaso y siguientes pasos][ex10] | Resumen | Repasa el flujo de trabajo, las personalizaciones reutilizables y otros recursos |
| [Opcional: incorpora Foundry][foundry] | Agentes hospedados | Prepara un modelo, implementa un agente basado en el catálogo y conéctalo al sitio web |

## Requisitos previos

Antes de asistir a este taller, asegúrate de tener:

- [ ] Una cuenta de GitHub con un plan activo de **Copilot Student, Pro, Pro+, Business o Enterprise**
- [ ] Permiso para crear un repositorio y un Codespace
- [ ] Conocimientos básicos de operaciones de terminal/línea de comandos
- [ ] Git instalado y configurado

> [!TIP]
> ¿No tienes un plan de pago? Los estudiantes verificados pueden obtener GitHub Copilot gratis a través de [GitHub Education][student-plan]. El plan **Copilot Student** incluye las funciones de agente, MCP, revisión de código y Copilot CLI que utiliza este taller.

> [!NOTE]
> Si usas Copilot Business o Copilot Enterprise, comprueba que el administrador haya habilitado Copilot CLI.

## Empieza

**[Empieza por los requisitos previos →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
