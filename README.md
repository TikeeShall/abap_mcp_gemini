# mcp-abap-adt: 您的 SAP ABAP 开发工具 (ADT) 门户

本项目源自 [mario-andreschak/mcp-abap-adt](https://github.com/mario-andreschak/mcp-abap-adt) 的修改与增强，专为 **Gemini**、**钉钉悟空** 等 AI 与办公集成应用进行了深度优化。

本项目提供了一个基于 Model Context Protocol (MCP) 的服务器，充当 AI 工具与 SAP ABAP 系统之间的桥梁。通过它，Gemini 或钉钉悟空能够直接读取您的 ABAP 系统，获取源代码、表结构、包详情等信息。

## 1. 前提条件

在开始之前，您需要准备：

*   **SAP ABAP 系统**：
    *   系统的 URL（例如：`https://my-sap-system.com:8000`）。
    *   有效的用户名和密码。
    *   SAP 客户端编号（Client，例如：`100`）。
    *   确保系统已激活 ADT 服务（通常是 `/sap/bc/adt`）。
    *   可选项，对于 `GetTableContents` 工具，需要部署自定义服务 `/z_mcp_abap_adt/z_tablecontent`。

*   **Node.js 和 npm**：
    *   建议安装最新的 **LTS** 版本。

## 2. 安装与设置

### 2.1 克隆仓库
```bash
git clone https://github.com/TikeeShall/abap_mcp_gemini.git
cd abap_mcp_gemini
```

### 2.2 配置 `.env` 文件
这是存储敏感信息的关键步骤：
1.  在克隆后的根目录下创建名为 `.env` 的文件，可参照.env.example。
2.  添加以下内容（将占位符替换为您的实际信息）：
    **注意：如果您的密码包含 "#" 字符，请务必用引号括起来！**
    ```
    SAP_URL=https://your-sap-system.com:8000
    SAP_USERNAME=your_username
    SAP_PASSWORD=your_password
    SAP_CLIENT=100
    ```
    **重要提示：切勿共享您的 `.env` 文件，也不要将其提交到 Git 仓库！**

## 3. Gemini中需要需要提示它安排即可

## 4. 钉钉悟空
确保 .env 环境变量已正确配置。打开钉钉悟空，依次点击：设置 -> MCP 服务 -> 添加 MCP。
按照以下说明填写表单：
配置项x填写内容
类型,STDIO
名称,abap-adt-mcp
命令,node
额外参数,本地编译后的文件绝对路径，例如：C:\SAP_Dependency\mcp-abap-adt-gemini\dist\index.js

## 5. 可用工具 (Available Tools)

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

本项目基于并修改自 mario-andreschak/mcp-abap-adt。感谢原作者的开源贡献，本项目在此基础上针对现代 AI 助手集成的实际落地场景做了进一步适配。
