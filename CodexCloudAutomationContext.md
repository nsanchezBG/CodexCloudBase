# CodexCloudBase Automation Context

Fecha de contexto: 2026-06-19  
Repo GitHub: `nsanchezBG/CodexCloudBase`  
Arcade: `NicoSavesTheWorld Brain Training`  
Objetivo: automatizar la creacion diaria de juegos web de puzzle mental con Codex Cloud + GitHub Actions + GitHub Pages.

## Objetivo General

Todos los dias a las 3:00 a.m. hora Peru, GitHub Actions debe crear un issue en `nsanchezBG/CodexCloudBase` y comentar mencionando a `@codex`.

Codex Cloud debe:

1. Leer `instructions.md`.
2. Leer `memory.md`.
3. Idear un juego nuevo de puzzle mental distinto a los anteriores.
4. Crear una carpeta nueva en la raiz del repo, por ejemplo `prism-courier-2026-06-19/`.
5. Crear el juego en `<game-slug>/index.html`.
6. Actualizar el arcade raiz `index.html`.
7. Actualizar `memory.md`.
8. Hacer commit.
9. Hacer push directo a `main`.
10. Verificar que los cambios esten visibles en GitHub `main`.

No se debe crear PR. No se debe usar `make_pr`. No se debe crear un repositorio nuevo por juego.

## Estructura Del Repo

Repo base:

```text
CodexCloudBase/
  Assets/
  index.html
  instructions.md
  memory.md
  .github/workflows/daily-codex.yml
  echo-orchard-2026-06-19/
  lantern-ledger-2026-06-19/
  shadow-cartographer-2026-06-19/
  prism-courier-2026-06-19/
```

El `index.html` raiz es el arcade publico. No detecta carpetas automaticamente: cada corrida debe agregar manualmente un objeto nuevo al arreglo `const games = [...]`.

GitHub Pages usa:

```text
https://nsanchezBG.github.io/CodexCloudBase/
```

Cada juego usa:

```text
https://nsanchezBG.github.io/CodexCloudBase/<game-slug>/
```

## Juegos Publicados Hasta Ahora

Publicados correctamente en `main`:

1. Echo Orchard
   - Carpeta: `echo-orchard-2026-06-19/`
   - Mecanica: memoria de secuencias con transformaciones de frutas.

2. Lantern Ledger
   - Carpeta: `lantern-ledger-2026-06-19/`
   - Mecanica: deduccion aritmetica en grilla 4x4.

3. Shadow Cartographer
   - Carpeta: `shadow-cartographer-2026-06-19/`
   - Mecanica: reconstruccion espacial 5x5 con conteos de filas/columnas.

4. Prism Courier
   - Carpeta: `prism-courier-2026-06-19/`
   - Mecanica: ruta sin repetir casillas con transformaciones de color.
   - Commit reportado por Codex: `a82599a`

Intento fallido:

- Tempo Atelier
  - Issue: `#7`
  - Codex lo creo localmente, pero no pudo hacer push a `main`.
  - Error: `403 Permission to nsanchezBG/CodexCloudBase.git denied to chatgpt-codex-connector[bot]`.

## Workflow De GitHub Actions

Archivo:

```text
.github/workflows/daily-codex.yml
```

Schedule:

```yaml
on:
  schedule:
    - cron: "0 8 * * *" # 3:00 a.m. Peru time
  workflow_dispatch:
```

Nota: GitHub Actions usa UTC. Peru esta en UTC-5, asi que 08:00 UTC = 03:00 Peru.

El workflow:

1. Usa `actions/github-script@v7`.
2. Usa el secret de GitHub Actions `GH_PAT`.
3. Crea un issue diario.
4. Crea un comentario en el issue empezando con `@codex please run this task now`.

El comentario incluye instrucciones para:

- no crear repo nuevo,
- crear juego dentro del repo,
- actualizar `index.html`,
- actualizar `memory.md`,
- limpiar `extraheader`,
- hacer `git push origin HEAD:main`,
- verificar en GitHub `main`.

## Secrets

Hay dos sistemas de secrets separados.

### GitHub Actions Secret

Nombre:

```text
GH_PAT
```

Ubicacion:

```text
GitHub repo > Settings > Secrets and variables > Actions
```

Uso:

- Permite que GitHub Actions cree el issue y comente mencionando a `@codex`.

### Codex Environment Secret

Nombre:

```text
GH_PUSH_TOKEN
```

Ubicacion:

```text
Codex Cloud > Environments > nsanchezBG/CodexCloudBase1 > Secrets
```

Uso:

- Permite que el contenedor de Codex configure credenciales de Git para hacer push a `main`.

Importante:

- `GH_PUSH_TOKEN` puede no estar visible durante la fase del agente.
- En Codex Cloud, los secrets del environment se usan en setup/maintenance.
- Por eso no hay que pedirle al agente que lea `GH_PUSH_TOKEN` durante la tarea.

## Environment De Codex Cloud

Environment:

```text
nsanchezBG/CodexCloudBase1
```

Repo asociado:

```text
nsanchezBG/CodexCloudBase
```

Internet:

```text
Activado
```

Cache:

```text
Activado
```

Secret:

```text
GH_PUSH_TOKEN
```

## Setup Script Actual Recomendado

Este debe estar en `Script de configuracion` del environment.

```bash
set +x

test -n "$GH_PUSH_TOKEN" || {
  echo "GH_PUSH_TOKEN is missing in Codex environment setup"
  exit 1
}

git config --global user.name "Codex Daily Bot"
git config --global user.email "codex-daily-bot@users.noreply.github.com"

git config --global --unset-all http.https://github.com/.extraheader || true
git config --local --unset-all http.https://github.com/.extraheader || true

git config --global credential.helper store
git config --global credential.useHttpPath false

printf "https://x-access-token:%s@github.com\n" "$GH_PUSH_TOKEN" > ~/.git-credentials
chmod 600 ~/.git-credentials

if git remote | grep -qx origin; then
  git remote set-url origin "https://x-access-token:${GH_PUSH_TOKEN}@github.com/nsanchezBG/CodexCloudBase.git"
else
  git remote add origin "https://x-access-token:${GH_PUSH_TOKEN}@github.com/nsanchezBG/CodexCloudBase.git"
fi

git remote set-url --push origin "https://x-access-token:${GH_PUSH_TOKEN}@github.com/nsanchezBG/CodexCloudBase.git"
```

## Maintenance Script Actual Recomendado

Este tambien debe estar en `Script de mantenimiento` del environment.

Razon: vimos que el setup script puede aplicarse al crear contenedores, pero no necesariamente antes de cada tarea. El maintenance script se ejecuta antes de cada tarea y deberia reestablecer credenciales de Git de forma consistente.

```bash
set +x

test -n "$GH_PUSH_TOKEN" || {
  echo "GH_PUSH_TOKEN is missing in Codex environment maintenance"
  exit 1
}

git config --global user.name "Codex Daily Bot"
git config --global user.email "codex-daily-bot@users.noreply.github.com"

git config --global --unset-all http.https://github.com/.extraheader || true
git config --local --unset-all http.https://github.com/.extraheader || true
git config --global --unset-all http.extraheader || true
git config --local --unset-all http.extraheader || true

git config --global credential.helper store
git config --global credential.useHttpPath false

printf "https://x-access-token:%s@github.com\n" "$GH_PUSH_TOKEN" > ~/.git-credentials
chmod 600 ~/.git-credentials

if git remote | grep -qx origin; then
  git remote set-url origin "https://x-access-token:${GH_PUSH_TOKEN}@github.com/nsanchezBG/CodexCloudBase.git"
else
  git remote add origin "https://x-access-token:${GH_PUSH_TOKEN}@github.com/nsanchezBG/CodexCloudBase.git"
fi

git remote set-url --push origin "https://x-access-token:${GH_PUSH_TOKEN}@github.com/nsanchezBG/CodexCloudBase.git"

git config --global url."https://x-access-token:${GH_PUSH_TOKEN}@github.com/".insteadOf "https://github.com/"
git config --global url."https://x-access-token:${GH_PUSH_TOKEN}@github.com/".pushInsteadOf "https://github.com/"
```

Despues de modificar setup o maintenance script:

1. Guardar environment.
2. Hacer `Restablecer cache`.
3. Correr prueba manual desde GitHub Actions.

## Problemas Encontrados

### 1. Codex hacia commit local pero no push

Mensaje recurrente:

```text
remote: Permission to nsanchezBG/CodexCloudBase.git denied to chatgpt-codex-connector[bot].
fatal: unable to access 'https://github.com/nsanchezBG/CodexCloudBase.git/': The requested URL returned error: 403
```

Causa probable:

- El push estaba usando credenciales del bot `chatgpt-codex-connector[bot]`, no el PAT configurado como `GH_PUSH_TOKEN`.

### 2. `GH_PUSH_TOKEN` no visible durante la tarea

Diagnostico en issue #3:

```text
GH_PUSH_TOKEN FAIL
origin auth read OK
origin auth write OK
```

Interpretacion:

- Normal que el secret no sea visible durante la tarea.
- Los secrets del environment se consumen en setup/maintenance.
- No hay que depender de leer `GH_PUSH_TOKEN` dentro del agente.

### 3. Setup script fallo por no existir `origin`

Error:

```text
error: No such remote 'origin'
```

Solucion:

Usar:

```bash
if git remote | grep -qx origin; then
  git remote set-url origin "..."
else
  git remote add origin "..."
fi
```

### 4. `git remote get-url origin` podia filtrar token

Se reemplazo en `instructions.md` y workflow por:

```bash
git remote | grep -qx origin || git remote add origin https://github.com/nsanchezBG/CodexCloudBase.git
```

Ademas se instruyo a Codex:

```text
Do not run git remote -v, git remote get-url origin, or otherwise print authenticated remote URLs.
```

### 5. Exito no estable

Issue #6 funciono:

- Creo Prism Courier.
- Hizo push a `main`.
- Verifico GitHub `main`.

Issue #7 fallo:

- Creo Tempo Atelier localmente.
- Fallo al hacer push con 403 del bot.

Hipotesis:

- El setup script corrigio el primer contenedor, pero tareas posteriores no rehicieron credenciales.
- Por eso se agrego el mismo flujo al `Script de mantenimiento`.

## Estado Actual Al Migrar

El usuario agrego el `Script de mantenimiento`, borro cache, y esta corriendo una nueva prueba manual para verificar estabilidad despues del cambio.

Pendiente:

1. Revisar el nuevo issue/run manual despues del cambio de maintenance script.
2. Confirmar si aparece un juego nuevo en `memory.md` y `index.html`.
3. Confirmar que Codex reporta `git push origin HEAD:main` exitoso.
4. Si falla otra vez, revisar si el maintenance script realmente se esta ejecutando y si hay algun `extraheader`/credential helper sobreescribiendo el token.

## Comandos / Checks Utiles

Leer issues recientes:

```text
GitHub connector: search issues in nsanchezBG/CodexCloudBase with query "Daily brain training game", sort created desc.
```

Verificar `memory.md`:

```text
Fetch file: nsanchezBG/CodexCloudBase, path memory.md, ref main.
```

Verificar arcade:

```text
Fetch file: nsanchezBG/CodexCloudBase, path index.html, ref main.
```

Verificar carpeta de juego:

```text
Fetch file: nsanchezBG/CodexCloudBase, path <game-slug>/index.html, ref main.
```

## Recomendacion Si Sigue Fallando

Si despues del maintenance script sigue apareciendo:

```text
Permission denied to chatgpt-codex-connector[bot]
```

probar un comentario diagnostico en el issue fallido:

```md
@codex Diagnostics only. Do not print secrets or remote URLs.

Run:

git config --get-all http.https://github.com/.extraheader >/tmp/eh 2>/dev/null && test -s /tmp/eh && echo "extraheader present" || echo "extraheader absent"
git config --get-all http.extraheader >/tmp/eh2 2>/dev/null && test -s /tmp/eh2 && echo "generic extraheader present" || echo "generic extraheader absent"
git config --global credential.helper || true
git remote | grep -qx origin && echo "origin exists" || echo "origin missing"
git push origin HEAD:main
```

No usar `git remote -v`.
No usar `git remote get-url origin`.

## Archivos Locales Relevantes

Este archivo fue creado en:

```text
C:\Users\Usuario\Desktop\HillsWebApp1\CodexCloudAutomationContext.md
```

La carpeta actual no es el proyecto real de esta automatizacion. El usuario quiere migrar el contexto a otra carpeta para no seguir invadiendo `HillsWebApp1`.
