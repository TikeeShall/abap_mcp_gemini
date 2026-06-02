# mcp-abap-adt: 您的 SAP ABAP 开发工具 (ADT) 门户

本项目源自 [mario-andreschak/mcp-abap-adt](https://github.com/mario-andreschak/mcp-abap-adt) 的修改与增强，专门针对 **Gemini**、**钉钉** 等集成应用进行了优化。

本项目提供了一个服务器，允许您使用 Model Context Protocol (MCP) 与 SAP ABAP 系统进行交互。它就像是一个桥梁，让 Gemini 或 钉钉悟空 等工具能够直接与您的 ABAP 系统“对话”，获取源代码、表结构等信息。

<a href="https://glama.ai/mcp/servers/gwkh12xlu7">
  <img width="380" height="200" src="https://glama.ai/mcp/servers/gwkh12xlu7/badge" alt="ABAP ADT MCP server" />
</a>

本指南旨在帮助您快速入门，我们将分步骤介绍：

1.  **前提条件**：开始前的准备工作。
2.  **安装与设置**：环境配置。
3.  **运行服务器**：如何启动服务。
4.  **工具集成**：连接到 Cline 或其他 MCP 客户端。
5.  **故障排除**：常见问题及解决方案。
6.  **可用工具**：可调用的命令列表。

## 1. 前提条件

在开始之前，您需要准备：

*   **SAP ABAP 系统**：
    *   系统的 URL（例如：`https://my-sap-system.com:8000`）。
    *   有效的用户名和密码。
    *   SAP 客户端编号（Client，例如：`100`）。
    *   确保系统已激活 ADT 服务（通常是 `/sap/bc/adt`）。
    *   对于 `GetTableContents` 工具，需要部署自定义服务 `/z_mcp_abap_adt/z_tablecontent`。

*   **Node.js 和 npm**：
    *   建议安装最新的 **LTS** 版本。

## 2. 安装与设置

### 2.1 克隆仓库
```bash
git clone https://github.com/TikeeShall/abap_mcp_gemini.git
cd abap_mcp_gemini
```

### 2.2 安装依赖
```bash
npm install
```

### 2.3 构建项目
```bash
npm run build
```

### 2.4 配置 `.env` 文件
这是存储敏感信息的关键步骤：
1.  在根目录下创建名为 `.env` 的文件。
2.  添加以下内容（将占位符替换为您的实际信息）：
    **注意：如果您的密码包含 "#" 字符，请务必用引号括起来！**
    ```
    SAP_URL=https://your-sap-system.com:8000
    SAP_USERNAME=your_username
    SAP_PASSWORD=your_password
    SAP_CLIENT=100
    ```
    **重要提示：切勿共享您的 `.env` 文件，也不要将其提交到 Git 仓库！**

## 3. 运行服务器

### 3.1 标准模式
```bash
npm run start
```

### 3.2 调试模式（带 Inspector）
```bash
npm run dev
```
启动后会输出一个 URL（通常是 `http://localhost:5173`），您可以在浏览器中打开它进行交互式调试。

## 4. 工具集成

### Cline (VS Code 扩展)
1. 在 VS Code 中打开 `cline_mcp_settings.json`。
2. 添加以下配置：
```json
{
  "mcpServers": {
    "mcp-abap-adt": {
      "command": "node",
      "args": ["C:/你的路径/abap_mcp_gemini/dist/index.js"],
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

## 5. 故障排除
*   **连接错误**：检查 `.env` 中的凭据是否正确，以及 SAP 系统是否允许外部 ADT 连接。
*   **CSRF 令牌失败**：本项目已针对 CSRF 校验进行了优化，如果仍报错，请检查用户权限。

## 6. 可用工具 (Available Tools)

本服务器提供以下工具，可通过 Gemini 或其他 MCP 客户端调用：

| 工具名称 | 描述 | 输入参数 | 示例用法 |
| :--- | :--- | :--- | :--- |
| `GetProgram` | 获取 ABAP 程序源代码。 | `program_name` (string) | `@tool GetProgram program_name=ZMY_PROGRAM` |
| `GetClass` | 获取 ABAP 类源代码。 | `class_name` (string) | `@tool GetClass class_name=ZCL_MY_CLASS` |
| `GetFunctionGroup` | 获取 ABAP 函数组源代码。 | `function_group` (string) | `@tool GetFunctionGroup function_group=ZMY_FG` |
| `GetFunction` | 获取 ABAP 函数模块源代码。 | `function_name` (string), `function_group` (string) | `@tool GetFunction function_name=ZMY_FM` |
| `GetStructure` | 获取 ABAP 结构信息。 | `structure_name` (string) | `@tool GetStructure structure_name=ZMY_STRUCT` |
| `GetTable` | 获取 ABAP 数据库表结构。 | `table_name` (string) | `@tool GetTable table_name=ZMY_TABLE` |
| `GetTableContents` | 获取 ABAP 表内容数据。 | `table_name` (string), `max_rows` (number) | `@tool GetTableContents table_name=ZMY_TABLE` |
| `GetPackage` | 获取 ABAP 开发包详情。 | `package_name` (string) | `@tool GetPackage package_name=ZMY_PACKAGE` |
| `GetTypeInfo` | 获取 ABAP 类型信息。 | `type_name` (string) | `@tool GetTypeInfo type_name=ZMY_TYPE` |
| `GetInclude` | 获取 ABAP 包含程序源代码。 | `include_name` (string) | `@tool GetInclude include_name=ZMY_INCLUDE` |
| `SearchObject` | 全局搜索 ABAP 对象。 | `query` (string) | `@tool SearchObject query=ZMY*` |
| `GetInterface` | 获取 ABAP 接口源代码。 | `interface_name` (string) | `@tool GetInterface interface_name=ZIF_MY_INT` |

## 致谢 (Credits)

本项目基于并引用了 [mario-andreschak/mcp-abap-adt](https://github.com/mario-andreschak/mcp-abap-adt) 的工作，并针对现代 AI 助手集成的需求进行了适配。
