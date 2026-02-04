---
name: ssh-server-manager
description: SSH服务器管理技能。当需要连接SSH服务器执行命令、部署应用、检查服务器状态时使用。关键词：SSH、服务器、远程、部署、deploy、连接服务器、服务器状态。
---

# SSH服务器管理器

管理SSH服务器连接并执行远程操作。自动读取 `~/.ssh/config` 中的SSH配置。

## 触发条件

- 用户说"连接服务器"、"SSH到xxx"、"部署到服务器"
- 用户说"检查服务器状态"、"服务器磁盘"、"服务器内存"
- 用户说"在服务器上执行"、"远程执行"、"deploy"
- 用户提到SSH配置中的服务器名称（如"fat"）
- 需要执行远程命令或部署应用

---

## Phase 1: Discover SSH Configuration

### 1.1 Read SSH Config

First, read the SSH configuration file to discover available hosts:

```
Read file: ~/.ssh/config (Windows: C:\Users\<username>\.ssh\config)
```

### 1.2 Parse Host Information

Extract from SSH config:
- Host alias (e.g., `fat`)
- HostName (IP or domain)
- User
- IdentityFile (SSH key path)
- Port (default: 22)

### 1.3 List Available Hosts

If user doesn't specify a host, list available hosts for selection.

---

## Phase 2: Connect and Execute

### 2.1 Use SSH MCP Tools

Use the built-in SSH MCP tools:

| Tool | Purpose |
|------|---------|
| `ssh___ssh_execute` | Execute command on remote server |
| `ssh___ssh_connections` | Manage SSH connections (list/close) |

### 2.2 ssh_execute Parameters

```json
{
  "host": "IP or hostname",
  "username": "user",
  "privateKeyPath": "path/to/key.pem",
  "command": "command to execute",
  "port": 22,
  "timeout": 10000
}
```

### 2.3 Connection Management

```json
// List active connections
{ "action": "list" }

// Close specific connection
{ "action": "close", "connectionKey": "user@host:port" }

// Close all connections
{ "action": "close_all" }
```

---

## Phase 3: Common Operations

### 3.1 Server Status Check

```bash
# System info
uname -a

# Memory usage
free -h

# Disk usage
df -h

# CPU info
cat /proc/cpuinfo | grep "model name" | head -1

# Running processes
ps aux | head -20
```

### 3.2 Docker Operations

```bash
# Check Docker installation
docker --version
docker compose version

# List containers
docker ps -a

# Container logs
docker logs <container_name>

# Deploy with compose
cd /path/to/app && docker compose up -d

# Stop containers
docker compose down
```

### 3.3 File Operations

```bash
# Create directory
mkdir -p /opt/myapp

# List files
ls -la /path

# View file content
cat /path/to/file

# Check file exists
test -f /path/to/file && echo "exists" || echo "not found"
```

---

## Phase 4: Output Report

### 4.1 Connection Report Template

```markdown
## SSH连接报告

### 服务器信息
- **主机**: [host alias] ([IP])
- **用户**: [username]
- **端口**: [port]
- **状态**: ✅ 已连接 / ❌ 连接失败

### 执行结果
- **命令**: `[command]`
- **退出码**: [exit code]
- **输出**:
\`\`\`
[stdout]
\`\`\`

### 错误信息（如有）
\`\`\`
[stderr]
\`\`\`
```

### 4.2 Server Status Report Template

```markdown
## 服务器状态报告

### 基本信息
- **主机**: [hostname]
- **系统**: [OS info]

### 资源使用
| 资源 | 已用 | 可用 | 使用率 |
|------|------|------|--------|
| 内存 | X GB | Y GB | Z% |
| 磁盘 | X GB | Y GB | Z% |

### Docker状态
- **Docker版本**: [version]
- **运行容器数**: [count]

### 容器列表
| 容器名 | 镜像 | 状态 | 端口 |
|--------|------|------|------|
| xxx | xxx | Running | 80:80 |
```

---

## Edge Case Handling

### Case 1: SSH Config Not Found

```
提示: 未找到 SSH 配置文件 (~/.ssh/config)
请提供以下连接信息:
1. 服务器IP/主机名
2. 用户名
3. SSH密钥路径或密码
```

### Case 2: Connection Failed

```
提示: SSH连接失败
可能原因:
1. 服务器不可达 - 检查网络和IP
2. 认证失败 - 检查用户名和密钥
3. 端口错误 - 确认SSH端口
4. 密钥权限 - 确保密钥文件权限正确
```

### Case 3: Docker Not Installed

```
提示: 服务器未安装Docker
安装命令 (Ubuntu/Debian):
curl -fsSL https://get.docker.com | sh
```

### Case 4: Permission Denied

```
提示: 权限不足
解决方案:
1. 使用 sudo 执行命令
2. 检查用户权限
3. 检查文件/目录权限
```

---

## Coordination with Other Skills

```
需要部署 → [ssh-server-manager] 连接服务器
    ↓
检查环境 → 确认Docker/依赖
    ↓
执行部署 → docker compose up -d
    ↓
验证结果 → 检查容器状态
```

---

## Important Notes

1. **Security**: Never log or display passwords/keys in output
2. **Timeout**: Set appropriate timeout for long-running commands
3. **Connection reuse**: SSH connections are pooled and reused
4. **Cleanup**: Close connections when done with `ssh_connections` action: close_all
5. **Error handling**: Always check exit code and stderr
