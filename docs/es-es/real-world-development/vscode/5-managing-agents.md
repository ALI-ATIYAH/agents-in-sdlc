---
title: "Ejercicio 5 - Supervisa y gestiona los agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando usas el modo de agente de GitHub Copilot, trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como el trabajo se realiza en tiempo real en tu equipo, Copilot Chat en VS Code muestra cada llamada a herramienta y cada edición de archivo a medida que se producen. Puedes revisar cada diferencia en línea, aceptar o rechazar cambios concretos y guiar la conversación con mensajes de seguimiento para ajustar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás las ediciones de archivos propuestas.
- guiarás al agente durante la sesión para añadir un selector de modo claro junto al de contraste alto.

## Escenario

El agente personalizado de accesibilidad que usaste en el [ejercicio 4][previous-lesson] está implementando el modo de contraste alto. Mientras trabaja, el equipo de diseño solicita además un modo claro para quienes encuentran demasiado oscuro el tema predeterminado. En lugar de iniciar otra conversación desde cero, guiarás la sesión existente para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en línea en el chat con las diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver cada llamada a herramienta. Verás lecturas de archivos, ediciones y los comandos de terminal que quiera ejecutar.
3. Cuando el agente proponga una edición, aparecerán las diferencias en línea:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** en la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a herramienta solicita permiso (por ejemplo, para ejecutar un comando de terminal), el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de contraste alto. Deberías ver un mensaje final que resuma los cambios.

## Guía al agente para añadir el modo claro

Ahora que el modo de contraste alto está implementado, ampliaremos la misma conversación para añadir un selector de modo claro. Como continuarás en la misma sesión de chat, el agente conserva todo el contexto del trabajo anterior: los componentes que modificó, el enfoque de estilo y el patrón de almacenamiento local.

1. En el campo de Copilot Chat, envía este mensaje de seguimiento:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. El agente continuará desde donde lo dejó, explorará los cambios realizados y propondrá los cambios necesarios para añadir el modo claro.
3. Revisa cada nueva edición a medida que aparezca, igual que con el modo de contraste alto. Usa **Keep** o **Undo** para cada diferencia.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, intenta rediseñar los selectores de una forma que no te convence—, puedes detenerlo con el botón **Stop** junto al campo de chat y enviar un mensaje aclaratorio.

> [!TIP]
> Guiar una sesión en curso es más rápido que empezar de nuevo. Usa mensajes de seguimiento para solicitar ajustes («simplifica el CSS: usa variables CSS en lugar de duplicar selectores»), corregir el rumbo («no añadas un tercer selector; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos selectores»).

## Revisa los cambios

Antes de confirmar el trabajo, revisa rápidamente todo lo que ha modificado el agente.

1. Abre la vista **Source Control** en VS Code.
2. Revisa la lista completa de archivos modificados. Deberían aparecer actualizaciones de componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos modificados y revisa las diferencias. Comprueba que se hayan aplicado los patrones de accesibilidad del agente personalizado: atributos ARIA, navegación con el teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios en el panel Source Control. Los publicarás en la [lección siguiente][next-lesson].

## Resumen y siguientes pasos

Has usado el modo de agente de Copilot Chat para supervisar y guiar una sesión en curso: revisaste cada edición, aceptaste o rechazaste cambios y enviaste mensajes de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección has aprendido a:

- supervisar el trabajo de un agente en tiempo real.
- aceptar y rechazar ediciones individuales.
- guiar una sesión en curso con mensajes de seguimiento.

En la [lección siguiente][next-lesson], publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios que ha producido el agente personalizado local durante este taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usa el modo de agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Itera sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
