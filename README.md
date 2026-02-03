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

PAI is pre-installed at `/root/.claude`. To configure and customize:

### Run PAI Configuration Wizard

```bash
cd /root/.claude
bun run INSTALL.ts
```

### PAI Features

- **Skills System** - Specialized capabilities in `/root/.claude/skills/`
- **Agents** - Custom AI agent configurations
- **Fabric Patterns** - 240+ content analysis patterns
- **System Prompts** - Customizable behavior templates

### PAI Documentation

See the PAI skill files in `/root/.claude/skills/PAI/` for detailed documentation:
- `SKILL.md` - Core system overview
- `PAI_ARCHITECTURE.md` - Technical architecture

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
