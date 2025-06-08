# PlayStation 奖杯追踪器

一个交互式的 PlayStation 奖杯追踪应用，支持配置文件管理和点击交互。

## 🎮 功能特性

### 📊 数据管理

- **配置文件**: 使用 `config.json` 存储游戏信息、奖杯数据和设置
- **自动保存**: 数据变更时自动保存到本地存储
- **数据持久化**: 刷新页面后数据保持不变

### 🖱️ 交互操作

- **左键点击奖杯**: 增加对应奖杯数量 +1
- **右键点击奖杯**: 减少对应奖杯数量 -1
- **悬停效果**: 鼠标悬停时奖杯图标放大并显示背景高亮
- **视觉反馈**: 点击动画、数字变化高亮效果和进度圆环更新
- **数据验证**: 奖杯数量不会小于 0，确保数据有效性

### 🎨 视觉特性

- **双主题支持**: 内置暗色和亮色主题，可通过快捷键或 API 切换
- **主题切换**: 使用 `Ctrl+T` 快捷键或 API 调用切换主题
- **自适应设计**: 两种主题都经过精心调优，确保最佳视觉体验

### ⌨️ 键盘快捷键

- `Ctrl+S` / `Cmd+S`: 手动保存配置
- `Ctrl+R` / `Cmd+R`: 重置所有奖杯数据
- `Ctrl+E` / `Cmd+E`: 导出配置文件
- `Ctrl+T` / `Cmd+T`: 切换主题（暗色/亮色）

## 📊 奖杯进度计算方式

### 🎯 本工具的计算方式

本工具采用**简单计数法**来计算奖杯进度：

```javascript
进度百分比 = (已获得奖杯总数 / 游戏总奖杯数) × 100%
```

**计算逻辑**：

- 已获得奖杯总数 = 白金 + 金 + 银 + 铜奖杯数量
- 所有奖杯类型具有相同权重，每个奖杯对进度的贡献度相等
- 简单直观，易于理解和计算

**示例**：

- 假设游戏有 50 个奖杯，已获得 25 个（无论类型）
- 进度 = (25 ÷ 50) × 100% = 50%

### 🏆 PlayStation 官方计算方式

PlayStation 官方采用**加权计分制**来计算奖杯进度：

```javascript
进度分数 = (铜奖杯×15分) + (银奖杯×30分) + (金奖杯×90分) + (白金奖杯×300分)
总分数 = 游戏所有奖杯的理论最高分数
进度百分比 = (进度分数 / 总分数) × 100%
```

**计算逻辑**：

- **铜奖杯**：15 分
- **银奖杯**：30 分
- **金奖杯**：90 分
- **白金奖杯**：300 分
- 高级奖杯对进度贡献更大，体现获得难度差异

**示例**：

- 假设游戏有：1 白金 + 5 金 + 10 银 + 34 铜 = 50 个奖杯
- 总分数 = (1×300) + (5×90) + (10×30) + (34×15) = 300 + 450 + 300 + 510 = 1560 分
- 已获得：0 白金 + 2 金 + 5 银 + 20 铜
- 获得分数 = (0×300) + (2×90) + (5×30) + (20×15) = 0 + 180 + 150 + 300 = 630 分
- PS 官方进度 = (630 ÷ 1560) × 100% ≈ 40.4%
- 本工具进度 = (27 ÷ 50) × 100% = 54%

## ⚙️ 配置文件说明

`config.json` 包含以下配置项：

### gameInfo (游戏信息)

- `name`: 游戏名称
- `platform`: 游戏平台
- `lastUpdated`: 最后更新日期

### trophyData (奖杯数据)

- `totalAvailable`: 可获得奖杯总数
- `platinum`: 白金奖杯数量
- `gold`: 金奖杯数量
- `silver`: 银奖杯数量
- `bronze`: 铜奖杯数量

### settings (应用设置)

- `autoSave`: 是否自动保存
- `animationDuration`: 动画持续时间
- `theme`: 主题风格
- `language`: 界面语言

### ui (界面设置)

- `showPercentage`: 是否显示百分比
- `showAnimations`: 是否显示动画效果
- `compactMode`: 是否启用紧凑模式

## 🔧 开发者 API

应用提供了全局 API 接口，可以在浏览器控制台中使用：

```javascript
// 获取奖杯管理器实例
const manager = window.TrophyApp.manager();

// 更新单个奖杯数量
window.TrophyApp.updateTrophy("gold", 5);

// 批量更新奖杯数据
window.TrophyApp.updateTrophies({
  gold: 3,
  silver: 15,
  bronze: 42,
});

// 重置所有奖杯
window.TrophyApp.resetTrophies();

// 导出配置文件
window.TrophyApp.exportConfig();

// 获取当前奖杯数据
const data = window.TrophyApp.getData();
console.log(data);

// 获取完整配置
const config = window.TrophyApp.getConfig();
console.log(config);

// 切换主题
window.TrophyApp.toggleTheme();

// 设置特定主题
window.TrophyApp.setTheme("light"); // 或 "dark"
```

## 🚨 常见问题

### Q: 可以修改奖杯图标吗？

A: 可以！替换对应的 PNG 文件即可，建议保持文件名不变，尺寸建议 48x48 像素或更高。

### Q: 如何重置所有数据？

A: 使用快捷键 `Ctrl+R` / `Cmd+R`，或者在开发者控制台中执行 `window.TrophyApp.resetTrophies()`。

### Q: 如何切换主题？

A: 可以通过以下三种方式切换主题：

1. 点击右上角的主题切换按钮（🌙/☀️）
2. 使用键盘快捷键 `Ctrl+T` / `Cmd+T`
3. 在开发者控制台中执行 `window.TrophyApp.toggleTheme()` 或 `window.TrophyApp.setTheme("light")`

### Q: 主题设置会保存吗？

A: 是的，主题选择会自动保存到本地存储和配置文件中，下次打开应用时会自动应用您上次选择的主题。

---

## 📄 许可证

本项目采用 MIT 许可证，详情请参阅项目中的许可证文件。

## 🙏 致谢

- (PSRay)[https://psray.net/] 提供的 PlayStation 奖杯图标

**提示**: 首次使用时，应用会尝试加载 `config.json` 文件。如果文件不存在，将使用默认配置并自动保存到本地存储。
