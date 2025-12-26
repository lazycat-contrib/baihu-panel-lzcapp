# 白虎面板 - 懒猫应用

将白虎面板部署到懒猫云平台的应用包。

## 应用简介

白虎面板是一个轻量级定时任务管理系统，基于 Go + Vue3 构建，内置 Python3、Node.js、Bash 环境，开箱即用。

### 特色功能

- **轻量级**：Docker 部署，无需复杂配置
- **任务调度**：支持标准 Cron 表达式
- **脚本管理**：在线代码编辑器，支持文件上传
- **在线终端**：WebSocket 实时终端
- **环境变量**：安全存储敏感配置
- **现代 UI**：响应式设计，深/浅色主题切换

### 技术特点

- **数据库**：SQLite（默认），无需额外配置
- **内置环境**：Python3、Node.js、Bash
- **性能优化**：相比青龙面板，CPU 占用降低 60%+
- **存储路径**：
  - `/lzcapp/var/data` - 数据目录（包括 SQLite 数据库）
  - `/lzcapp/var/envs` - Python/Node.js 运行环境

## 文件结构

```
baihu-panel-lzcapp/
├── lzc-manifest.yml    # 应用配置清单
├── lzc-build.yml       # 构建配置
├── build.sh            # 自动化发布脚本
├── icon.png            # 应用图标（需要提供）
└── README.md           # 本文件
```

## 快速开始

### 1. 准备图标文件

提供一个 **512x512 的 PNG 格式图标**，命名为 `icon.png`，放在项目根目录。

### 2. 本地测试

```bash
# 构建应用包
./build.sh
# 选择 1 - 构建应用

# 本地安装测试
lzc-cli app install baihu-panel-1.0.0.lpk
```

### 3. 访问应用

安装后访问：`http://baihu.你的懒猫域名`

**默认账号**：`admin` / `123456`

> ⚠️ 首次登录后请立即修改默认密码

## 发布到应用商店

### 方式一：一键发布（推荐）

```bash
# 1. 登录懒猫应用商店
lzc-cli appstore login

# 2. 运行一键发布
./build.sh
# 选择 4 - 一键构建+复制+发布
```

一键发布会自动完成以下步骤：
1. 构建应用包（原始镜像）
2. 复制镜像到懒猫官方仓库
3. 自动更新 manifest 文件
4. 重新构建应用包（新镜像）
5. 发布到应用商店

### 方式二：手动分步操作

```bash
# 1. 登录
lzc-cli appstore login

# 2. 复制镜像
./build.sh
# 选择 2 - 复制镜像到懒猫仓库

# 3. 重新构建
./build.sh
# 选择 1 - 构建应用

# 4. 发布
./build.sh
# 选择 3 - 发布到应用商店
```

### 审核周期

提交后审核时间：**1-3 天**

可在开发者中心查看审核状态。

## 应用配置

### 默认配置（SQLite）

```yaml
environment:
  - BH_SERVER_PORT=8052
  - BH_DB_TYPE=sqlite
  - BH_DB_PATH=/app/data/ql.db
  - BH_DB_TABLE_PREFIX=baihu_
```

### 存储卷

- `/lzcapp/var/data:/app/data` - 数据库和脚本文件
- `/lzcapp/var/envs:/app/envs` - Python/Node.js 虚拟环境

### 资源限制

- CPU: 512 shares
- 内存: 512M

## 更新应用

### 更新步骤

1. **修改版本号**

编辑 `lzc-manifest.yml`：

```yaml
version: 1.0.1  # 从 1.0.0 升级
```

2. **如果镜像有更新**

```bash
# 复制新镜像
lzc-cli appstore copy-image ghcr.io/engigu/baihu:latest

# manifest 会自动更新，重新构建
./build.sh  # 选择 1
```

3. **发布更新**

```bash
./build.sh  # 选择 3 - 发布
```

## 常见问题

### 1. 图标文件格式错误

**错误**：`icon must be PNG format and 512x512`

**解决**：确保图标是 512x512 的 PNG 格式。

### 2. 镜像复制失败

**错误**：`未登录懒猫应用商店`

**解决**：先执行 `lzc-cli appstore login`

### 3. 发布失败

**检查清单**：
- ✅ 已登录应用商店
- ✅ manifest 使用懒猫仓库镜像
- ✅ 版本号已更新（如果是更新）
- ✅ icon.png 存在且格式正确

## 项目信息

- **原项目**：https://github.com/engigu/baihu-panel
- **许可证**：MIT
- **作者**：engigu

## 技术支持

### 白虎面板相关

- GitHub Issues: https://github.com/engigu/baihu-panel/issues
- 演示站点: https://baihu-demo-site.qwapi.eu.org/

### 懒猫云平台

- 开发者文档: https://developer.lazycat.cloud
- 文档仓库: https://gitee.com/lazycatcloud/lzc-developer-doc

## 使用建议

1. **首次部署**：建议先在本地测试，确认应用正常运行
2. **数据备份**：定期备份 `/lzcapp/var/data` 目录
3. **密码安全**：首次登录后立即修改默认密码
4. **资源调整**：根据实际使用调整 CPU 和内存限制

## 更新日志

### v1.0.0 (2025-12-26)
- 初始版本
- 基于 ghcr.io/engigu/baihu:latest
- SQLite 数据库配置
- 内置 Python、Node.js、Bash 环境
