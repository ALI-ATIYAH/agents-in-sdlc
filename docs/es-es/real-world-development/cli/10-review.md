---
title: "Lección 10 - Repaso y siguientes pasos"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has usado GitHub Copilot CLI a lo largo de un flujo de trabajo continuo en Tailspin Toys. Has:

- preparado un Codespace, instalado Copilot CLI, explorado el proyecto y encontrado la incidencia de filtrado del backlog.
- añadido valoraciones con estrellas, revisado el resultado en un navegador reenviado y combinado manualmente tu primera solicitud de incorporación de cambios (PR).
- partido de la incidencia de filtrado, definido el enfoque en el modo Plan, implementado la funcionalidad con Autopilot y revisado el resultado en el modo Interactive.
- guiado al agente con instrucciones personalizadas, personalizado la habilidad existente `quality-checks` y la usado para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- añadido el servidor Playwright Model Context Protocol (MCP) y lo usado para explorar el filtrado en un navegador real.
- creado y seleccionado un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas obtenidas en el navegador.
- revisado todos los cambios del filtrado y autorizado Agent Merge para combinar su PR.
- explorado comandos de barra para gestionar el contexto, los modelos, el uso compartido y la delegación opcional a la nube.

## Lo que has entregado

El taller tiene dos hitos de PR, cada uno en su propia rama a partir de `main` actualizada:

1. **Valoraciones con estrellas**: muestra `starRating` y un estado explícito para las valoraciones sin asignar en las tarjetas de juegos.
2. **Filtrado y flujo de trabajo de calidad**: implementa el filtrado, actualiza las instrucciones y las aplica a la funcionalidad, personaliza el informe de `quality-checks`, crea un perfil de QA e incluye las pruebas correspondientes.

Desde la planificación del filtrado hasta la creación de su PR, has usado la misma conversación y rama. Hemos combinado ese trabajo en una sola PR para simplificar el taller.

## Distintos tipos de validación

Has comprobado el código de varias formas: con pruebas automatizadas, inspeccionándolo en el navegador y dejando que Copilot explorara el navegador mediante MCP. La habilidad `quality-checks` ejecutó pruebas unitarias, lint y comprobaciones de tipos, y generó un informe con el nuevo formato. QA combinó esos resultados con una revisión de los requisitos y la cobertura antes de la PR.

Las pruebas nuevas deben cubrir carencias reales; si QA no necesita añadir pruebas, también puede ser correcto. Revisa el código y las pruebas antes de autorizar la combinación y vuelve a comprobar los resultados afectados después de hacer cambios.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller has actualizado instrucciones, personalizado una habilidad, creado un perfil de QA y configurado un servidor MCP. Reutiliza estas personalizaciones en distintas conversaciones y ajústalas a medida que cambien las necesidades del equipo. Las instrucciones definen estándares, las habilidades describen tareas repetibles, los agentes personalizados establecen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados de las herramientas, no solo el resumen del agente.

Adapta el **modo y el modelo** a la tarea. Usa **Plan** para definir un enfoque antes de implementar, **Interactive** para mantener el control de cambios concretos y **Autopilot** para tareas bien delimitadas. Elige un modelo más rápido para ediciones rutinarias y otro más capaz para tareas complejas.

El contexto sigue siendo tan importante como la infraestructura. Explicar claramente *qué* quieres desarrollar, *por qué* y *cómo* puede cambiar sustancialmente el resultado.

## Más cosas por explorar

Ya has completado el flujo de trabajo principal. Estas son algunas funciones más de la CLI que puedes probar:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para explicar un problema y obtener otra perspectiva.
- `/fleet` para coordinar tareas independientes en paralelo.
- `/worktree` para aislar una tarea distinta.
- `/delegate` para enviar una tarea al agente en la nube de Copilot.

## Siguientes pasos

La mejor forma de mejorar con cualquier herramienta es seguir usándola. Úsala con código de producción, en proyectos personales o para crear esa pequeña aplicación que llevas años queriendo hacer. Comparte lo aprendido con el equipo y aprende de sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Backer Concierge de Foundry][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente en la nube de Copilot][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personaliza Copilot CLI][customize-cli]
- [Gestiona solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
