# 贡献指南 (Contributing Guide)

欢迎为《末日防线》做出贡献！

## 🎯 如何贡献

### 1. 报告问题 (Bug Report)

发现 Bug？请创建 [Issue](https://github.com/18710111411/last-stand/issues)：
- 使用 Bug Report 模板
- 提供详细复现步骤
- 附上截图/日志 (如有)

### 2. 提出建议 (Feature Request)

有新想法？请创建 [Issue](https://github.com/18710111411/last-stand/issues)：
- 使用 Feature Request 模板
- 描述清楚解决的问题
- 说明优先级

### 3. 提交代码 (Pull Request)

想直接贡献代码？请遵循以下步骤：

```bash
# 1. Fork 仓库
# 2. 克隆到本地
git clone https://github.com/YOUR_USERNAME/last-stand.git
cd last-stand

# 3. 创建分支
git checkout -b feature/your-feature-name

# 4. 进行修改
# 编辑代码，测试功能...

# 5. 提交更改
git add .
git commit -m "feat: 添加 XXX 功能"

# 6. 推送到远程
git push origin feature/your-feature-name

# 7. 创建 Pull Request
# 在 GitHub 上点击 "Compare & pull request"
```

---

## 📝 代码规范

### 命名规范

```csharp
// 类名 - PascalCase
public class TowerManager { }

// 方法名 - PascalCase
public void ShootBullet() { }

// 变量名 - camelCase
private int bulletCount = 0;

// 常量名 - PascalCase 或 UPPER_CASE
public const int MaxHealth = 100;
private readonly float ATTACK_SPEED = 1.0f;

// 私有字段 - 下划线前缀
private Transform _target;
```

### 注释规范

```csharp
/// <summary>
/// 防御塔基类
/// 所有防御塔都应继承此类
/// </summary>
public class Tower : MonoBehaviour {
    /// <summary>
    /// 攻击目标
    /// </summary>
    private Transform target;
    
    /// <summary>
    /// 射击方法
    /// </summary>
    private void Shoot() {
        // 实现代码
    }
}
```

### 提交信息规范

```bash
# 格式：<type>: <description>

# 示例：
feat: 添加冰冻塔减速效果
fix: 修复狙击塔不攻击的问题
docs: 更新 README 文档
style: 格式化代码
refactor: 重构防御塔系统
test: 添加单元测试
chore: 更新依赖库
```

---

## 🎮 开发环境设置

### 前置要求

- Unity 2022 LTS 或更高版本
- Git
- Visual Studio 或 VS Code

### 安装步骤

```bash
# 1. 克隆仓库
git clone https://github.com/18710111411/last-stand.git
cd last-stand

# 2. 用 Unity Hub 打开项目
# 3. Unity 会自动导入资源
# 4. 开始开发！
```

---

## 📋 审查流程

1. **提交 PR** - 创建 Pull Request
2. **自动检查** - GitHub Actions 运行
3. **代码审查** - 维护者审查代码
4. **修改反馈** - 根据审查意见修改
5. **合并** - 审查通过后合并到主分支

---

## 🙏 致谢

感谢所有贡献者！你们让这个游戏变得更好！

---

**💬 有问题？在 [Discussions](https://github.com/18710111411/last-stand/discussions) 中提问！**
