# 梦幻西游放置挂机游戏 - 详细设计文档

## 一、游戏概述

### 1.1 游戏类型
- 类型：放置挂机 RPG
- 风格：梦幻西游端游风格
- 平台：Web 浏览器

### 1.2 核心玩法
- 自动战斗：自动打怪、自动用技能
- 自动任务：自动完成日常任务
- 挂机收益：离线也能获得经验和物品
- 角色养成：等级、技能、装备、宠物

---

## 二、界面设计

### 2.1 主界面布局
```
┌─────────────────────────────────────────────────┐
│                  顶部状态栏                       │
│  角色: 逍遥生 Lv.45  门派: 大唐官府  银两: 12,345  │
├──────────────┬────────────────────┬──────────────┤
│              │                    │              │
│   角色面板    │     场景/战斗区域    │    任务面板   │
│              │                    │              │
│  [头像]       │                    │   师门任务    │
│  气血: ████░  │   [玩家] vs [怪物]   │   抓鬼任务    │
│  魔法: ███░░  │                    │   副本挑战    │
│  经验: ██░░░  │   战斗日志区域       │   活动任务    │
│              │                    │              │
│  攻击: 156    │   [攻击] [技能]     │   [开始挂机]  │
│  防御: 89     │   [自动] [挂机]     │   [停止挂机]  │
│  速度: 45     │                    │              │
│  灵力: 32     │                    │              │
│              │                    │              │
│  [装备] [属性]│                    │   任务进度    │
│              │                    │   ██████░░   │
├──────────────┴────────────────────┴──────────────┤
│                  底部操作栏                       │
│  [背包] [技能] [宠物] [地图] [社交] [设置]        │
└─────────────────────────────────────────────────┘
```

### 2.2 界面元素

#### 顶部状态栏
- 角色名称、等级、门派
- 银两、仙玉数量
- 当前地图名称

#### 左侧角色面板
- 角色头像
- 气血/魔法/经验条
- 基础属性（攻击、防御、速度、灵力）
- 装备快捷按钮
- 属性加点按钮

#### 中间战斗区域
- 场景背景
- 怪物显示
- 玩家角色显示
- 战斗日志滚动区域
- 操作按钮（攻击、技能、自动、挂机）

#### 右侧任务面板
- 任务列表
- 任务进度
- 开始/停止挂机按钮
- 挂机收益预览

#### 底部操作栏
- 背包、技能、宠物、地图、社交、设置

---

## 三、核心系统设计

### 3.1 角色养成系统

#### 角色属性
```javascript
{
  name: "逍遥生",
  race: "人族",
  faction: "大唐官府",
  level: 1,
  exp: 0,
  expToNext: 100,
  
  // 基础属性
  hp: 100,
  maxHp: 100,
  mp: 50,
  maxMp: 50,
  
  attack: 15,
  defense: 8,
  speed: 10,
  spirit: 5,
  
  // 成长属性
  hpGrowth: 10,
  mpGrowth: 5,
  attackGrowth: 3,
  defenseGrowth: 2,
  speedGrowth: 1,
  spiritGrowth: 1,
  
  // 装备槽
  equipment: {
    weapon: null,
    armor: null,
    accessory: null
  }
}
```

#### 门派系统
```javascript
const FACTIONS = {
  // 人族门派
  datang: {
    name: "大唐官府",
    race: "人族",
    skills: ["横扫千军", "后发制人", "破釜沉舟"],
    bonus: { attack: 10 }
  },
  fangcun: {
    name: "方寸山",
    race: "人族",
    skills: ["符咒", "定身", "封印"],
    bonus: { spirit: 10 }
  },
  huasheng: {
    name: "化生寺",
    race: "人族",
    skills: ["治疗", "复活", "增益"],
    bonus: { mp: 20 }
  },
  
  // 仙族门派
  longgong: {
    name: "龙宫",
    race: "仙族",
    skills: ["龙卷雨击", "龙腾", "龙吟"],
    bonus: { spirit: 15 }
  },
  tiangong: {
    name: "天宫",
    race: "仙族",
    skills: ["天雷斩", "五雷轰顶", "雷霆万钧"],
    bonus: { attack: 8, spirit: 8 }
  },
  
  // 魔族门派
  mowang: {
    name: "魔王寨",
    race: "魔族",
    skills: ["三昧真火", "飞沙走石", "牛劲"],
    bonus: { hp: 20 }
  },
  difu: {
    name: "阴曹地府",
    race: "魔族",
    skills: ["阎罗令", "尸腐毒", "修罗隐身"],
    bonus: { defense: 10 }
  }
};
```

### 3.2 战斗系统

#### 战斗流程
```
1. 玩家进入战斗
2. 计算速度 → 确定行动顺序
3. 回合开始
   - 自动选择目标
   - 自动选择技能
   - 执行攻击/技能
   - 计算伤害
   - 更新状态
4. 判断胜负
5. 战斗结算（经验、物品）
6. 自动恢复（如启用）
```

#### 战斗数据结构
```javascript
const Battle = {
  player: {
    hp: 100,
    maxHp: 100,
    attack: 15,
    defense: 8,
    speed: 10,
    skills: ["普通攻击", "横扫千军"]
  },
  
  enemy: {
    name: "野猪",
    sprite: "🐷",
    hp: 30,
    maxHp: 30,
    attack: 5,
    defense: 3,
    speed: 5,
    exp: 15,
    gold: 5,
    drops: [
      { item: "小还丹", rate: 0.1 },
      { item: "铁剑", rate: 0.01 }
    ]
  },
  
  turn: 1,
  log: [],
  state: "fighting" // fighting, won, lost
};
```

#### 伤害计算公式
```
基础伤害 = 攻击力 - 防御力 * 0.5
实际伤害 = 基础伤害 * (0.9 ~ 1.1 随机浮动)
暴击伤害 = 实际伤害 * 1.5 (暴击率 5%)
```

### 3.3 任务系统

#### 任务类型
```javascript
const TASKS = {
  // 师门任务
  shimen: {
    name: "师门任务",
    type: "daily",
    count: 20,
    reward: {
      exp: 1000,
      gold: 500,
      factionRep: 10
    },
    autoCombat: true,
    duration: 30000 // 30秒一个
  },
  
  // 抓鬼任务
  zhuagui: {
    name: "抓鬼任务",
    type: "daily",
    count: 10,
    reward: {
      exp: 2000,
      gold: 1000,
      items: ["还魂丹", "金创药"]
    },
    autoCombat: true,
    duration: 60000 // 60秒一个
  },
  
  // 副本挑战
  dungeon: {
    name: "副本挑战",
    type: "weekly",
    count: 3,
    reward: {
      exp: 5000,
      gold: 3000,
      equipment: "精良"
    },
    autoCombat: true,
    duration: 300000 // 5分钟一个
  }
};
```

#### 任务队列系统
```javascript
class TaskQueue {
  constructor() {
    this.queue = [];
    this.running = false;
    this.currentTask = null;
  }
  
  addTask(task) {
    this.queue.push(task);
  }
  
  start() {
    this.running = true;
    this.processNext();
  }
  
  stop() {
    this.running = false;
  }
  
  async processNext() {
    if (!this.running || this.queue.length === 0) return;
    
    this.currentTask = this.queue.shift();
    await this.executeTask(this.currentTask);
    
    this.processNext();
  }
}
```

### 3.4 宠物系统

#### 宠物数据结构
```javascript
const PET = {
  name: "吸血鬼",
  level: 1,
  growth: 1.2, // 成长率
  
  // 资质
  aptitude: {
    attack: 1200,
    defense: 1000,
    hp: 1500,
    speed: 1100
  },
  
  // 技能
  skills: ["连击", "必杀", "吸血"],
  
  // 属性点
  points: 0,
  allocation: {
    attack: 0,
    defense: 0,
    hp: 0,
    speed: 0
  }
};
```

### 3.5 装备系统

#### 装备数据结构
```javascript
const EQUIPMENT = {
  name: "玄铁剑",
  type: "weapon",
  quality: "精良", // 普通、优秀、精良、史诗、传说
  level: 10,
  
  attributes: {
    attack: 50,
    speed: 5
  },
  
  // 强化等级
  reinforce: 0,
  
  // 镶嵌宝石
  gems: []
};
```

### 3.6 挂机系统

#### 离线收益计算
```javascript
function calculateOfflineRewards(lastSaveTime) {
  const offlineTime = Date.now() - lastSaveTime;
  const offlineHours = offlineTime / (1000 * 60 * 60);
  
  // 每小时收益
  const hourlyReward = {
    exp: player.level * 100,
    gold: player.level * 50
  };
  
  // 最多计算24小时
  const effectiveHours = Math.min(offlineHours, 24);
  
  return {
    exp: Math.floor(hourlyReward.exp * effectiveHours),
    gold: Math.floor(hourlyReward.gold * effectiveHours),
    offlineTime: offlineHours.toFixed(1)
  };
}
```

---

## 四、技术实现

### 4.1 技术栈
- **前端**: HTML5 + CSS3 + JavaScript (原生)
- **存储**: LocalStorage
- **部署**: GitHub Pages

### 4.2 数据存储
```javascript
// 保存游戏
function saveGame() {
  const gameData = {
    player: player,
    pets: pets,
    equipment: equipment,
    settings: settings,
    lastSave: Date.now()
  };
  localStorage.setItem('mhxyIdleGame', JSON.stringify(gameData));
}

// 加载游戏
function loadGame() {
  const saved = localStorage.getItem('mhxyIdleGame');
  if (saved) {
    const data = JSON.parse(saved);
    Object.assign(player, data.player);
    Object.assign(pets, data.pets);
    // ...
  }
}
```

### 4.3 自动战斗循环
```javascript
class AutoBattle {
  constructor() {
    this.running = false;
    this.interval = null;
  }
  
  start() {
    this.running = true;
    this.battleLoop();
  }
  
  stop() {
    this.running = false;
    if (this.interval) clearTimeout(this.interval);
  }
  
  async battleLoop() {
    if (!this.running) return;
    
    // 开始战斗
    const result = await this.executeBattle();
    
    // 处理结果
    this.processResult(result);
    
    // 自动恢复
    this.autoRecover();
    
    // 下一场战斗
    this.interval = setTimeout(() => this.battleLoop(), 2000);
  }
  
  async executeBattle() {
    // 战斗逻辑
  }
  
  processResult(result) {
    // 处理经验、物品掉落
  }
  
  autoRecover() {
    // 自动使用药品恢复
  }
}
```

---

## 五、美术资源

### 5.1 界面风格
- 配色：紫色 + 金色主题（梦幻风格）
- 字体：微软雅黑
- 图标：emoji 或 SVG

### 5.2 角色和怪物
- 使用 emoji 或简单的 CSS 绘制
- 后期可替换为图片资源

---

## 六、开发计划

### 6.1 第一阶段：核心功能
- [ ] 角色创建和属性系统
- [ ] 基础战斗系统
- [ ] 自动战斗和挂机
- [ ] 数据存储

### 6.2 第二阶段：养成系统
- [ ] 门派和技能
- [ ] 装备系统
- [ ] 宠物系统

### 6.3 第三阶段：任务系统
- [ ] 日常任务
- [ ] 副本挑战
- [ ] 离线收益

### 6.4 第四阶段：优化和完善
- [ ] 界面美化
- [ ] 数值平衡
- [ ] 性能优化

---

## 七、数值设计

### 7.1 等级成长
```
等级1 → 等级2: 100 经验
等级2 → 等级3: 150 经验
...
等级N → 等级N+1: 100 * N * 1.5
```

### 7.2 怪物强度
```
怪物HP = 20 + 等级 * 5
怪物攻击 = 3 + 等级 * 2
怪物防御 = 1 + 等级 * 1
怪物经验 = 等级 * 10
怪物金币 = 等级 * 3
```

### 7.3 装备属性
```
白装: 基础属性 * 1.0
绿装: 基础属性 * 1.3
蓝装: 基础属性 * 1.6
紫装: 基础属性 * 2.0
金装: 基础属性 * 2.5
```

---

这份设计文档涵盖了游戏的主要方面，可以作为开发的指导。