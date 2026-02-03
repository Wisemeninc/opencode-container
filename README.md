# OpenCode Docker Container

A pre-configured Docker container with OpenCode CLI and Personal AI Infrastructure (PAI) for AI-assisted development.

## Features

- **OpenCode CLI** - AI coding assistant at `/root/.opencode/bin/opencode`
- **PAI v2.5** - Personal AI Infrastructure from danielmiessler at `/root/.claude`
- **Bun Runtime** - JavaScript/TypeScript runtime for PAI scripts
- **SSH Access** - Remote access on port 2222
- **Persistent Volumes** - Configs survive container restarts

## Quick Start

### 1. Build and Start

```bash
docker-compose up -d --build
```

### 2. Connect via SSH

```bash
ssh root@localhost -p 2222
# password: password123
```

### 3. Run OpenCode

```bash
opencode
```

## Using PAI (Personal AI Infrastructure)

PAI v2.5 is pre-installed at `/root/.claude` and automatically enhances OpenCode with additional capabilities. When you run `opencode`, it reads the PAI configuration from `~/.claude` to provide:

- **Skills System** - Specialized capabilities (research, OSINT, web assessment, etc.)
- **Agents** - Custom AI agent personalities and configurations
- **Fabric Patterns** - 240+ content analysis and transformation patterns
- **Hooks** - Automated behaviors triggered by events
- **System Prompts** - Customizable behavior templates in `CLAUDE.md`

### How It Works

PAI works by providing context and instructions to OpenCode via the `~/.claude` directory structure. When OpenCode starts, it reads:

1. `~/.claude/CLAUDE.md` - Main system instructions
2. `~/.claude/skills/` - Available skill definitions
3. `~/.claude/agents/` - Agent configurations

Simply run `opencode` and PAI's enhancements are automatically available.

### Initial Setup (Optional)

To customize PAI with your name and preferences:

```bash
cd /root/.claude
bun run INSTALL.ts
```

### PAI Documentation

See the PAI skill files in `/root/.claude/skills/PAI/` for detailed documentation:
- `SKILL.md` - Core system overview
- `PAI_ARCHITECTURE.md` - Technical architecture

### Note on pai CLI

PAI includes a `pai` CLI tool (`~/.claude/skills/PAI/Tools/pai.ts`) designed for Claude Code. This tool is not compatible with OpenCode and should not be used. Use `opencode` directly instead.

## Volume Mounts

| Host Path | Container Path | Purpose |
|-----------|----------------|---------|
| `~/.claude` | `/root/.claude` | PAI config & skills |
| `~/.opencode` | `/root/.opencode` | OpenCode config |
| `./code_output` | `/workspace/output` | Code output directory |

## Configuration

### Environment Variables

Set your API keys in docker-compose.yml or pass them at runtime:

```yaml
environment:
  - ANTHROPIC_API_KEY=your_key_here
  - OPENAI_API_KEY=your_key_here
```

### SSH Keys

To use your own SSH keys instead of password auth:

```bash
# Copy your public key
docker cp ~/.ssh/id_rsa.pub opencode:/root/.ssh/authorized_keys
```

## Ports

| Port | Service |
|------|---------|
| 2222 | SSH |

## Building Manually

```bash
docker build -f Dockerfile.opencode -t opencode-container .
docker run -d -p 2222:22 --name opencode opencode-container
```

## Troubleshooting

### OpenCode not found
```bash
# Use full path or reload shell
/root/.opencode/bin/opencode
# or
source ~/.bashrc
```

### PAI not working
```bash
# Ensure Bun is in path
export PATH="/root/.bun/bin:$PATH"
# Check PAI installation
ls -la /root/.claude/
```

## License

MIT
