# Turbo Kart Rush

**一款致敬《马力欧卡丁车》的街机卡丁车竞速游戏，完全基于 Three.js 构建。所有模型、纹理、音效和音乐均在加载时通过代码生成，本仓库不包含任何外部资源文件。**

整个游戏由五个 Claude Fable 5.1 子代理并行开发完成，仅凭一条指令，无需任何后续提问。

<p align="center">
  <a href="https://stellarisedward.github.io/turbo-kart-rush/"><img src="docs/screenshots/title.jpg" alt="Turbo Kart Rush 标题画面" width="800"></a>
</p>

<p align="center">
  <a href="https://stellarisedward.github.io/turbo-kart-rush/"><strong>▶ 在浏览器中游玩</strong></a>
</p>

## 游玩方式

在支持 WebGL2 的桌面浏览器（Chrome、Edge、Firefox 或 Safari）中打开 **https://stellarisedward.github.io/turbo-kart-rush/**。在标题画面点击或按回车键，选择八位赛车手之一，挑选赛道和难度，然后开始比赛。支持键盘和游戏手柄。

与七名 AI 赛车手进行三圈竞速。漂移过弯并松开以获得迷你加速。拾取道具箱，发射龟壳、丢香蕉、使用蘑菇，或召唤闪电攻击全场。

## 构建此项目的指令

以下是给 Claude Code 的完整原始指令，未做任何额外说明。

> I need you to launch five Fable 5.1 sub-agents and help me build a triple A quality game that is a clone of Mario Kart. What I want you to do is I want you to launch these sub-agents, build the game without asking me any questions at all, and use 3JS to build the game. And once you're done, report back to me.

## 构建方式

主代理首先编写了架构契约，然后启动了五个子代理，每个子代理负责代码库的一个模块，基于共享的 TypeScript 接口和类型化事件总线进行开发。子代理之间不会互相修改文件。

| 工作流 | 负责模块 | 交付内容 |
| --- | --- | --- |
| A | `src/game`, `src/ui`, `src/main.ts` | 游戏循环、渲染器、比赛管理器、跟踪摄像机、HUD、菜单、结算 |
| B | `src/kart` | 街机卡丁车物理、漂移和迷你加速、卡丁车模型、输入、角色列表 |
| C | `src/track` | 程序化赛道构建器、四条赛道、地形、天空、装饰、看台 |
| D | `src/items`, `src/ai` | 道具箱和十种道具、具有个性和橡皮筋机制的 AI 赛车手 |
| E | `src/audio`, `src/fx` | Web Audio 引擎和音乐序列器、粒子系统、后处理 |

所有子代理共同遵循的架构契约位于 [CONTRACT.md](CONTRACT.md)。它定义了世界约定、各模块的公共 API、游戏流程和质量标准。`src/core` 包含共享类型、常量、数学工具和事件总线，在子代理开始工作前已冻结。

## 功能特性

- **八位赛车手**，三个重量级别，各自拥有独特的卡丁车、配色和操控感：Zippy Nova、Pixel Pop、Fennec Flash、Max Vortex、Juno Bolt、Kai Tidewater、Boulder Bram 和 Big Rig Rosa。
- **四条赛道**，每条 900 至 1400 米，包含山丘、跳台、发卡弯、S 弯和长直道：Sunny Circuit（草地）、Dune Drift（沙漠）、Frostbite Falls（雪地，含冰面虚空区域）和 Neon Nexus（夜间城市）。
- **街机操控**，支持跳跃、漂移、三段迷你加速、加速带、越野减速、撞墙反弹以及按重量计算的碰撞。
- **十种道具**：香蕉、绿龟壳、红龟壳、蓝龟壳、蘑菇、三连蘑菇、金色蘑菇、星星、闪电和炸弹。道具出现概率根据比赛名次加权。
- **AI 赛车手**：遵循赛车线、漂移过弯、躲避障碍、搜寻道具箱、合理使用道具，并向玩家进行橡皮筋调节。
- **完全合成音频**：每辆卡丁车独立的引擎合成与空间声道、数十种音效，以及程序化芯片音乐序列器，包含菜单、比赛、最后一圈和结算音乐。
- **视觉效果**：6000 粒子的 GPU 池用于漂移火花、加速火焰、轮胎烟雾、尘土和速度线，加上泛光、速度线、径向模糊、色差和暗角后处理。
- **广播级呈现**：3-2-1-GO 倒计时、名次和圈数 HUD、道具轮盘、小地图、最后一圈和逆行横幅、带彩纸的结算画面。
- **手柄支持**，与键盘并行使用。

## 操作方式

| 动作 | 键盘 | 手柄 |
| --- | --- | --- |
| 加速 | W 或 ↑ | 右扳机 |
| 刹车 / 倒车 | S 或 ↓ | 左扳机 |
| 转向 | A 和 D，或 ← 和 → | 左摇杆 |
| 跳跃 / 漂移 | 空格 或 Shift | A 或 RB |
| 使用道具 | E、Ctrl 或 Enter | X 或 LB |
| 向后看 | Q | |
| 暂停 | Esc 或 P | Start |
| 菜单确认 / 返回 | 回车或空格 / Esc | A / B |

在过弯时按住漂移，火花会依次变为蓝色、橙色、紫色。按住时间越长，松开时获得的加速越大。

## 本地运行

```bash
git clone https://github.com/stellarisedward/turbo-kart-rush.git
cd turbo-kart-rush
npm install
npm run dev
```

打开终端显示的地址，默认为 http://localhost:5178/。

| 脚本 | 功能 |
| --- | --- |
| `npm run dev` | 开发服务器，支持热更新 |
| `npm run build` | 生产构建，输出到 `dist/` |
| `npm run preview` | 预览生产构建 |
| `npm run typecheck` | TypeScript 类型检查 |

## 项目结构

```
turbo-kart-rush/
├── index.html                 入口页面，包含 #app 容器和模块脚本
├── CONTRACT.md                五个子代理共同遵循的架构契约
├── src/
│   ├── main.ts                启动 Game
│   ├── style.css              菜单、HUD 和结算样式
│   ├── core/                  冻结的共享层：类型、常量、数学工具、事件总线
│   ├── game/                  游戏循环、RaceManager、FollowCamera、菜单背景
│   ├── ui/                    MainMenu、HUD、Minimap、PauseMenu、ResultsScreen、LoadingScreen
│   ├── kart/                  卡丁车物理、KartModel、InputManager、角色列表
│   ├── track/                 Track、Centerline、TerrainField、纹理、builders/、tracks/
│   ├── items/                 ItemManager 和道具视觉效果
│   ├── ai/                    AIDriver
│   ├── audio/                 AudioEngine、引擎合成、音效、音乐序列器
│   └── fx/                    ParticleSystem、PostFX、着色器
├── docs/screenshots/          README 中使用的截图
└── .github/workflows/         每次推送到 main 时自动构建并部署到 GitHub Pages
```

## 技术说明

- **技术栈**：Three.js 0.185、TypeScript、Vite 8。WebGL2，使用 `ACESFilmicToneMapping` 和阴影贴图。
- **模拟**：固定步长累加器驱动卡丁车物理、道具、AI 和比赛逻辑；渲染、摄像机、粒子、音频和 HUD 按显示帧率运行。
- **全程序化**：纹理在加载时绘制到 Canvas，几何体从基础图元构建并合并或实例化，音频通过 Web Audio API 合并。生产包为单个 JavaScript 文件和一个样式表。
- **性能目标**（来自契约）：2020 年笔记本电脑上 1080p 分辨率 60fps，约 400 个绘制调用以下，单个阴影投射光源，实例化装饰，所有资源可释放以确保返回菜单时不泄漏。

## 部署方式

每次推送到 `main` 分支会触发 `.github/workflows/deploy.yml`，执行类型检查、构建并发布 `dist/` 到 GitHub Pages。Vite 配置了相对路径 `base`，因此构建产物可在任意子路径下运行。

## 截图

| | |
| --- | --- |
| ![角色选择](docs/screenshots/character-select.jpg) | ![赛道选择](docs/screenshots/track-select.jpg) |
| ![比赛](docs/screenshots/race.jpg) | ![标题](docs/screenshots/title.jpg) |

## 免责声明

Turbo Kart Rush 是一款原创的、粉丝制作的卡丁车竞速游戏致敬作品。与任天堂无任何关联、背书或合作关系。本仓库中的所有角色、赛道、名称、美术、音乐和代码均为原创。

## 许可证

[MIT](LICENSE) © 2026 BridgeMind
