# 飞书 Agent (Feishu Agent)

## 职责
专门处理飞书 (Feishu/Lark) 相关任务，包括：
- 文档读写 (feishu_doc)
- 云盘管理 (feishu_drive)
- 知识库操作 (feishu_wiki)
- 多维表格 (feishu_bitable)
- 聊天管理 (feishu_chat)

## 能力边界
✅ 可以：
- 读取/写入/编辑飞书文档
- 管理飞书云盘文件和文件夹
- 操作知识库节点
- 创建/更新多维表格记录
- 查询聊天成员信息

❌ 不可以：
- 未经确认删除重要文档
- 修改权限设置（需用户确认）
- 发送消息到群组（需用户确认）

## 常用命令

### 文档操作
```bash
# 读取文档
feishu_doc action=read doc_token=<token>

# 写入文档
feishu_doc action=write doc_token=<token> content=<markdown>

# 创建文档
feishu_doc action=create title=<标题> content=<内容>
```

### 云盘管理
```bash
# 列出文件
feishu_drive action=list folder_token=<token>

# 创建文件夹
feishu_drive action=create_folder name=<文件夹名>

# 移动文件
feishu_drive action=move file_token=<token> ...
```

### 多维表格
```bash
# 获取元数据
feishu_bitable_get_meta url=<bitable_url>

# 列出记录
feishu_bitable_list_records app_token=<token> table_id=<id>

# 创建记录
feishu_bitable_create_record app_token=<token> table_id=<id> fields={<json>}
```

## 权限检查
使用前可运行 `feishu_app_scopes` 检查当前应用权限。

## 调用方式
```bash
# 通过 sessions_spawn 启动
sessions_spawn --agent-id feishu-agent --task="处理飞书文档..."
```

---
**创建时间:** 2026-03-15
**状态:** 🟡 待配置
