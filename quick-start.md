# MCP快速入门指南

本指南将帮助您在5分钟内开始使用Model Context Protocol (MCP)，特别是在Windsurf环境中的配置和应用。作为初学者，本教程将从基础概念到实际应用，为您提供全面的指导。

## 前提条件

在开始之前，请确保您已经：

1. 安装了支持MCP的客户端（如Claude Desktop、Windsurf IDE等）
2. 了解基本的命令行操作
3. 有Node.js环境（用于运行MCP服务器）

## 第1步：安装MCP服务器

MCP服务器是提供工具和资源给LLM的核心组件。最常用的MCP服务器包括：

```bash
# 文件系统服务器 - 允许LLM访问您的文件系统
npm install -g @modelcontextprotocol/server-filesystem

# GitHub服务器 - 允许LLM访问GitHub仓库
npm install -g @modelcontextprotocol/server-github
```

## 第2步：配置MCP服务器

### 文件系统服务器配置

在Windsurf/Claude Desktop中，您需要编辑配置文件来启用MCP服务器。配置文件通常位于：

- Windows: `C:\Users\<用户名>\AppData\Roaming\Claude\claude_desktop_config.json`
- macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
- Linux: `~/.config/Claude/claude_desktop_config.json`

添加以下配置到JSON文件中：

```json
{
  "servers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "C:\\Users\\<用户名>\\Desktop",
        "C:\\Users\\<用户名>\\Documents"
      ]
    }
  }
}
```

这将允许Claude访问您的桌面和文档文件夹。您可以根据需要添加或移除路径。

### GitHub服务器配置

要启用GitHub访问，添加以下配置：

```json
{
  "servers": {
    "filesystem": { ... },
    "github": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-github"
      ],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "您的GitHub个人访问令牌"
      }
    }
  }
}
```

您需要从GitHub获取个人访问令牌，并确保它具有适当的权限（如repo、read:user等）。

## 第3步：在Windsurf中启用MCP

Windsurf是一个支持MCP的IDE环境，它允许您与LLM进行交互，同时提供文件访问和工具调用功能。

1. 启动Windsurf
2. 打开设置面板（通常通过点击右上角的齿轮图标）
3. 导航到"AI助手"或"MCP"设置部分
4. 确保MCP选项已启用
5. 如果需要，指定MCP配置文件的路径

## 第4步：测试MCP连接

要测试MCP是否正常工作，您可以尝试以下操作：

1. 在Windsurf中打开AI助手面板
2. 输入一个需要文件系统访问的请求，例如：
   ```
   请列出我桌面上的文件
   ```
3. 如果配置正确，AI助手应该能够列出您桌面上的文件

## 第5步：使用MCP功能

一旦MCP配置完成，您可以使用以下功能：

### 文件操作

```
请读取文件 C:\Users\<用户名>\Desktop\example.txt
```

```
请创建一个新文件，内容为Hello World
```

### GitHub操作

```
请列出我的GitHub仓库
```

```
请查看仓库 username/repo 中的README.md文件
```

## 常见问题解决

### MCP服务器无法启动

检查：
- Node.js是否正确安装
- 配置文件格式是否正确
- 指定的路径是否存在

### 权限错误

检查：
- 文件系统路径是否有正确的访问权限
- GitHub令牌是否有足够的权限
- 配置文件是否正确保存

### 连接问题

检查：
- MCP服务器是否正在运行
- 客户端是否正确配置为使用MCP
- 防火墙设置是否阻止了连接

## 高级配置

### 自定义MCP服务器

您可以创建自己的MCP服务器来提供自定义功能：

```javascript
const { createServer } = require('@modelcontextprotocol/server');

// 创建服务器
const server = createServer();

// 注册工具
server.registerTool({
  name: 'hello',
  description: '返回问候消息',
  inputSchema: {
    type: 'object',
    properties: {
      name: {
        type: 'string',
        description: '要问候的名称'
      }
    }
  },
  handler: async (params) => {
    return `Hello, ${params.name || 'World'}!`;
  }
});

// 启动服务器
server.listen();
```

### 多服务器配置

您可以同时运行多个MCP服务器，每个服务器提供不同的功能：

```json
{
  "servers": {
    "filesystem": { ... },
    "github": { ... },
    "custom": {
      "command": "node",
      "args": ["path/to/your/custom-server.js"]
    }
  }
}
```

## 下一步

一旦您熟悉了MCP的基本用法，可以尝试：

1. 创建自定义MCP服务器提供特定功能
2. 探索更多MCP客户端和工具
3. 将MCP集成到您的工作流程中
4. 贡献到MCP开源社区

## 资源

- [MCP官方文档](https://modelcontextprotocol.info/docs/)
- [Windsurf文档](https://windsurf.ai/docs/)
- [Claude Desktop指南](https://claude.ai/docs/desktop)
- [MCP GitHub仓库](https://github.com/modelcontextprotocol/)