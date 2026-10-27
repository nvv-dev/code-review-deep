# code-review-deep

> Pocos hallazgos, verificados, con severidad y línea.

**Revisión de código profunda** · Skill para agentes de código de la comunidad [nvvdev](https://github.com/nvv-dev). Funciona en Claude Code como plugin o como skill suelta, y en cualquier agente compatible con el formato abierto [`SKILL.md`](https://agentskills.io).

*English summary at the end.*

## Qué hace

Revisa solo el código cambiado buscando errores, cambios que rompen, problemas de seguridad, funciones a medio activar (flags, IDs de prueba, botones de depuración en release) y pruebas debilitadas. No reporta sospechas sin investigarlas; cada hallazgo lleva veredicto (confirmado, por validar o descartado), severidad, archivo y línea, y la corrección mínima.

## Objetivo

Que la revisión encuentre lo que va a doler en producción sin ahogar al autor en comentarios cosméticos.

## Cuándo usarla

- Revisar un PR, una rama o un diff antes del merge
- Segunda opinión sobre un cambio de riesgo

## Cuándo no

- Revisar estructura y mantenibilidad: usa code-review-maintainability

## Ejemplo

```text
Revisa el PR #42 con code-review-deep.
```

## Cómo se activa

Esta skill **se activa sola**: Claude Code lee su descripción y la carga cuando la tarea coincide. También puedes pedirla por nombre ("usa la skill code-review-deep") o invocarla con `/code-review-deep:code-review-deep` (plugin) o `/code-review-deep` (copiada a `.claude/skills/`).

## Instalación

### Opción 1: Claude Code, como plugin (recomendada)

Dentro de Claude Code:

```text
/plugin marketplace add nvv-dev/claude-plugins
/plugin install code-review-deep@nvvdev
```

La primera línea agrega el catálogo de nvvdev una sola vez; la segunda instala esta skill. Si prefieres el paquete completo, instala `dev-workflow@nvvdev` en su lugar (incluye esta skill y las demás de [dev-workflow](https://github.com/nvv-dev/dev-workflow)); no instales los dos a la vez.

### Opción 2: Claude Code, copiando la skill

Para un solo proyecto (se comparte con tu equipo por git):

```bash
git clone --depth 1 https://github.com/nvv-dev/code-review-deep.git /tmp/code-review-deep
mkdir -p .claude/skills
cp -R /tmp/code-review-deep/skills/code-review-deep .claude/skills/
```

Para todos tus proyectos, cópiala en `~/.claude/skills/` en lugar de `.claude/skills/`.

### Opción 3: otros agentes

Copia la carpeta `skills/code-review-deep/` en la carpeta de skills de tu agente (cada agente documenta la suya). Con la CLI [skills](https://github.com/vercel-labs/skills) de Vercel Labs:

```bash
DISABLE_TELEMETRY=1 npx skills add nvv-dev/code-review-deep
```

### Verificar

Abre una sesión nueva y pregunta "¿qué skills tienes disponibles?"; `code-review-deep` debe aparecer. Con el plugin, `/plugin` la muestra como instalada.

### Actualizar o desinstalar

- Plugin: `/plugin marketplace update nvvdev` para actualizar; `/plugin uninstall code-review-deep@nvvdev` para quitarla.
- Copia manual: repite la copia para actualizar, o borra la carpeta `.claude/skills/code-review-deep`.

Antes de instalar cualquier skill, incluida esta, conviene leerla: es un solo archivo, [`skills/code-review-deep/SKILL.md`](skills/code-review-deep/SKILL.md). Ver [vetting-external-skills](https://github.com/nvv-dev/vetting-external-skills).

## Contenido

| Ruta | Qué es |
|---|---|
| `skills/code-review-deep/SKILL.md` | La skill: instrucciones en inglés para que los escáneres de seguridad las analicen completas; el agente responde en tu idioma |
| `.claude-plugin/plugin.json` | Manifiesto para instalarla como plugin |
| `.github/workflows/` | Validación de estructura y escaneo con SkillSpector en cada cambio |

## Contribuir

Este repo se **genera** desde [nvv-dev/dev-workflow](https://github.com/nvv-dev/dev-workflow), donde vive la versión canónica. Abre issues y PRs allí. La mejor contribución es un caso real en la sección *Known failures* de la skill, con la regla que lo evita.

## Origen

Escrita desde cero por nvvdev. Inspirada en: [cursor/plugins — thermos](https://github.com/cursor/plugins/tree/main/thermos) (deep review rubric), [cloudflare/security-audit-skill](https://github.com/cloudflare/security-audit-skill) (three-verdict findings).

## Licencia

MIT.

---

## English

**code-review-deep** — Reviews only what changed for bugs, breaking changes, security and half-activated features; every finding has a verdict, severity and file:line.

Install in Claude Code with `/plugin marketplace add nvv-dev/claude-plugins` and `/plugin install code-review-deep@nvvdev`, or copy `skills/code-review-deep/` into `.claude/skills/` (project) or `~/.claude/skills/` (personal). Other agents: copy the folder into their skills directory, or run `DISABLE_TELEMETRY=1 npx skills add nvv-dev/code-review-deep`. Loads automatically when the task matches its description. Canonical source: [nvv-dev/dev-workflow](https://github.com/nvv-dev/dev-workflow). License: MIT.
