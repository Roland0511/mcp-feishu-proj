[![MseeP.ai Security Assessment Badge](https://mseep.net/pr/roland0511-mcp-feishu-proj-badge.png)](https://mseep.ai/app/roland0511-mcp-feishu-proj)

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
      "args": ["mcp-feishu-proj@latest","--transport", "stdio"],
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
- [x] 获取工作项详情
- [x] 获取创建工作项元数据
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

### 角色与人员配置
- [x] 获取流程角色配置详情

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


### 其他功能
- [ ] 拉机器人入群
- [ ] 获取度量图表明细数据
- [ ] 获取流程角色配置详情




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


### 飞书项目Open API参考

本项目包含了飞书项目Open API的Postman集合，位于`docs/open-api-postman`目录下，将目录下文件导入Postman可以进行快速调试飞书项目接口：

- `postman_environment.json`：Postman环境变量配置
- `postman_collection.json`：Postman API集合


## 容器化部署指南

### Docker部署

本项目提供了Docker部署支持，可以通过Docker容器运行MCP飞书项目服务。

#### 前提条件

- 安装 [Docker](https://docs.docker.com/get-docker/)
- 安装 [Docker Compose](https://docs.docker.com/compose/install/)

#### 使用Docker Compose运行

1. 创建`.env`文件，设置必要的环境变量

```bash
cp .env.example .env
```

然后编辑`.env`文件，填入你的飞书项目相关信息：

```
FS_PROJ_BASE_URL=https://project.feishu.cn/
FS_PROJ_PROJECT_KEY=your_project_key
FS_PROJ_USER_KEY=your_user_key
FS_PROJ_PLUGIN_ID=your_plugin_id
FS_PROJ_PLUGIN_SECRET=your_plugin_secret
```

2. 使用Docker Compose启动服务

```bash
docker-compose -f docker/docker-compose.yml up -d
```

这将使用`ghcr.io/astral-sh/uv`镜像，并挂载项目根目录到容器中，直接运行本地代码，便于开发和调试。Docker Compose会自动加载项目根目录中的`.env`文件作为环境变量。

3. 查看日志

```bash
docker-compose -f docker/docker-compose.yml logs -f
```

4. 停止服务

```bash
docker-compose -f docker/docker-compose.yml down
```

更多详细信息请参阅[Docker部署文档](docker/docker-README.md)。

### Kubernetes部署

#### 前提条件

- 一个可用的Kubernetes集群
- 已安装kubectl命令行工具
- 具有创建Deployment、ConfigMap和Secret的权限

#### 部署步骤

1. 准备Secret

首先，需要创建包含敏感信息的Secret。由于Kubernetes Secret需要使用base64编码的值，您需要对敏感信息进行编码：

```bash
# 对敏感信息进行base64编码
echo -n "your_project_key" | base64
echo -n "your_user_key" | base64
echo -n "your_plugin_id" | base64
echo -n "your_plugin_secret" | base64
```

然后，使用生成的base64编码值更新`k8s-secret.yaml`文件中的相应字段。

2. 应用配置

依次应用以下配置文件：

```bash
# 创建ConfigMap
kubectl apply -f k8s-configmap.yaml

# 创建Secret
kubectl apply -f k8s-secret.yaml

# 创建Deployment
kubectl apply -f k8s-deployment.yaml
```

3. 验证部署

检查部署状态：

```bash
# 查看Deployment状态
kubectl get deployments

# 查看Pod状态
kubectl get pods

# 查看Pod日志
kubectl logs -f <pod-name>
```

更多详细信息请参阅[Kubernetes部署文档](k8s/k8s-README.md)。



## 贡献指南

欢迎贡献代码、报告问题或提出改进建议。请遵循以下步骤：

1. Fork本仓库
2. 创建您的特性分支 (`git checkout -b feature/amazing-feature`)
3. 提交您的更改 (`git commit -m 'Add some amazing feature'`)
4. 推送到分支 (`git push origin feature/amazing-feature`)
5. 创建一个Pull Request

## 许可证

本项目采用MIT许可证。详情请参阅[LICENSE](LICENSE)文件。

