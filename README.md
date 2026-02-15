# llenv

Lightweight CLI to apply environment variables from a single config before launching any command. Zero dependencies — just Node.js.

## Install

```bash
# Copy to somewhere on your PATH
cp llenv /usr/local/bin/
# or symlink
ln -s "$(pwd)/llenv" /usr/local/bin/llenv
```

## Config

All environments live in a single `~/.llenv/config.json`:

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
    }
  }
}
```

- `defaultEnv` — name of the env to use when `--env` is omitted. If not set, the first entry in `envs` is used.
- `envs` — object mapping environment names to their variables.

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

An example config is included in `examples/simple.json`:

```bash
LLENV_CONFIG=./examples/simple.json llenv --list
LLENV_CONFIG=./examples/simple.json llenv --show dev
LLENV_CONFIG=./examples/simple.json llenv --env prod printenv APP_ENV
```
