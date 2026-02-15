# llenv

Lightweight CLI to apply environment variables from JSON configs before launching any command. Zero dependencies — just Node.js.

## Install

```bash
# Copy to somewhere on your PATH
cp llenv /usr/local/bin/
# or symlink
ln -s "$(pwd)/llenv" /usr/local/bin/llenv
```

## Config

Environment files live in `~/.llenv/` as flat JSON objects:

```bash
mkdir -p ~/.llenv
```

```json
// ~/.llenv/ollama.json
{
  "OLLAMA_HOST": "http://localhost:11434",
  "OLLAMA_MODELS": "/data/models"
}
```

```json
// ~/.llenv/config.json  (default)
{
  "ANTHROPIC_API_KEY": "sk-ant-..."
}
```

## Usage

```bash
# Launch with a named environment
llenv --env ollama claude

# Launch with default env (~/.llenv/config.json)
llenv claude

# Works with any command
llenv --env staging docker compose up
llenv --env prod ssh deploy@server

# List available environments
llenv --list

# Preview env vars without running anything
llenv --show ollama
```

## Options

| Flag | Description |
|------|-------------|
| `--env <name>` | Load `~/.llenv/<name>.json` instead of the default |
| `--list` | List available environment configs |
| `--show <name>` | Print resolved vars without executing |
| `--help` | Show help |

## Override config directory

Set `LLENV_HOME` to use a different directory:

```bash
LLENV_HOME=/path/to/configs llenv --env prod my-app
```
