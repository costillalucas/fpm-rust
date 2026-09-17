# reproducible

Plantilla de arranque para proyectos de "reproducir un paper / derivar un
resultado con agentes": trae el toolkit [`agent-team`](https://github.com/matiaszaldarriaga/agent-team)
como submódulo y la estructura mínima alrededor para que un proyecto nuevo
empiece a andar en minutos.

`agent-team` lanza un equipo acotado de agentes (Claude Code o Codex, a
elección) sobre un objetivo concreto — una derivación, una feature, un
draft — y se detiene solo. Vos decidís si lo continuás, lo congelás o lo
abandonás. Ver `agent-team/README.md` para el manual completo.

## Estructura

```
OBJECTIVE.template.md   plantilla del objetivo: copiala a OBJECTIVE.md y completala
papers/                 el material fuente que el equipo puede leer (papers, specs, datos)
agent-team/             el toolkit (submódulo git, no se edita acá)
jobs/                   donde `job` crea cada corrida (gitignored, es estado local)
```

## Puesta en marcha de un proyecto nuevo

1. Cloná este repo (o usalo como template de GitHub) con los submódulos:

   ```sh
   git clone --recurse-submodules <url-del-repo-nuevo>
   # si ya lo clonaste sin --recurse-submodules:
   git submodule update --init
   ```

2. Instalá `agent-team` una sola vez por máquina (deja `job` en el PATH y
   registra la skill para Claude/Codex):

   ```sh
   cd agent-team && ./install.sh && cd ..
   ```

3. Copiá la plantilla de objetivo y completala para este proyecto:

   ```sh
   cp OBJECTIVE.template.md OBJECTIVE.md
   ```

4. Poné el material fuente en `papers/` (o lo que corresponda).

5. Lanzá el job:

   ```sh
   job new derive "$(cat OBJECTIVE.md)" --pi --run
   open jobs/<id>/view.html       # monitor + caja para inyectar directivas
   ```

   `derive` es una de varias recetas (`agent-team/recipes/`); también hay
   `feature`, `draft`, `wiki`. `job roles` y `job recipes` listan lo
   instalado.

## Actualizar el toolkit

`agent-team` es un submódulo — vive en su propio repo y se actualiza aparte:

```sh
cd agent-team && git pull origin main && cd ..
git add agent-team && git commit -m "bump agent-team"
```

## Correr un job con otra cuenta de Claude

`agent-team` no tiene noción de credenciales por job: cada llamada a `claude`
hereda el login OAuth de la máquina donde corre `job` (`~/.claude/.credentials.json`),
sin importar el rol (`agentteam/backends.py`). Eso descarta dos atajos:

- **`ANTHROPIC_API_KEY` por comando** solo sirve si la otra cuenta tiene una
  API key de [console.anthropic.com](https://console.anthropic.com) — una
  suscripción Pro/Max de claude.ai no emite una.
- **`claude logout` / `claude login` en esta misma máquina** cambia la cuenta
  para toda la máquina, no solo para este job — afecta cualquier otra sesión
  de Claude Code corriendo ahí.

Para una suscripción Pro/Max sin tocar la sesión actual, la forma limpia es
un **segundo Codespace** sobre este mismo repo, logueado con la otra cuenta:

1. En GitHub, sobre este repo: botón **Code** → pestaña **Codespaces** →
   **Create codespace on main**.
2. Ahí adentro, loguear la otra cuenta:
   ```sh
   claude login
   ```
3. Instalar `agent-team` en ese Codespace (el submódulo ya viene clonado):
   ```sh
   cd agent-team && git submodule update --init --recursive 2>/dev/null; ./install.sh && cd ..
   ```
4. Lanzar el job ahí:
   ```sh
   job new derive "$(cat OBJECTIVE.md)" --pi --run
   ```

Ambos Codespaces apuntan al mismo remoto, así que una vez que ese job
comitee y pushee su resultado, alcanza con un `git pull` normal desde
cualquier otro Codespace/clon para traerlo.

## Qué NO incluye esta plantilla a propósito

Cosas específicas de un proyecto (script de setup del entorno, el paper
puntual a reproducir, los `OBJECTIVE.md` ya completados, las corridas en
`jobs/`) no van en la plantilla — nacen en cada proyecto concreto. Esta
plantilla es el esqueleto reutilizable, no un proyecto en sí.
