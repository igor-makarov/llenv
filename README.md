# llenv

Lightweight CLI to apply environment variables from a single JSON config before launching any command. Zero dependencies — just Node.js.

## Install

### Homebrew

```bash
brew install igor-makarov/formulae/llenv
```

## Config

Create `~/.llenv/config.json`:

```json
{
  "defaultEnv": "dev",
  "envs": {
    "dev": {
      "APP_ENV": "development",
      "DEBUG": "true"
    },
    "prod": {
      "APP_ENV": "production",
      "DEBUG": "false"
    },
    "ollama": {
      "OLLAMA_HOST": "http://localhost:11434",
      "OLLAMA_MODELS": "/data/models"
    },
    "debug": {
      "APP_ENV": "development",
      "DEBUG": "$SHOULD_DEBUG"
    }
  }
}
```

- `envs` — object mapping environment names to their variables.
- `defaultEnv` (optional) — env to use when `--env` is omitted. If not set, the first entry in `envs` is used.

### Variable interpolation

Values can reference existing environment variables using `$VAR` or `${VAR}` syntax. Unset variables expand to an empty string.

```json
{
  "envs": {
    "dev": {
      "DEBUG": "$SHOULD_DEBUG",
      "GREETING": "hello ${USER}"
    }
  }
}
```

```bash
SHOULD_DEBUG=1 llenv --env dev my-app
# DEBUG=1, GREETING=hello <your username>
```

## Usage

```bash
# Launch with default env
llenv claude

# Launch with a named env
llenv --env ollama claude

# Works with any command
llenv --env prod docker compose up

# List available environments
llenv --list

# Preview env vars without running anything
llenv --show prod
```

## Options

| Flag | Description |
|------|-------------|
| `--env <name>` | Use a named env from config |
| `--list` | List available environments |
| `--show [name]` | Print resolved vars without executing |
| `--help` | Show help |

## Override config path

Set `LLENV_CONFIG` to use a different config file:

```bash
LLENV_CONFIG=./my-config.json llenv --env prod my-app
```

## Try it out

Example configs are included in `examples/`:

```bash
LLENV_CONFIG=./examples/simple.json llenv --list
LLENV_CONFIG=./examples/simple.json llenv --show dev
LLENV_CONFIG=./examples/simple.json llenv --env prod printenv APP_ENV

# Variable interpolation
SHOULD_DEBUG=1 LLENV_CONFIG=./examples/interpolation.json llenv --show dev
```
