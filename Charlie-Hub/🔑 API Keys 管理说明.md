---
tags: [charlie-hub, auto-sync]
updated: 2026-04-17 05:03:19
source: /home/charlie/.config/api-keys/README.md
---

# API Keys 集中式管理

## 文件说明

**两个配置文件**：
- `env` - Shell 格式（带 export），用于 .bashrc 加载
- `systemd.env` - Systemd 格式（无 export），用于 systemd 服务

**权限**: `600` (仅本人可读写)
**用途**: 存储所有 API Keys、Tokens 和敏感配置

## 安全措施

✅ **已启用保护**:
1. 文件权限 `600` - 仅 charlie 可读写
2. 目录权限 `700` - 仅 charlie 可访问
3. Git 全局忽略 - 不会被提交到仓库
4. 集中式管理 - 避免分散存储

## 使用方式

### Shell 环境加载

在 `~/.bashrc` 中已自动添加：
```bash
[ -f ~/.config/api-keys/env ] && source ~/.config/api-keys/env
```

新开终端自动加载所有 API Keys。

### Systemd 服务加载

Discord Bot 等服务通过 `EnvironmentFile` 加载：
```ini
[Service]
EnvironmentFile=-%h/.config/api-keys/env
```

## 添加新 API Key

编辑 `~/.config/api-keys/env`：
```bash
vim ~/.config/api-keys/env
```

添加格式：
```bash
export YOUR_API_KEY="key-value-here"
```

重新加载环境：
```bash
source ~/.config/api-keys/env
```

重启相关服务：
```bash
systemctl --user restart discord-bot.service
```

## 当前管理的 Keys

- `DISCORD_BOT_TOKEN` - Discord Bot 认证
- `DEEPSEEK_API_KEY` - DeepSeek AI API
- `GEMINI_API_KEY` - Google Gemini API
- `OPENAI_API_KEY` - OpenAI API (可选)
- `AGI_DB_PATH` - AGI Brain 数据库路径
- `HTTPS_PROXY` / `HTTP_PROXY` - 代理配置

## 备份建议

```bash
# 加密备份（需要 gpg）
gpg -c ~/.config/api-keys/env

# 备份到安全位置
cp ~/.config/api-keys/env.gpg /path/to/secure/location/
```

## 恢复方法

```bash
# 从加密备份恢复
gpg -d /path/to/backup/env.gpg > ~/.config/api-keys/env
chmod 600 ~/.config/api-keys/env
```

## 安全检查

验证权限：
```bash
ls -la ~/.config/api-keys/
# 应显示 drwx------ (700) 和 -rw------- (600)
```

检查是否被 Git 追踪：
```bash
cd ~ && git check-ignore -v .config/api-keys/env
# 应显示匹配到 .gitignore 规则
```

---

**创建时间**: 2026-04-07
**维护者**: Charlie
**会话 ID**: a8efcb99-6e68-48d3-9318-6f453f05fd3e
