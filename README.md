# MCP-飞书项目管理工具

基于MCP（Model Context Protocol）协议的飞书项目管理工具，允许AI助手通过MCP协议与飞书项目管理系统进行交互。

## 项目简介

本项目是一个MCP服务器实现，它封装了飞书项目管理的Open API，使AI助手能够获取飞书项目的视图列表、视图详情等信息。通过这个工具，AI助手可以帮助用户管理和查询飞书项目中的工作项。

## 使用方法

在支持MCP协议的客户端（如[Claude桌面客户端](https://claude.ai/download),[Cursor](https://www.cursor.com/),[Cline](https://github.com/cline/cline)等）的配置文件中添加本服务器。

> 更多MCP客户端可参考：https://modelcontextprotocol.io/clients

以Claude桌面客户端为例，编辑`claude_desktop_config.json`文件:
- macOS: ~/Library/Application Support/Claude/claude_desktop_config.json
- Windows: %APPDATA%\Claude\claude_desktop_config.json

在`mcpServers`字段中添加以下配置：

```json
{
  "mcpServers": {
    "feishuproj": {
      "command": "uvx",
      "args": ["mcp-feishu-proj","--transport", "stdio"],
      "env": {
        "FS_PROJ_PROJECT_KEY": "your_project_key",
        "FS_PROJ_USER_KEY": "your_user_key",
        "FS_PROJ_PLUGIN_ID": "your_plugin_id",
        "FS_PROJ_PLUGIN_SECRET": "your_plugin_secret"
      }
    }
  }
}
```

## 已支持功能([欢迎贡献](#贡献指南))

### 登录认证
- [x] 登录及认证流程

### 视图功能
- [x] 获取飞书项目视图列表
- [x] 获取视图工作项列表
- [ ] 创建固定视图
- [ ] 更新固定视图
- [ ] 创建条件视图
- [ ] 更新条件视图
- [ ] 删除视图

### 工作项管理
- [ ] 获取工作项详情
- [ ] 获取创建工作项元数据
- [ ] 创建工作项
- [ ] 更新工作项
- [ ] 批量更新工作项字段值
- [ ] 删除工作项
- [ ] 终止/恢复工作项
- [ ] 获取工作项操作记录

### 工作项搜索
- [ ] 获取指定的工作项列表（单空间）
- [ ] 获取指定的工作项列表（跨空间）
- [ ] 获取指定的工作项列表（单空间-复杂传参）
- [ ] 获取指定的工作项列表（全局搜索）
- [ ] 获取指定的关联工作项列表

### 附件管理
- [ ] 添加附件
- [ ] 文件上传
- [ ] 下载附件
- [ ] 删除附件

### 空间管理
- [ ] 获取空间列表
- [ ] 获取空间详情
- [ ] 获取空间下业务线详情
- [ ] 获取空间下工作项类型
- [ ] 获取空间下团队成员

### 用户管理
- [ ] 获取用户详情
- [ ] 搜索租户内的用户列表
- [ ] 创建自定义用户组
- [ ] 更新用户组成员
- [ ] 查询用户组成员

### 空间关联
- [ ] 获取空间关联规则列表
- [ ] 获取空间关联下的关联工作项实例列表
- [ ] 绑定空间关联的关联工作项实例
- [ ] 解绑空间关联的关联工作项实例

### 流程与节点
- [ ] 获取工作流详情
- [ ] 获取工作流详情（WBS）
- [ ] 更新节点/排期
- [ ] 节点完成/回滚
- [ ] 状态流转

### 流程配置
- [ ] 获取工作项下的流程模板列表
- [ ] 获取流程模板配置详情
- [ ] 新增流程模板
- [ ] 更新流程模板
- [ ] 删除流程模板

### 子任务
- [ ] 获取指定的子任务列表
- [ ] 获取子任务详情
- [ ] 创建子任务
- [ ] 更新子任务
- [ ] 子任务完成/回滚
- [ ] 删除子任务

### 评论
- [ ] 添加评论
- [ ] 查询评论
- [ ] 更新评论
- [ ] 删除评论

### 工作项工时
- [ ] 获取工作项的工时记录列表
- [ ] 创建实际工时
- [ ] 更新实际工时
- [ ] 删除实际工时

### 评审管理
- [ ] 批量查询评审意见、评审结论
- [ ] 修改评审结论和评审意见
- [ ] 评审结论标签值查询

### 其他功能
- [ ] 拉机器人入群
- [ ] 获取度量图表明细数据
- [ ] 获取流程角色配置详情


## API文档

本项目包含了飞书项目Open API的Postman集合，位于`docs/open-api-postman`目录下：

- `开放能力环境变量.postman_environment.json`：Postman环境变量配置
- `开放能力open-api接口.postman_collection.json`：Postman API集合

## 开发指南

## 开发环境配置

1. 克隆本仓库：

```bash
git clone https://github.com/yourusername/mcp-feishu-proj.git
cd mcp-feishu-proj
```

2. 安装依赖（使用uv）：

```bash
# 安装uv（如果尚未安装）
pip install uv
# 创建虚拟环境并安装依赖
uv venv
uv pip install -e .
```

## 配置说明

1. 复制环境变量示例文件并进行配置：

```bash
cp .env.example .env
```

2. 编辑`.env`文件，填入以下必要的配置信息：

```
FS_PROJ_BASE_URL=https://project.feishu.cn/
FS_PROJ_PROJECT_KEY=your_project_key
FS_PROJ_USER_KEY=your_user_key
FS_PROJ_PLUGIN_ID=your_plugin_id
FS_PROJ_PLUGIN_SECRET=your_plugin_secret
```

其中：
- `FS_PROJ_BASE_URL`：飞书项目API的基础URL，默认为https://project.feishu.cn/
- `FS_PROJ_PROJECT_KEY`：飞书项目的标识
- `FS_PROJ_USER_KEY`：用户标识
- `FS_PROJ_PLUGIN_ID`：飞书项目Open API的插件ID
- `FS_PROJ_PLUGIN_SECRET`：飞书项目Open API的插件密钥

### 添加新功能

要添加新的飞书项目API功能，请按照以下步骤操作：

1. 在`fsprojclient.py`中添加新的API方法
2. 在`server.py`中使用`@mcp.tool`装饰器注册新的MCP工具

## 贡献指南

欢迎贡献代码、报告问题或提出改进建议。请遵循以下步骤：

1. Fork本仓库
2. 创建您的特性分支 (`git checkout -b feature/amazing-feature`)
3. 提交您的更改 (`git commit -m 'Add some amazing feature'`)
4. 推送到分支 (`git push origin feature/amazing-feature`)
5. 创建一个Pull Request

## 许可证

本项目采用MIT许可证。详情请参阅[LICENSE](LICENSE)文件。
