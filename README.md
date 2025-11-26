# ShowDoc MCP Plugin

一个用于 Cursor IDE 的 ShowDoc MCP（Model Context Protocol）插件，让你可以在 Cursor 中直接访问和操作 ShowDoc 文档。

## 功能特性

- 📋 **项目 API 列表**：查看指定项目的所有 API 接口（后端API、服务端API、后端接口、服务端接口）
- 🔍 **文档搜索**：通过关键词搜索 ShowDoc 文档（支持搜索后端API、服务端API、后端接口、服务端接口）
- 📖 **API 详情**：获取指定 API 的详细信息（包括请求参数、响应格式等）

## 安装

### 使用 npx

在 Cursor 的项目下的.cursor/mcp.json中进行以下 MCP 配置：

```json
{
  "mcpServers": {
    "showdoc": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-showdoc",
        "--host", "https://your-showdoc-domain.com",
        "--login_secret_key", "your_login_secret_key",
        "--project_name", "your_project_name"
      ]
    }
  }
}
```

## 配置说明

### 必需参数

- `--host`: ShowDoc 服务器地址（包含协议，如 `https://www.showdoc.com.cn`）
- `--login_secret_key`: ShowDoc 登录密钥（在 ShowDoc 用户设置中获取）
- `--project_name`: 项目名称。将锁定该项目，后续所有 API 调用只能访问该项目的资源，防止访问其他项目

### 可选参数

- `--username`: ShowDoc 用户名 (默认为mcp-showdoc用户, 对此用户给项目的可读权限)
- `--debug`: 启用调试模式，输出详细日志

**配置示例**：
```json
{
  "mcpServers": {
    "showdoc": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-showdoc",
        "--host", "https://your-showdoc-domain.com",
        "--login_secret_key", "your_login_secret_key",
        "--project_name", "项目名"
      ]
    }
  }
}
```

## 使用方法

### 在 Cursor 中使用

配置完成后，重启 Cursor，然后你可以在对话中：

1. **搜索文档**：
   - "在 ShowDoc 中搜索用户注册API"
   - "搜索包含关键词的API文档"

2. **查看项目 API 列表**：
   - "列出项目的后端接口"
   - "显示所有服务端API"
   - "将项目中的所有API接口及接口名保存到api.md文件中"

3. **查看 API 详情**：
   - "显示服务端接口的登录详情"
   - "查看登录api的详情"

4. **生成模型**:
   - "生成XXX的API接口模型类"
### 命令行调试

项目提供了命令行调试工具，可以直接调用 ShowDoc API：

#### 1. 获取项目信息

```bash
node dist/index.js debug --action info \
  --host https://your-showdoc-domain.com \
  --login_secret_key your_key \
  --username your_username \
  --project_name xxxx
```

#### 2. 获取页面内容

```bash
node dist/index.js debug --action api-detail \
  --host https://your-showdoc-domain.com \
  --login_secret_key your_key \
  --username your_username \
  --project_name xxxx \
  --page 268
```

#### 3. 搜索文档

```bash
node dist/index.js debug --action search \
  --host https://your-showdoc-domain.com \
  --login_secret_key your_key \
  --username your_username \
  --project_name xxxx \
  --keyword "用户注册"
```

#### 查看帮助信息

```bash
node dist/index.js --help
node dist/index.js debug --help
```

## MCP 能力

### Resources（资源）

MCP Resources 允许 Cursor 访问 ShowDoc 中的项目和页面：

- **列出资源**：自动列出所有可用的 ShowDoc 项目和页面
  - URI 格式：`showdoc://item/{item_id}/page/{page_id}`
  - 资源列表缓存 5 分钟，提高性能

- **读取资源**：根据 URI 读取具体的页面内容
  - 返回 Markdown 格式的页面内容
  - 自动包含页面标题和内容

## 系统要求

- **Node.js**: >= 20
- **Cursor IDE**: 最新版本


## 常见问题

### Q: 如何知道 MCP 是否正常工作？

A: 在 Cursor 中尝试询问 "列出我的 ShowDoc 项目"，如果能看到项目列表，说明配置成功。也可以查看 Cursor 的 MCP 日志。

### Q: 登录失败怎么办？

A: 检查以下几点：
1. `--host` 参数是否正确（包含协议）
2. `--login_secret_key` 是否正确（在 ShowDoc 用户设置中查看）
4. 网络连接是否正常
5. 启用 `--debug` 查看详细错误信息

### Q: 如何更新插件？

A: 
- 如果使用 npx：`npx -y mcp-showdoc` 会自动使用最新版本


## 许可证

ISC

## 相关链接

- [ShowDoc 官方文档](https://www.showdoc.com.cn/)
- [MCP 协议文档](https://modelcontextprotocol.io/)
- [Cursor IDE](https://cursor.sh/)
