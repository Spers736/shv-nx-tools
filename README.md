# shv-nx-tools

Herramientas en Python para que una IA cree y edite plugins C++ de **ScriptHookV-NX** (GTA V en Nintendo Switch), con reglas del SDK y revisión automática.

Solo usan la librería estándar de Python. Funcionan en **Linux, Termux (Android) y Windows**.

## Qué incluye

| Archivo | Para qué sirve |
|---|---|
| `shv_plugin_gen.py` | Crea un mod nuevo, corrige uno que falló al compilar (`--fix`) o modifica uno existente (`--edit`). Usa Gemini, Claude u OpenAI. |
| `shv_intellectual_bridge.py` | El "enseñador": reglas verificadas del SDK, lista de natives reales, guía de menús, guardado de ajustes y controles. También revisa el código (firmas de natives, archivos cortados, errores típicos). |
| `ai_agent.py` | Agente de IA para Termux y Linux que lee, crea y edita archivos. Con `/teach shv` aprende las reglas del SDK. |
| `tutorial.txt` | Cómo usar los comandos de cada archivo. |

`shv_plugin_gen.py` y `ai_agent.py` necesitan `shv_intellectual_bridge.py` en la misma carpeta.

## Preparación

```
python3 shv_plugin_gen.py --get-sdk
```

Descarga el SDK en `~/ScriptHookV-NX` (necesita `git`). Sin el SDK la IA inventa natives y firmas.

Claves de API (nunca las pongas dentro de los archivos):

```
export GEMINI_API_KEY="..."      # Gemini (por defecto)
export ANTHROPIC_API_KEY="..."   # Claude (-p anthropic)
export OPENAI_API_KEY="..."      # OpenAI (-p openai)
```

En Windows usa `python` o `py` en vez de `python3`, y comillas dobles.

## Uso rápido

```
# Mod nuevo
python3 shv_plugin_gen.py -n mimod "descripción de lo que hace el mod"

# Corregir con los errores que dio tu PC al compilar
python3 shv_plugin_gen.py --fix mimod.cpp --errors errores.txt

# Modificar un mod que ya existe
python3 shv_plugin_gen.py --edit mimod.cpp "cambios que quieres"

# Usar Claude si Gemini se quedó sin cuota
python3 shv_plugin_gen.py -p anthropic --edit mimod.cpp "cambios"
```

El generador guarda un registro de lo que cambió (`*_cambios.txt` y `*.diff`) y comprueba, comparando contra tu archivo, si la IA quitó algo que no pediste.

La compilación real se hace en tu PC con devkitPro.

## Qué comprueba

- Que cada native exista y tenga el número de argumentos correcto.
- Que no se use nada que no existe en un plugin de Switch (`fopen`, `.ini`, libc, `TRUE`/`FALSE`...).
- Que el archivo no esté cortado.
- Que los ajustes se guarden de verdad con `scriptSaveFile` / `scriptReadFile`.
- Errores conocidos de controles (palancas, `DISABLE_ALL_CONTROL_ACTIONS`).

## Aviso

Proyecto personal, no afiliado a Rockstar, Take-Two ni al autor de ScriptHookV-NX. Úsalo bajo tu responsabilidad.
