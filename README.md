# Grab Claude

一个单文件的网页小游戏:各个 Claude 模型会以像素螃蟹的样子短暂冒出来,在 20 秒内尽可能多地点中它们,换取 Token(游戏积分)。

界面模仿 Claude 的视觉风格,包含仿制的用量页和一个玩笑性质的"账户停用"页。

> 非官方的同人作品,与 Anthropic 无关。"Claude"是 Anthropic 的商标,见文末[声明](#声明)。

## 怎么玩

直接用浏览器打开 `Grab Claude.html` 即可。没有构建步骤,没有依赖,不联网,离线也能玩。电脑和手机浏览器都支持。

- 每局 20 秒,点击或触摸冒出来的螃蟹。
- 连续命中会累积连击,每 10 连击额外 +5 分。连击窗口为 1.6 秒,点空会中断连击。
- 命中判定很宽松,会取离点击位置最近的一只。
- 螃蟹会在场地里走动,并绕开连击计数器和彼此。

### 计分

| 模型 | 分值 | 特点 |
|---|---|---|
| Haiku | +0.5 | 小、快、停留短 |
| Sonnet | +1 | 最常见 |
| Opus | +10 | 较稀有,带闪光 |
| Fable | +20 | 更稀有,停留更短 |
| Mythos | +50 | 极稀有,出现时有横幅和专属音效 |

越值钱越稀有,也越难抓。

## 功能

- **A\* 寻路**:螃蟹挑选附近的目标点,在粗网格上用 A\*(8 方向、不斜穿墙角、路径平滑)规划路线,绕开连击计数器和其他螃蟹。
- **打击手感**:命中停顿、屏幕震动、像素碎裂粒子、冲击环。
- **合成音效**:全部由 WebAudio 实时合成,没有音频文件。
- **触感反馈**:Android 使用 Vibration API;iOS 通过原生 `<input type="checkbox" switch>` 触发系统触感(iOS 18+)。
- **用量页**:点击右上角的 Token 数打开,按 Sonnet / Opus 分别显示每日和每周"用量"。它只是根据你抓到的数量做的展示,不会限制游玩。
- **地区与语言**:首次运行时选择地区。选"美国"是英文界面;选"中国"是中文界面,并会弹出一个"账户已停用"的玩笑页面,点"申请解封"即可通过。页面不会收集任何信息。
- **适配**:支持深色模式、小屏手机、横屏短屏,并尊重 `prefers-reduced-motion`。

## 数据与隐私

游戏没有任何网络请求。进度只保存在你浏览器的 `localStorage`,不会上传。

| 键 | 内容 |
|---|---|
| `crabcatcher_claude_v1` | 累计 Token、累计抓获数、最佳单局 |
| `crabcatcher_sfx` | 音效开关 |
| `grabclaude_haptics` | 触感开关 |
| `grabclaude_region` | 所选地区 |
| `grabclaude_usage` | 用量页的每日和每周计数 |

在 设置 → 重置记录 里可以清空累计 Token、抓获数和最佳记录。

## 调整参数

所有玩法数值都在 `Grab Claude.html` 里的脚本部分:

| 想改什么 | 位置 |
|---|---|
| 各模型的分值、出现概率、停留时间、速度、大小 | `KINDS` 表(`fixed` 为真时,`life` 和 `speed` 是绝对值,否则按局内节奏缩放) |
| 一局时长、连击窗口 | `ROUND`、`COMBO_WIN` |
| 用量页的上限 | `LIMITS` |
| 螃蟹每次走多远、绕路的明显程度 | `planPath` 中目标点的距离 |
| 寻路算法本身 | `astar()` 及其平滑函数 |

## 项目结构

```
Grab Claude.html   游戏本体:HTML + CSS + JS,约 1200 行
LICENSE            Apache License 2.0
```

## 声明

这是一个非官方的同人项目,与 Anthropic 没有任何关联,也未获其认可。"Claude"及相关名称、标识归 Anthropic 所有。界面中的仿制页面(用量页、账户停用页)仅用于玩笑和致敬,请勿用于冒充官方服务。字体栈中提到的 Copernicus、Styrene 为 Anthropic 的专有字体,本项目没有包含它们,未安装时会回退到系统字体。

## 许可

[Apache License 2.0](LICENSE)

---

## English

**Grab Claude** is a single-file browser game. Claude models pop up as pixel crabs for a moment; tap as many as you can in 20 seconds. Haiku is worth 0.5, Sonnet 1, Opus 10, Fable 20 and the rare Mythos 50, with a bonus for every 10-hit combo. Crabs walk around using A\* pathfinding.

Open `Grab Claude.html` in a browser. There is no build step, no dependencies and no network access; progress stays in `localStorage`. Choosing the United States on first run gives the English interface. Gameplay numbers live in the `KINDS` table in the script.

This is an unofficial fan project, not affiliated with or endorsed by Anthropic. Licensed under Apache 2.0.
