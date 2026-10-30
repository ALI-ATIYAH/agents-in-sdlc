---
slug: es-es/real-world-development/app
title: "Aplicación GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La **[aplicación GitHub Copilot](https://docs.github.com/copilot/concepts/agents/github-copilot-app)** es una aplicación de escritorio basada en Copilot CLI que reúne el desarrollo dirigido por agentes en un espacio de trabajo específico. Añade sesiones de agente en paralelo, modos de sesión intercambiables, lienzos compartidos y gestión nativa de incidencias y solicitudes de incorporación de cambios de GitHub, incluido **Agent Merge**, que guía las solicitudes durante los cambios de base, los comentarios de revisión, las correcciones de integración continua (CI) y la combinación.

El taller sigue un único flujo de trabajo continuo en Tailspin Toys:

1. Prepararás el proyecto, instalarás la aplicación, conectarás el repositorio y explorarás el espacio de trabajo y el backlog inicial.
2. Harás un cambio concreto en las valoraciones con estrellas, lo revisarás en el navegador y combinarás manualmente tu primera solicitud de incorporación de cambios (PR).
3. Partirás de la incidencia de filtrado, definirás el enfoque en el modo **Plan**, lo implementarás con **Autopilot** y lo revisarás en el modo **Interactive**.
4. Actualizarás las instrucciones del repositorio y las aplicarás al trabajo de filtrado.
5. Personalizarás la habilidad existente `quality-checks` y la usarás para ejecutar las comprobaciones del proyecto.
6. Añadirás el servidor Playwright Model Context Protocol (MCP) y lo usarás para explorar el filtrado en un navegador.
7. Crearás un agente personalizado de control de calidad (QA) y lo usarás para revisar los requisitos, la cobertura y las pruebas de validación.
8. Revisarás todos los cambios del filtrado y usarás Agent Merge para combinar la segunda PR.
9. Comprobarás que los lienzos estén disponibles; después, usarás Database Explorer y crearás y probarás un lienzo de clasificación respaldado por el repositorio. Si alguna capacidad necesaria no está disponible, registrarás el bloqueo y usarás la alternativa de solo lectura o continuarás con el repaso.

Para centrarte en el taller, crearás dos PR: una para las valoraciones con estrellas y otra para el filtrado, que incluirá las actualizaciones de instrucciones y de la habilidad, el perfil de QA y las pruebas. Inicia cada una desde `main` actualizada. El flujo de filtrado y calidad comparte una sesión, un árbol de trabajo y una rama para que puedas aprovechar lo aprendido mientras exploras cada herramienta. El ejercicio final del lienzo permanece en su sesión para que puedas centrarte en crear y probar la superficie compartida, sin repetir el flujo de PR.

## Lecciones

| Lección | Tema | Descripción |
|--------|-------|-------------|
| [0. Requisitos previos][ex0] | Configuración | Instala Node.js y crea tu copia del proyecto Tailspin Toys |
| [1. Instala la aplicación Copilot][ex1] | Configuración | Instala la aplicación, conecta el proyecto y familiarízate con el espacio de trabajo |
| [2. Añade valoraciones con estrellas: una mejora rápida][ex2] | Primer cambio | Muestra las valoraciones existentes y el valor alternativo para datos nulos; después, combina la primera PR |
| [3. Modos de agente: Plan y Autopilot][ex3] | Modos de agente | Planifica la funcionalidad a partir de su incidencia, créala con Autopilot y revísala con Interactive |
| [4. Guía a Copilot con instrucciones personalizadas][ex4] | Contexto | Explora y actualiza las instrucciones y aplícalas al filtrado |
| [5. Personaliza y usa una habilidad quality-checks][ex5] | Comprobaciones repetibles | Explora la habilidad existente, cambia el formato del informe y ejecútala |
| [6. Valida la funcionalidad con Playwright MCP][ex6] | Observación en el navegador | Configura MCP desde Customize e inspecciona el comportamiento del filtrado |
| [7. Crea y usa un agente de QA][ex7] | Requisitos y cobertura | Crea y selecciona un perfil especializado y reúne las pruebas de validación finales |
| [8. Crea y combina la PR de la funcionalidad][ex8] | Revisión y combinación | Revisa el filtrado, las instrucciones, la habilidad, el perfil de QA y las pruebas; después, usa Agent Merge para la segunda PR |
| [9. Explora y crea lienzos][ex9] | Colaboración | Usa Database Explorer y, después, crea y prueba un lienzo de clasificación respaldado por el repositorio |
| [10. Repaso y siguientes pasos][ex10] | Resumen | Repasa el flujo de trabajo, los recursos creados y otros materiales |

## Requisitos previos

Antes de asistir a este taller, asegúrate de tener:

- [ ] Una cuenta de GitHub con un plan activo **Copilot Student, Pro, Pro+, Business o Enterprise**
- [ ] Un equipo con **macOS, Linux o Windows**
- [ ] [Git instalado][install-git] en el equipo

> [!TIP]
> ¿No tienes un plan de pago? Los estudiantes verificados pueden obtener GitHub Copilot gratis a través de [GitHub Education][callout-student-plan-education]. El plan **Copilot Student** incluye el agente, MCP, la revisión de código y Copilot CLI que usa este taller, por lo que puedes completar todos los itinerarios con él.

> [!NOTE]
> Como la aplicación Copilot se ejecuta en tu propio equipo y no en un Codespace, el [ejercicio de requisitos previos][ex0] te guiará para instalar Node.js y crear tu copia del proyecto antes de instalar la aplicación.

> [!NOTE]
> Si usas Copilot Business o Copilot Enterprise, el administrador debe mantener habilitada la directiva **GitHub Copilot app**. Es una directiva independiente de la de Copilot CLI.

## Empieza

**[Empieza por los requisitos previos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
