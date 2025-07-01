# 使用 GitHub Container Registry 部署 LibreKB

## 概述

本项目已配置 GitHub Actions 自动构建和发布 Docker 镜像到 GitHub Container Registry (ghcr.io)。每次推送到主分支或创建标签时，都会自动构建新的镜像。

## 可用镜像标签

- `ghcr.io/[your-username]/librekb:latest` - 最新的主分支构建
- `ghcr.io/[your-username]/librekb:main` - 主分支构建
- `ghcr.io/[your-username]/librekb:v1.0.0` - 特定版本标签（如果有）

## 快速部署

### 1. 创建 docker-compose.yml 文件

```yaml
version: '3.8'

services:
  web:
    image: ghcr.io/[your-username]/librekb:latest
    container_name: librekb_web
    ports:
      - "80:80"
    depends_on:
      - db
    environment:
      MYSQL_DATABASE: ${MYSQL_DATABASE}
      MYSQL_USER: ${MYSQL_USER}
      MYSQL_PASSWORD: ${MYSQL_PASSWORD}
    volumes:
      - ./config.php:/var/www/html/config.php

  db:
    image: mysql:8.1
    container_name: librekb_db
    environment:
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
      MYSQL_DATABASE: ${MYSQL_DATABASE}
      MYSQL_USER: ${MYSQL_USER}
      MYSQL_PASSWORD: ${MYSQL_PASSWORD}
    volumes:
      - librekb_db_data:/var/lib/mysql

volumes:
  librekb_db_data:
    name: librekb_db_data
```

### 2. 创建 .env 文件

```bash
MYSQL_ROOT_PASSWORD=your_root_password
MYSQL_DATABASE=librekb
MYSQL_USER=librekb_user
MYSQL_PASSWORD=securepassword
```

### 3. 创建配置文件

从项目仓库下载 `config.docker-example.php` 并重命名为 `config.php`，然后根据需要修改数据库连接设置。

### 4. 启动服务

```bash
docker-compose up -d
```

### 5. 安装 LibreKB

1. 打开浏览器访问 `http://localhost/install.php`
2. 按照安装向导创建管理员用户
3. 安装完成后删除安装文件：
   ```bash
   docker-compose exec web rm /var/www/html/install.php /var/www/html/update.php
   ```

## 私有仓库访问

如果您的仓库是私有的，需要先登录到 GitHub Container Registry：

```bash
echo $GITHUB_TOKEN | docker login ghcr.io -u USERNAME --password-stdin
```

其中：
- `GITHUB_TOKEN` 是您的 GitHub Personal Access Token（需要 `read:packages` 权限）
- `USERNAME` 是您的 GitHub 用户名

## 自动构建说明

GitHub Actions 工作流会在以下情况下自动构建镜像：

1. **推送到主分支** - 创建 `latest` 和 `main` 标签
2. **创建版本标签** - 创建对应的版本标签（如 `v1.0.0`）
3. **Pull Request** - 创建 PR 标签用于测试

构建的镜像支持多架构：
- `linux/amd64` (x86_64)
- `linux/arm64` (ARM64)

## 故障排除

### 镜像拉取失败
1. 确认镜像名称和标签正确
2. 检查是否有访问权限（私有仓库需要认证）
3. 确认网络连接正常

### 容器启动失败
1. 检查 `.env` 文件配置
2. 确认 `config.php` 文件存在且配置正确
3. 查看容器日志：`docker-compose logs web`

## 更新镜像

要更新到最新版本：

```bash
docker-compose pull
docker-compose up -d
```