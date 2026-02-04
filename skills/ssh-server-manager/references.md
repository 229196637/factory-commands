# SSH Server Manager References

## SSH Config Format

```
Host <alias>
    HostName <ip_or_domain>
    User <username>
    Port <port>
    IdentityFile <path_to_key>
```

## Common Commands Quick Reference

### System Information

| Command | Description |
|---------|-------------|
| `uname -a` | Full system info |
| `hostname` | Server hostname |
| `uptime` | System uptime |
| `whoami` | Current user |
| `id` | User ID and groups |

### Resource Monitoring

| Command | Description |
|---------|-------------|
| `free -h` | Memory usage (human readable) |
| `df -h` | Disk usage (human readable) |
| `df -h /` | Root partition usage |
| `du -sh /path` | Directory size |
| `top -bn1 \| head -20` | Process snapshot |
| `htop` | Interactive process viewer |

### Docker Commands

| Command | Description |
|---------|-------------|
| `docker --version` | Docker version |
| `docker compose version` | Compose version |
| `docker ps` | Running containers |
| `docker ps -a` | All containers |
| `docker images` | List images |
| `docker logs <name>` | Container logs |
| `docker logs -f <name>` | Follow logs |
| `docker exec -it <name> sh` | Shell into container |
| `docker compose up -d` | Start services |
| `docker compose down` | Stop services |
| `docker compose pull` | Pull latest images |
| `docker compose restart` | Restart services |
| `docker system prune -f` | Clean unused resources |

### File Operations

| Command | Description |
|---------|-------------|
| `ls -la` | List files with details |
| `cat <file>` | View file content |
| `head -n 50 <file>` | First 50 lines |
| `tail -n 50 <file>` | Last 50 lines |
| `tail -f <file>` | Follow file changes |
| `mkdir -p <path>` | Create directory recursively |
| `rm -rf <path>` | Remove directory (careful!) |
| `cp -r <src> <dst>` | Copy recursively |
| `mv <src> <dst>` | Move/rename |
| `chmod 600 <file>` | Set file permissions |
| `chown user:group <file>` | Change ownership |

### Network Commands

| Command | Description |
|---------|-------------|
| `netstat -tlnp` | Listening ports |
| `ss -tlnp` | Listening ports (modern) |
| `curl -I <url>` | HTTP headers |
| `ping -c 3 <host>` | Ping test |
| `ip addr` | Network interfaces |

### Service Management (systemd)

| Command | Description |
|---------|-------------|
| `systemctl status <service>` | Service status |
| `systemctl start <service>` | Start service |
| `systemctl stop <service>` | Stop service |
| `systemctl restart <service>` | Restart service |
| `systemctl enable <service>` | Enable on boot |
| `journalctl -u <service> -f` | Service logs |

---

## SSH MCP Tool Examples

### Execute Single Command

```json
{
  "host": "43.139.124.187",
  "username": "root",
  "privateKeyPath": "E:\\Download\\fat.pem",
  "command": "docker ps"
}
```

### Execute Multiple Commands

```json
{
  "command": "cd /opt/myapp && docker compose pull && docker compose up -d"
}
```

### With Custom Timeout

```json
{
  "host": "server.example.com",
  "username": "deploy",
  "privateKeyPath": "/path/to/key",
  "command": "docker build -t myapp .",
  "timeout": 300000
}
```

### Password Authentication

```json
{
  "host": "server.example.com",
  "username": "user",
  "password": "password",
  "command": "whoami"
}
```

---

## Deployment Patterns

### Docker Compose Deployment

```bash
# 1. Create app directory
mkdir -p /opt/myapp

# 2. Create docker-compose.yml
cat > /opt/myapp/docker-compose.yml << 'EOF'
version: '3.8'
services:
  app:
    image: myapp:latest
    restart: always
    ports:
      - "8080:8080"
EOF

# 3. Start services
cd /opt/myapp && docker compose up -d

# 4. Verify
docker ps | grep myapp
```

### Update Deployment

```bash
cd /opt/myapp && docker compose pull && docker compose up -d
```

### Rollback

```bash
cd /opt/myapp && docker compose down
docker tag myapp:latest myapp:broken
docker tag myapp:previous myapp:latest
docker compose up -d
```

---

## Troubleshooting

### SSH Connection Issues

| Error | Solution |
|-------|----------|
| Connection refused | Check if SSH service is running, verify port |
| Permission denied (publickey) | Check key path, key permissions (600) |
| Host key verification failed | Add host to known_hosts or use `-o StrictHostKeyChecking=no` |
| Connection timed out | Check firewall, network connectivity |

### Docker Issues

| Error | Solution |
|-------|----------|
| Cannot connect to Docker daemon | Start Docker: `systemctl start docker` |
| Permission denied | Add user to docker group or use sudo |
| Port already in use | Find process: `lsof -i :PORT` |
| Out of disk space | Clean: `docker system prune -af` |

---

*Last updated: 2026-01-19*
