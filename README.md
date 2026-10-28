# Status line para Claude Code

Añade una barra de 2 líneas al pie de Claude Code con tu proyecto, la rama de git, el modelo y cuánto uso te queda.

```
proyectos/mi-app · main ●3 ↑1 · Opus (high)
ctx ▓▓▓░░░░░ 42% usado · 5h: 80% libre (reinicia 18:30) · semana: 50% libre (reinicia mié 09:00)
```

Funciona en macOS, Linux y Windows.

## Instalarla en 1 paso

1. Abre el prompt en texto plano: **[claude-code-statusline-prompt.md](https://gist.githubusercontent.com/Jacopalas/00c53e2f087c8c66790ddf294d349397/raw/claude-code-statusline-prompt.md)**
2. Selecciona todo (Cmd+A en Mac, Ctrl+A en Windows y Linux) y cópialo.
3. Pégalo en Claude Code como mensaje y envíalo.

Si prefieres verlo con formato antes, está en [el gist](https://gist.github.com/Jacopalas/00c53e2f087c8c66790ddf294d349397).

Ya está. Claude revisa primero que el prompt sea seguro, lo instala y te pregunta antes de cambiar algo que ya tengas. Puedes pasárselo las veces que quieras: si ya está instalada, no rompe nada.

No hace falta reiniciar. La barra aparece después de la siguiente respuesta de Claude.

---

## Todo lo de abajo es opcional

Solo si quieres saber qué significa cada parte o qué hace el script.

### Qué significa cada parte

| Parte | Qué significa |
|---|---|
| `proyectos/mi-app` | La carpeta del proyecto. Con Cmd-clic o Ctrl-clic se abre la página del repo (en terminales que admiten enlaces). |
| `main` | La rama de git. |
| `●3` | 3 archivos cambiados o nuevos sin guardar en git. |
| `↑1` / `↓2` | 1 commit por subir / 2 commits por bajar. |
| `Opus (high)` | El modelo y su nivel de esfuerzo. |
| `ctx ▓▓▓░░░░░ 42% usado` | Cuánta memoria de la conversación ha usado Claude. Verde; amarillo desde el 70 %; rojo desde el 90 %. |
| `5h: 80% libre` | Lo que te queda del límite de uso de 5 horas y a qué hora se reinicia. |
| `semana: 50% libre` | Lo que te queda del límite semanal y qué día y a qué hora se reinicia. Amarillo por debajo del 30 %; rojo por debajo del 10 %. |

Lo que no aplica no sale:

- Fuera de un proyecto de git no hay rama, `●`, `↑` ni `↓`.
- `↑` y `↓` solo salen si la rama está enlazada a una rama remota.
- La línea 2 puede salir vacía hasta tu primer mensaje.
- Las partes de 5 h y semana solo salen en los planes Pro y Max de Claude, no con una clave de API.

### Qué hace el script

- Lee los datos que le pasa Claude Code e imprime 2 líneas. Nada más.
- No se conecta a internet.
- Solo escribe un pequeño archivo de caché en la carpeta temporal (la ruta de la carpeta y los datos de git, que se actualizan cada 5 segundos).
- El script completo está dentro del prompt, en texto plano, para que cualquiera pueda leerlo antes de instalarlo.

### Requisitos

El prompt los comprueba por ti.

- **bash**: viene con macOS y Linux. En Windows viene con [Git para Windows](https://git-scm.com/download/win) (Git Bash), que Claude Code ya usa.
- **jq**: un programa pequeño que lee datos. Si falta, Claude te dice cómo instalarlo y te pide permiso antes.

En Windows, `~/.claude` es la carpeta `C:\Users\<tu usuario>\.claude`. Si usas Claude Code dentro de WSL, todo se hace dentro de WSL.

### Si algo falla

| Problema | Solución |
|---|---|
| No sale nada | Manda un mensaje y espera la respuesta. Si sigue sin salir, comprueba que la carpeta es de confianza (Claude Code lo pregunta al abrir una carpeta nueva) y pídele a Claude que revise `~/.claude/settings.json`. |
| `jq: command not found` | Falta jq. Instálalo y cierra y vuelve a abrir el terminal y Claude Code. |
| Cuadrados o `?` en vez de ▓ ░ ● ↑ | La fuente del terminal no tiene esos símbolos. Usa Windows Terminal, iTerm2 o Terminal de macOS. |
| No salen las partes de 5 h y semana | Es normal con una clave de API o con un plan sin límites de uso. |

### Desinstalarla

Pídele a Claude: «Quita la status line: borra la clave statusLine de ~/.claude/settings.json y el archivo ~/.claude/statusline-command.sh».
