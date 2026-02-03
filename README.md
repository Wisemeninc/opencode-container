# Opencode Docker Setup

This setup provides an SSH-accessible Opencode container with a mounted volume for code output.

## Setup

1. Build and start the container:
```bash
docker-compose up -d --build
```

2. SSH into the container:
```bash
ssh root@localhost -p 2222
password: password123
```

3. Your output directory is mounted at `/workspace/output` within the container.
