---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Explora los modos de agente: usa Plan para acordar un enfoque, Autopilot para crear filtros a partir de una incidencia e Interactive para revisar y comprobar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Empezamos añadiendo una pequeña funcionalidad al proyecto. Sin embargo, los cambios más amplios requieren un proceso más sólido. Por suerte, la aplicación GitHub Copilot está diseñada para adaptarse al flujo existente de una organización y garantizar que se desarrolle lo adecuado de la forma correcta. Esta es la primera de varias lecciones en las que seguirás un proceso de desarrollo habitual dirigido por agentes: partirás de una incidencia para crear una funcionalidad, comprobarás que el código es válido y que la funcionalidad se comporta como se espera y, finalmente, la combinarás correctamente en el proyecto.

> [!NOTE]
> Utilizarás la misma sesión durante todo el flujo de trabajo de la funcionalidad. Normalmente usarías sesiones o PR distintas para los diferentes tipos de archivo, pero tomaremos un atajo para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una sesión de agente desde una incidencia de GitHub.
- definirás los requisitos en el modo **Plan**.
- implementarás la funcionalidad con **Autopilot**.
- revisarás el código.
- validarás la funcionalidad manualmente en un lienzo del navegador.

Mientras continúas con esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad existente `quality-checks`, añadirás validación MCP, crearás un agente de QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys está creciendo y sus visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero hay que acordar detalles como la combinación de categorías antes de programar. Utilizarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Introducir agentes de programación con IA en el flujo de desarrollo no cambia los principios fundamentales. De hecho, adquieren aún más importancia. La mayoría de los desarrolladores siguen un flujo similar al siguiente:

1. Abrir una incidencia registrada con los detalles del trabajo.
2. Crear un plan de lo que debe desarrollarse.
3. Desarrollar y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Combinar el código cuando se haya revisado y se hayan superado las comprobaciones de integración continua.

> [!NOTE]
> Los detalles concretos variarán según el equipo y la organización, pero la mayoría de los procesos serán una variante del flujo anterior.

Si sigues este enfoque estándar, te aseguras de que el código generado por IA cumpla los requisitos establecidos y pase por el mismo proceso de validación que el código escrito manualmente.

## Modos de sesión

El **modo de sesión** controla el grado de autonomía del agente. Puedes elegirlo en el menú desplegable situado debajo del campo de mensaje y cambiarlo en cualquier momento:

- **Interactive**: trabajas junto con el agente. Este propone cambios y espera tus indicaciones antes de continuar.
- **Plan**: el agente crea primero un plan. Revisas y apruebas el plan antes de que el agente lo ejecute.
- **Autopilot**: el agente trabaja de forma totalmente autónoma: escribe código, ejecuta pruebas e itera sin esperar indicaciones.

Empieza en el modo Plan, revisa el plan y, después, utiliza Autopilot para implementarlo.

## Inicia una sesión desde la incidencia

Antes de empezar, confirma que se ha combinado la PR de las valoraciones con estrellas y que la rama `main` local está actualizada.

1. En **Sessions**, en la barra lateral, selecciona la carpeta del repositorio Tailspin Toys. Después, selecciona **Issues** en la navegación superior del repositorio y abre **Allow users to filter games by category and publisher**. Esta incidencia ya se ha creado a partir de la plantilla; no tienes que crear otra.
2. Selecciona **New session** y elige un **new working tree** basado en `main` actualizada.

   ![Vista de una incidencia en la aplicación GitHub Copilot con una flecha que señala el botón New session](../../../_images/app-new-session-from-issue.png)

3. Confirma que la incidencia está adjunta a la sesión y selecciona **Plan** en el selector de modo.

> [!NOTE]
> Si no se encuentra Node.js o npm, pide al agente que ejecute `node --version` y `npm --version`. Si funcionan en tu terminal, pero no en la sesión, comprueba la configuración de `PATH` o del gestor de versiones de la aplicación y ciérrala por completo antes de volver a abrirla. Reintenta la operación y las comprobaciones fallidas; no vuelvas a instalar npm solo porque el agente no lo encuentre.

## Planifica la funcionalidad de filtrado

Planificar te permite revisar el enfoque antes de que Copilot escriba código. Como has iniciado la sesión desde la incidencia, Copilot ya tiene la solicitud como contexto. Envía:

```plaintext
Build this feature.
```

Responde a las preguntas de Copilot y compara el plan con los criterios de aceptación de la incidencia. Comprueba que incluye el filtrado por categoría y editorial, controles accesibles, cambios en el acceso a datos y pruebas. Aclara cualquier comportamiento poco definido, como la combinación de varias categorías o qué ocurre cuando ningún juego coincide.

El plan debe incluir lint, pruebas unitarias, pruebas E2E y comprobación de tipos con las herramientas existentes del proyecto. Céntralo en implementar y probar el filtrado; crearás la PR después de completar el flujo de calidad. Solicita cambios en el plan antes de aprobarlo y conserva la URL de la incidencia y las aclaraciones acordadas para la validación posterior.

## Aprueba Autopilot explícitamente

Cuando estés conforme con el plan, selecciona **Approve and implement this plan** o la opción equivalente de tu versión. Comprueba cuál es el modo activo en lugar de dar por hecho que la aprobación cambia de modo. Confirma que el indicador muestre **Autopilot**.

Copilot comenzará a implementar la funcionalidad. Verás cómo itera por el proceso, sigue el plan acordado, genera código y ejecuta pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación de inmediato, así que revisa primero el plan. Si Copilot informa de dependencias ausentes o de un conflicto de puertos, resuelve el problema de configuración antes de dar las comprobaciones por completadas. Detén solo los servidores que hayas iniciado.

## Revisa y comprueba la implementación

Una vez generado el código, hay que revisarlo antes de combinarlo, igual que cualquier otro código. Revisemos los cambios y ejecutemos el sitio para comprobar que todo funciona correctamente.

1. Abre **Changes** e inspecciona la implementación del filtrado y las pruebas.
2. Compara el resultado con la incidencia y las aclaraciones aprobadas, incluidas las combinaciones de varias categorías y editoriales. Comprueba que los cambios siguen las instrucciones existentes del repositorio.
3. Revisa los resultados de lint, las pruebas unitarias, las pruebas E2E y la comprobación de tipos. Una comprobación omitida no cuenta como superada.
4. Resuelve los errores y vuelve a ejecutar las comprobaciones afectadas antes de aceptar la implementación. La configuración E2E de Playwright compila y sirve una vista previa, y puede reutilizar un servidor local. Asegúrate de que el servidor probado pertenece a este árbol de trabajo, no a una lección anterior.

## Prueba la nueva funcionalidad

El código parece correcto, pero ¿se ejecuta? Iniciemos la aplicación como antes y abramos el sitio en un lienzo del navegador.

1. Usa este mensaje para pedir a Copilot que inicie la aplicación y abra la página en el lienzo del navegador:

   ```plaintext
   Start the app and open it in the browser canvas.
   ```

2. En unos instantes, la aplicación se iniciará y se abrirá una ventana del navegador dentro de la aplicación Copilot.
3. Selecciona una categoría y confirma que los resultados coinciden. Borra el filtro, selecciona una editorial y confirma que los resultados coinciden con ella.
4. Combina los filtros de categoría y editorial y compara los resultados con la incidencia y el plan aprobado. Si se admiten varias categorías, comprueba el comportamiento acordado para combinarlas.
5. Prueba una combinación sin juegos coincidentes y revisa el estado vacío. Borra o restablece los filtros y confirma que vuelve a aparecer la lista completa.
6. Cuando termines, pide a Copilot que detenga el servidor de desarrollo que ha iniciado para esta sesión:

   ```plaintext
   Stop the dev server and close the browser canvas.
   ```

## Conserva esta sesión para las lecciones siguientes

Todavía no crees ni combines la PR del filtrado. Conserva esta sesión, este árbol de trabajo y esta rama para las lecciones 4 a 8. A continuación, aplicarás instrucciones personalizadas al código de filtrado y añadirás las mejoras de la habilidad y de QA antes de crear y combinar la PR conjunta en la lección 8. Puedes dejar la sesión inactiva después de detener el servidor de desarrollo y volver a ella en la lección siguiente.

## Resumen y siguientes pasos

Has utilizado distintos modos de agente para desarrollar y revisar una funcionalidad. En esta lección:

- iniciaste una sesión de agente desde una incidencia de GitHub.
- definiste los requisitos en el modo **Plan**.
- implementaste la funcionalidad con **Autopilot**.
- revisaste el código.
- validaste la funcionalidad manualmente en un lienzo del navegador.

A continuación, profundizarás en cómo se genera el código y te asegurarás de que siga las prácticas documentadas mediante el [uso de instrucciones personalizadas][next-lesson].

## Recursos

- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
