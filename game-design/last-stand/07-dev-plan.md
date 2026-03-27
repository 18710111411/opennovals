# 末日防线 (Last Stand) - 游戏开发计划

## 📋 项目概述

| 属性 | 说明 |
|------|------|
| **项目名称** | 末日防线 (Last Stand) |
| **游戏类型** | 塔防 + 探索双模式 |
| **目标平台** | 移动端 (iOS/Android) + PC (Steam) |
| **开发引擎** | Unity / Godot / Unreal |
| **开发语言** | C# / C++ |
| **文档版本** | v1.0 (2026-03-15) |

---

## 🎯 开发阶段

### 阶段 1: MVP (最小可行产品) - 4 周

**目标：** 核心玩法验证

| 周次 | 任务 | 交付物 |
|------|------|--------|
| **第 1 周** | 项目搭建，基础框架 | 可运行空项目 |
| **第 2 周** | 防御塔系统 (3 种塔) | 机枪/狙击/散弹塔 |
| **第 3 周** | 丧尸系统 (3 种丧尸) | 普通/奔跑者/胖子 |
| **第 4 周** | 第 1-5 关 playable | 可玩 demo |

**MVP 功能清单：**
- [ ] 基础防御塔建造
- [ ] 丧尸生成与寻路
- [ ] 金币系统
- [ ] 第 1-5 关关卡
- [ ] 基础 UI

---

### 阶段 2: Alpha - 8 周

**目标：** 完整核心循环

| 周次 | 任务 | 交付物 |
|------|------|--------|
| **第 5-6 周** | 探索模式 | 可探索废墟 |
| **第 7-8 周** | 英雄系统 (5 英雄) | 雷克斯/艾琳等 |
| **第 9-10 周** | 第一章完整 (10 关) | 第一章可通关 |
| **第 11-12 周** | 动态任务系统 | NPC 竞争机制 |

**Alpha 功能清单：**
- [ ] 塔防 + 探索双模式
- [ ] 5 个核心英雄
- [ ] 12 种防御塔
- [ ] 9 种丧尸
- [ ] 动态任务系统
- [ ] 第一章完整剧情

---

### 阶段 3: Beta - 12 周

**目标：** 完整游戏内容

| 周次 | 任务 | 交付物 |
|------|------|--------|
| **第 13-16 周** | 第二 - 四章内容 | 40 关完整 |
| **第 17-20 周** | 双结局系统 | 真结局解锁 |
| **第 21-24 周** | 优化与打磨 | 性能优化 |

**Beta 功能清单：**
- [ ] 40 关主线剧情
- [ ] 双结局系统
- [ ] 支线任务
- [ ] 成就系统
- [ ] 美术资源完善
- [ ] 音效音乐

---

### 阶段 4: 发布准备 - 4 周

**目标：** 上线准备

| 周次 | 任务 | 交付物 |
|------|------|--------|
| **第 25-26 周** | 测试与修复 | Bug 修复 |
| **第 27-28 周** | 商店页面 | Steam/应用商店 |

---

## 🛠️ 技术架构

### 核心系统

```
┌─────────────────────────────────┐
│         游戏核心层              │
├─────────────────────────────────│
│  GameManager                    │
│  SceneManager                   │
│  SaveSystem                     │
└─────────────────────────────────┘
              ↓
┌─────────────────────────────────┐
│         玩法系统层              │
├─────────────────────────────────│
│  TowerSystem     EnemySystem    │
│  HeroSystem      QuestSystem    │
│  EconomySystem   SkillSystem    │
└─────────────────────────────────┘
              ↓
┌─────────────────────────────────┐
│         表现层                  │
├─────────────────────────────────│
│  UIManager       AudioManager   │
│  VFXSystem       CameraSystem   │
└─────────────────────────────────┘
```

### 数据驱动设计

```
数据表 (JSON/ScriptableObject)
├── towers.json      - 防御塔数据
├── enemies.json     - 丧尸数据
├── heroes.json      - 英雄数据
├── quests.json      - 任务数据
├── levels.json      - 关卡配置
└── story.json       - 剧情文本
```

---

## 📁 项目结构

```
LastStand/
├── Assets/
│   ├── Scripts/
│   │   ├── Core/           # 核心系统
│   │   ├── Gameplay/       # 玩法系统
│   │   ├── UI/             # UI 系统
│   │   └── Data/           # 数据定义
│   ├── Art/
│   │   ├── Characters/     # 角色模型
│   │   ├── Towers/         # 防御塔模型
│   │   ├── Enemies/        # 丧尸模型
│   │   └── Environment/    # 场景资源
│   ├── Audio/
│   │   ├── BGM/            # 背景音乐
│   │   ├── SFX/            # 音效
│   │   └── Voice/          # 语音
│   └── Resources/
│       ├── Data/           # 数据表
│       └── Config/         # 配置文件
├── Docs/
│   ├── 游戏设定集/         # 本文档
│   ├── 技术设计/           # 技术文档
│   └── 美术设定/           # 美术文档
└── README.md
```

---

## 🎮 核心玩法实现

### 防御塔系统

```csharp
// Tower.cs - 防御塔基类
public class Tower : MonoBehaviour {
    public string towerId;
    public int level;
    public float range;
    public float damage;
    public float attackSpeed;
    
    private Transform target;
    private float fireCountdown = 0f;
    
    void Update() {
        UpdateTarget();
        if (target != null && fireCountdown <= 0f) {
            Shoot();
            fireCountdown = 1f / attackSpeed;
        }
        fireCountdown -= Time.deltaTime;
    }
    
    void UpdateTarget() {
        // 寻找范围内最近的敌人
    }
    
    void Shoot() {
        // 发射子弹
    }
}
```

### 丧尸寻路系统

```csharp
// Enemy.cs - 丧尸 AI
public class Enemy : MonoBehaviour {
    public float speed;
    public int health;
    public int damage;
    
    private Transform target; // 基地位置
    private float pathUpdateTimer = 0f;
    
    void Update() {
        Move();
        Attack();
    }
    
    void Move() {
        // 沿路径移动到基地
    }
    
    void Attack() {
        // 攻击防御塔或基地
    }
}
```

### 动态任务系统

```csharp
// QuestSystem.cs - 任务系统
public class QuestSystem : MonoBehaviour {
    public List<Quest> activeQuests;
    public List<Faction> factions; // NPC 势力
    
    void Update() {
        foreach (var quest in activeQuests) {
            if (quest.isDynamic) {
                CheckFactionProgress(quest);
            }
            CheckTimeout(quest);
        }
    }
    
    void CheckFactionProgress(Quest quest) {
        // 检查 NPC 势力进度
        // 如果 NPC 先完成，玩家任务失败
    }
    
    void CompleteQuest(Quest quest, string completer) {
        if (completer == "player") {
            // 玩家获得全部奖励
        } else {
            // 玩家无奖励，任务关闭
        }
    }
}
```

---

## 📊 数据表设计

### 防御塔数据表

| 字段 | 类型 | 说明 |
|------|------|------|
| towerId | string | 唯一标识 |
| name | string | 塔名称 |
| cost | int | 建造成本 |
| damage | int | 伤害值 |
| range | float | 射程 |
| attackSpeed | float | 攻速 |
| upgradePath | string[] | 升级路线 |

### 丧尸数据表

| 字段 | 类型 | 说明 |
|------|------|------|
| enemyId | string | 唯一标识 |
| name | string | 丧尸名称 |
| health | int | 血量 |
| speed | float | 速度 |
| damage | int | 伤害 |
| abilities | string[] | 特殊能力 |

### 任务数据表

| 字段 | 类型 | 说明 |
|------|------|------|
| questId | string | 唯一标识 |
| title | string | 任务标题 |
| type | enum | 任务类型 |
| isDynamic | bool | 是否可竞争 |
| timeout | int | 倒计时 (秒) |
| rewards | object | 奖励数据 |
| competitors | string[] | 竞争势力 |

---

## 🎨 美术资源清单

### 防御塔模型 (12 种)

- [ ] 机枪塔
- [ ] 狙击塔
- [ ] 散弹塔
- [ ] 火焰塔
- [ ] 冰冻塔
- [ ] 电击塔
- [ ] 路障
- [ ] 修复塔
- [ ] 雷达塔
- [ ] 补给塔
- [ ] 导弹塔
- [ ] 激光塔

### 丧尸模型 (13 种)

- [ ] 普通感染者
- [ ] 奔跑者
- [ ] 胖子
- [ ] 尖叫者
- [ ] 装甲丧尸
- [ ] 自爆丧尸
- [ ] 隐形丧尸
- [ ] 吞噬者
- [ ] 智慧丧尸
- [ ] 巨型丧尸
- [ ] 尸潮领袖 (BOSS)
- [ ] 实验体 01 (BOSS)
- [ ] 病毒母体 (BOSS)
- [ ] 零号病人 (BOSS)

### 英雄模型 (5 人)

- [ ] 雷克斯
- [ ] 艾琳
- [ ] 老杰克
- [ ] 影子
- [ ] 安娜

---

## 🎵 音效清单

### BGM

| 场景 | 曲目 | 时长 |
|------|------|------|
| 主菜单 | 末日主题 | 2:00 |
| 塔防战斗 | 紧张战斗 | 3:00 |
| 探索模式 | 悬疑探索 | 2:30 |
| BOSS 战 | 史诗决战 | 3:30 |
| 结局 A | 悲壮胜利 | 2:00 |
| 结局 B | 希望重生 | 2:30 |

### SFX

| 类型 | 音效 | 数量 |
|------|------|------|
| 防御塔 | 射击/装弹 | 12 个 |
| 丧尸 | 嘶吼/死亡 | 13 个 |
| UI | 点击/确认 | 5 个 |
| 爆炸 | 各种爆炸 | 8 个 |

---

## 📈 里程碑

| 里程碑 | 日期 | 交付物 |
|--------|------|--------|
| **MVP** | 第 4 周末 | 可玩 demo (1-5 关) |
| **Alpha** | 第 12 周末 | 第一章完整 |
| **Beta** | 第 24 周末 | 全部 40 关 |
| **Release** | 第 28 周末 | 正式上线 |

---

## 📝 待办事项

### 立即开始

- [ ] 选择游戏引擎 (Unity/Godot/Unreal)
- [ ] 搭建项目框架
- [ ] 创建数据表结构
- [ ] 实现基础防御塔

### 本周内

- [ ] 完成 MVP 防御塔系统
- [ ] 完成 MVP 丧尸系统
- [ ] 完成第 1 关 playable

### 本月内

- [ ] 完成 MVP 全部功能
- [ ] 内部测试
- [ ] 收集反馈

---

**最后更新：** 2026-03-15
**状态：** 🟡 开发准备中
