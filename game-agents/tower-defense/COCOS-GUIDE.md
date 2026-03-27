# Cocos Creator 3.8 塔防游戏开发指南

> 从零开始制作《萝卜保卫战》

---

## 🚀 快速开始

### 环境搭建

1. **安装 Cocos Creator 3.8+**
   - 官网：https://www.cocos.com/products
   - 下载最新 LTS 版本

2. **安装 VSCode + TypeScript 插件**
   - TypeScript 插件
   - ESLint（可选）

3. **创建项目**
   ```
   打开 Cocos Creator
   → 新建项目 → 选择 Empty 模板
   → 项目名称：tower-defense
   → 选择目录：game-agents/tower-defense/
   → 创建
   ```

---

## 📂 项目结构

```
tower-defense/
├── assets/                 # 资源目录（Cocos 自动管理）
│   ├── scripts/           # TypeScript 脚本
│   │   ├── core/          # 核心系统
│   │   │   ├── GameManager.ts
│   │   │   ├── MapManager.ts
│   │   │   ├── PathManager.ts
│   │   │   ├── TowerManager.ts
│   │   │   └── WaveManager.ts
│   │   ├── towers/        # 防御塔脚本
│   │   │   ├── TowerBase.ts
│   │   │   ├── ArrowTower.ts
│   │   │   └── ...
│   │   ├── enemies/       # 敌人脚本
│   │   │   ├── EnemyBase.ts
│   │   │   └── ...
│   │   └── ui/            # UI 脚本
│   │       ├── GameUI.ts
│   │       └── BuildMenu.ts
│   ├── textures/          # 图片资源
│   │   ├── towers/        # 塔素材
│   │   ├── enemies/       # 敌人素材
│   │   ├── maps/          # 地图素材
│   │   └── ui/            # UI 素材
│   ├── audio/             # 音效
│   │   ├── sfx/           # 音效
│   │   └── bgm/           # 背景音乐
│   └── prefab/            # 预制体
│       ├── towers/        # 塔预制体
│       └── enemies/       # 敌人预制体
├── settings/              # 项目设置
└── temp/                  # 临时文件（可忽略）
```

---

## 🎮 核心系统实现

### 1. 地图系统

```typescript
// assets/scripts/core/MapManager.ts
import { _decorator, Component, Node, Vec2, Vec3, director } from 'cc';
const { ccclass, property } = _decorator;

@ccclass('MapManager')
export class MapManager extends Component {
    @property
    tileSize: number = 64;
    
    @property
    gridSize: Vec2 = new Vec2(15, 10);
    
    private walkableGrid: boolean[][] = [];
    
    start() {
        this.initGrid();
    }
    
    initGrid() {
        this.walkableGrid = [];
        for (let y = 0; y < this.gridSize.y; y++) {
            const row: boolean[] = [];
            for (let x = 0; x < this.gridSize.x; x++) {
                row.push(true); // true = 可行走
            }
            this.walkableGrid.push(row);
        }
    }
    
    getTilePosition(gridPos: Vec2): Vec3 {
        return new Vec3(gridPos.x * this.tileSize, gridPos.y * this.tileSize, 0);
    }
    
    getGridPosition(worldPos: Vec3): Vec2 {
        return new Vec2(Math.floor(worldPos.x / this.tileSize), 
                       Math.floor(worldPos.y / this.tileSize));
    }
    
    isWalkable(gridPos: Vec2): boolean {
        const x = Math.floor(gridPos.x);
        const y = Math.floor(gridPos.y);
        if (x < 0 || x >= this.gridSize.x || y < 0 || y >= this.gridSize.y) {
            return false;
        }
        return this.walkableGrid[y][x];
    }
}
```

---

### 2. 路径系统

```typescript
// assets/scripts/core/PathManager.ts
import { _decorator, Component, Vec3, Vec2 } from 'cc';
const { ccclass, property } = _decorator;

@ccclass('PathManager')
export class PathManager extends Component {
    @property({ type: Vec3 })
    pathPoints: Vec3[] = [];
    
    getPath(): Vec3[] {
        return this.pathPoints;
    }
    
    getSpawnPoint(): Vec3 {
        return this.pathPoints[0];
    }
    
    getEndPoint(): Vec3 {
        return this.pathPoints[this.pathPoints.length - 1];
    }
    
    // 在编辑器中绘制路径
    onDrawGizmos() {
        if (this.pathPoints.length < 2) return;
        
        for (let i = 0; i < this.pathPoints.length - 1; i++) {
            Gizmos.drawLine(this.pathPoints[i], this.pathPoints[i + 1]);
        }
    }
}
```

---

### 3. 防御塔系统

```typescript
// assets/scripts/towers/TowerBase.ts
import { _decorator, Component, Node, Collider2D, ICollider2D } from 'cc';
const { ccclass, property } = _decorator;

@ccclass('TowerBase')
export class TowerBase extends Component {
    @property
    towerName: string = '箭塔';
    
    @property
    damage: number = 10;
    
    @property
    attackRange: number = 150;
    
    @property
    attackSpeed: number = 1; // 秒/次
    
    @property
    cost: number = 100;
    
    private currentTarget: Node | null = null;
    private attackTimer: number = 0;
    private level: number = 1;
    
    start() {
        // 初始化
    }
    
    update(deltaTime: number) {
        this.attackTimer += deltaTime;
        if (this.attackTimer >= this.attackSpeed) {
            this.findAndAttackTarget();
            this.attackTimer = 0;
        }
    }
    
    findAndAttackTarget() {
        // 查找范围内的敌人
        const enemies = this.getEnemiesInRange();
        if (enemies.length > 0) {
            this.currentTarget = enemies[0];
            this.attack();
        }
    }
    
    getEnemiesInRange(): Node[] {
        // 实现敌人查找逻辑
        return [];
    }
    
    attack() {
        if (this.currentTarget) {
            // 攻击敌人
            const enemyScript = this.currentTarget.getComponent('EnemyBase');
            if (enemyScript) {
                enemyScript.takeDamage(this.damage);
            }
        }
    }
    
    upgrade() {
        this.level++;
        this.damage *= 1.5;
        this.attackRange *= 1.1;
    }
}
```

---

### 4. 敌人系统

```typescript
// assets/scripts/enemies/EnemyBase.ts
import { _decorator, Component, Node, Vec3, UITransform } from 'cc';
const { ccclass, property } = _decorator;

@ccclass('EnemyBase')
export class EnemyBase extends Component {
    @property
    maxHealth: number = 100;
    
    @property
    speed: number = 50;
    
    @property
    reward: number = 10;
    
    private currentHealth: number = 0;
    private path: Vec3[] = [];
    private currentPointIndex: number = 0;
    
    start() {
        this.currentHealth = this.maxHealth;
    }
    
    update(deltaTime: number) {
        if (this.path.length === 0) return;
        
        const target = this.path[this.currentPointIndex];
        const direction = new Vec3();
        Vec3.subtract(direction, target, this.node.position);
        direction.normalize();
        
        if (direction.length() > 0) {
            const moveStep = direction.multiplyScalar(this.speed * deltaTime);
            this.node.setPosition(
                this.node.position.x + moveStep.x,
                this.node.position.y + moveStep.y,
                this.node.position.z
            );
        }
        
        // 检查是否到达目标点
        const distance = Vec3.distance(this.node.position, target);
        if (distance < 5) {
            this.currentPointIndex++;
            if (this.currentPointIndex >= this.path.length) {
                this.reachedEnd();
                return;
            }
        }
    }
    
    takeDamage(amount: number) {
        this.currentHealth -= amount;
        if (this.currentHealth <= 0) {
            this.die();
        }
    }
    
    die() {
        // 奖励金币
        const gameManager = this.findGameManager();
        if (gameManager) {
            gameManager.addGold(this.reward);
        }
        this.node.destroy();
    }
    
    reachedEnd() {
        // 到达终点，扣生命
        const gameManager = this.findGameManager();
        if (gameManager) {
            gameManager.playerHealth--;
        }
        this.node.destroy();
    }
    
    setPath(path: Vec3[]) {
        this.path = path;
    }
    
    findGameManager(): any {
        // 查找 GameManager
        return null;
    }
}
```

---

### 5. 经济系统

```typescript
// assets/scripts/core/GameManager.ts
import { _decorator, Component, EventTouch } from 'cc';
const { ccclass, property } = _decorator;

@ccclass('GameManager')
export class GameManager extends Component {
    static instance: GameManager | null = null;
    
    @property
    playerGold: number = 200;
    
    @property
    playerHealth: number = 10;
    
    @property
    interestRate: number = 0.1; // 10% 利息
    
    @property
    maxInterestGold: number = 100; // 最大计息金币
    
    onLoad() {
        if (!GameManager.instance) {
            GameManager.instance = this;
        }
    }
    
    start() {
        this.updateUI();
    }
    
    addGold(amount: number) {
        this.playerGold += amount;
        this.updateUI();
    }
    
    spendGold(amount: number): boolean {
        if (this.playerGold >= amount) {
            this.playerGold -= amount;
            this.updateUI();
            return true;
        }
        return false;
    }
    
    calculateInterest(): number {
        const interestGold = Math.min(this.playerGold, this.maxInterestGold);
        return Math.floor(interestGold * this.interestRate);
    }
    
    collectInterest() {
        const interest = this.calculateInterest();
        this.addGold(interest);
        return interest;
    }
    
    updateUI() {
        // 更新 UI 显示
        // 例如：this.node.getChildByName('GoldLabel')?.getComponent(Label).string = this.playerGold.toString();
    }
}
```

---

## 🎨 资源整合

### 导入 Kenney 资源

1. 下载资源包：https://kenney.nl/assets/tower-defense
2. 解压后复制到 `assets/textures/` 目录
3. Cocos 会自动导入

### 创建预制体（Prefab）

1. 在场景中创建塔/敌人
2. 拖拽到 `assets/prefab/` 目录
3. 代码中实例化：
```typescript
const towerPrefab = resources.get('prefab/towers/ArrowTower');
const tower = instantiate(towerPrefab);
```

---

## 🔊 音效整合

```typescript
// assets/scripts/core/AudioManager.ts
import { _decorator, Component, AudioClip, AudioSource } from 'cc';
const { ccclass, property } = _decorator;

@ccclass('AudioManager')
export class AudioManager extends Component {
    static instance: AudioManager | null = null;
    
    @property({ type: AudioClip })
    towerBuildSound: AudioClip | null = null;
    
    @property({ type: AudioClip })
    towerAttackSound: AudioClip | null = null;
    
    @property({ type: AudioClip })
    enemyHitSound: AudioClip | null = null;
    
    onLoad() {
        if (!AudioManager.instance) {
            AudioManager.instance = this;
        }
    }
    
    playSound(soundName: string) {
        // 播放音效逻辑
    }
}
```

---

## 📱 发布配置

### 微信小游戏

1. 项目设置 → 发布设置 → 微信小游戏
2. 配置 AppID
3. 构建发布

### Steam/PC

1. 项目设置 → 发布设置 → Windows/Mac
2. 构建发布
3. 打包上传 Steam

---

## 🐛 常见问题

### 性能优化
1. 使用对象池管理敌人
2. 减少 DrawCall（合批）
3. 使用 LOD（远距离简化）

### 内存管理
1. 及时释放不用的资源
2. 避免在 update 中创建对象
3. 使用资源预加载

---

**版本：** 1.0（Cocos 版）
**更新日期：** 2026-03-15
**适用：** Cocos Creator 3.8+
