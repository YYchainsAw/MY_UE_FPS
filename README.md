# My_FPS

基于 Unreal Engine 5.7 制作的第一人称射击游戏学习项目。项目主要跟随 B 站教程 [FPS游戏开发教程 | UE5制作第一人称射击游戏 | 虚幻引擎5.7 零基础入门](https://www.bilibili.com/video/BV1aw9jBeEHr) 搭建，目标是从零开始梳理一个 FPS 项目的基础工程结构、角色控制、武器逻辑、动画表现、射击反馈和资源管理流程。

本项目目前以蓝图为主，围绕第一人称角色和 MCX-VIRTUS 武器实现核心玩法闭环：玩家可以在测试地图中移动、观察、奔跑、瞄准、开火，并通过动画、音效、枪口特效、弹壳、命中反馈和弹孔痕迹增强射击手感。

## 项目概览

| 项目 | 内容 |
| --- | --- |
| 引擎版本 | Unreal Engine 5.7 |
| 项目类型 | 第一人称射击游戏学习项目 |
| 实现方式 | Blueprint / Enhanced Input / Animation Blueprint / MetaSound / Niagara |
| 默认地图 | `/Game/NewMap.NewMap` |
| 默认游戏模式 | `/Game/Code/Player/System/BP_FPSGameMode` |
| 主要武器 | MCX-VIRTUS |
| 参考教程 | B 站 BV1aw9jBeEHr，UP 主：星露Studio |

## 项目目标

这个项目不是完整商业游戏，而是一个面向学习和练习的 FPS 工程。它的重点在于理解 UE5 中第一人称射击项目的基本拆分方式，包括：

- 如何创建并配置 FPS 项目的基础工程。
- 如何使用 Enhanced Input 管理移动、视角、奔跑、瞄准、开火和换弹输入。
- 如何拆分玩家角色、武器基类、具体武器、动画蓝图和特效资源。
- 如何制作第一人称武器动画、开火表现、换弹流程和状态切换。
- 如何使用 MetaSound、Niagara、物理材质和贴花资源完善射击反馈。
- 如何按照较清晰的目录结构组织蓝图、材质、动画、音效、VFX 和地图资产。

## 已实现内容

### 玩家控制

- 第一人称玩家蓝图 `BP_FPSPlayer`。
- 角色移动、视角控制、奔跑、瞄准、开火、换弹等输入动作。
- 基于 Enhanced Input 的输入资产：
  - `IA_Move`
  - `IA_Look`
  - `IA_Run`
  - `IA_Aim`
  - `IA_Fire`
  - `IA_Reload`
  - `IMC_FPSInput`
- 玩家状态枚举 `E_PlayerState`，用于组织不同玩家行为状态。

### 武器系统

- 武器基类 `BP_WeaponBase`，用于承载可扩展的武器通用逻辑。
- 具体武器蓝图 `BP_VIRTUS`，对应 MCX-VIRTUS 资源。
- 武器状态枚举 `E_WeaponState`，用于处理武器行为状态。
- 开火接口 `BI_Fire`，用于解耦角色和武器之间的开火调用。
- 弹壳蓝图 `BP_Casing`，用于表现开火后的抛壳效果。

### 动画系统

- 玩家动画蓝图 `ABP_FPSPlayer`。
- 玩家移动混合空间 `BS_FPSPlayer`。
- 武器动画蓝图 `ABP_VIRTUS`。
- 武器动画混合空间 `BS_VIRTUS`。
- 换弹动画通知 `AN_Reload`，用于在动画流程中触发换弹相关逻辑。
- MCX-VIRTUS 相关的 Idle、ADS、Fire、Reload、Sprint 等动画资源。

### 射击反馈

- 开火音效通过 `MS_VIRTUS` 等 MetaSound / Sound 资源组织。
- 枪口火焰和冲击效果使用 Niagara / VFX 资源表现。
- 命中不同材质时使用不同的粒子、音效和弹孔材质反馈。
- 已配置物理材质资源，例如 `PM_Dirt` 和 `PM_Metal`。
- 弹孔贴花资源包括混凝土、布料、玻璃、金属、岩石、木材等材质版本。

### 地图与测试场景

- 默认测试地图为 `NewMap`。
- 项目启用 World Partition 相关的外部 Actor / Object 资源目录。
- 当前地图适合作为 FPS 系统调试场景，用于验证角色、武器、命中反馈和视觉效果。

## 目录结构

```text
My_FPS/
|-- Config/                         # 项目配置
|   |-- DefaultEngine.ini            # 默认地图、GameMode、渲染与音频配置
|   |-- DefaultGame.ini
|   `-- DefaultInput.ini             # Enhanced Input 基础配置
|-- Content/
|   |-- NewMap.umap                  # 默认测试地图
|   |-- Code/                        # 项目核心蓝图与输入资产
|   |   |-- AnimNotify/
|   |   |-- BlueprintInterface/
|   |   |-- Physics/
|   |   |-- Player/
|   |   `-- Weapon/
|   |-- Decals/                      # 弹孔贴花、材质与纹理
|   |-- ShotSystem/                  # 射击音效、命中特效、静态网格等资源
|   |-- XL_FPSPack/                  # FPS 角色、武器、动画、音效和 VFX 素材包
|   |-- __ExternalActors__/          # World Partition 外部 Actor 数据
|   `-- __ExternalObjects__/         # World Partition 外部 Object 数据
|-- My_FPS.uproject                  # Unreal 项目文件
|-- .gitattributes                   # Git LFS 规则
`-- .gitignore                       # Unreal 生成文件忽略规则
```

## 核心资产说明

| 路径 | 说明 |
| --- | --- |
| `Content/Code/Player/BP_FPSPlayer` | 第一人称玩家角色蓝图 |
| `Content/Code/Player/System/BP_FPSGameMode` | 项目默认 GameMode |
| `Content/Code/Player/System/Input/IMC_FPSInput` | FPS 输入映射上下文 |
| `Content/Code/Weapon/Base/BP_WeaponBase` | 武器基类 |
| `Content/Code/Weapon/BP_VIRTUS` | MCX-VIRTUS 具体武器蓝图 |
| `Content/Code/Weapon/BP_Casing` | 弹壳表现蓝图 |
| `Content/Code/Weapon/MetaSound/MS_VIRTUS` | VIRTUS 武器开火音效资源 |
| `Content/Decals/BP_BulletDecal` | 普通材质弹孔贴花蓝图 |
| `Content/Decals/BP_BulletDecal_Glass` | 玻璃材质弹孔贴花蓝图 |
| `Content/ShotSystem/Shot_VFX` | 命中、枪口、碎片等射击视觉效果 |
| `Content/ShotSystem/Shot_SFX` | 武器、命中、弹壳和材质反馈音效 |
| `Content/XL_FPSPack/Weapons/MCX-VIRTUS` | MCX-VIRTUS 模型、材质、动画和音效 |

## 参考教程路线

项目主要参考的教程页面包含宣导课和第 01 至第 25 课，路线覆盖：

- 创建项目与导入素材。
- 修改 DDC 缓存路径。
- 配置 GameMode 与输入系统。
- 实现角色基础能力与能力系统。
- 制作玩家和武器动画系统。
- 模块化设计与调试。
- 完善瞄准系统与灵敏度系统。
- 制作开火蒙太奇动画。
- 使用 MetaSound 实现武器音效。
- 添加 Niagara 开火特效。
- 实现弹壳生成、后坐力、击中特效和命中音效。
- 制作弹孔痕迹。
- 制作换弹系统。
- 制作武器 UI 界面。

当前仓库中的资源和蓝图重点对应角色、武器、瞄准、开火、换弹、射击反馈和弹孔系统。若继续跟进教程后续内容，可以在现有结构上继续扩展 UI、更多武器、敌人、伤害、关卡目标和打包优化等模块。

## 运行方式

1. 安装 Unreal Engine 5.7。
2. 如果从 Git 克隆项目，先安装并拉取 Git LFS 资源：

   ```powershell
   git lfs install
   git lfs pull
   ```

3. 双击打开 `My_FPS.uproject`。
4. 等待项目资源加载、着色器编译和派生数据缓存生成。
5. 打开默认地图 `NewMap`，或直接使用项目配置中的启动地图。
6. 在编辑器中点击 Play 进行测试。

## 操作说明

项目输入基于 Enhanced Input 配置，具体键位以 `IMC_FPSInput` 中的设置为准。当前输入动作包括：

- `IA_Move`：角色移动。
- `IA_Look`：视角控制。
- `IA_Run`：奔跑。
- `IA_Aim`：瞄准。
- `IA_Fire`：开火。
- `IA_Reload`：换弹。

如果需要调整键位，请在 Unreal Editor 中打开 `Content/Code/Player/System/Input/IMC_FPSInput` 修改对应映射。

## 开发与版本管理说明

- `.uasset` 和 `.umap` 已通过 `.gitattributes` 配置为 Git LFS 管理，适合保存 Unreal 二进制资产。
- `Binaries/`、`Build/`、`DerivedDataCache/`、`Intermediate/`、`Saved/` 等 Unreal 自动生成目录已在 `.gitignore` 中排除。
- 项目资源较多，首次打开时可能需要较长时间编译着色器和生成缓存。
- 建议不要直接提交本地缓存、日志、IDE 配置或打包产物。

## 后续可扩展方向

- 完成或增强武器 UI，包括弹药显示、准星、状态提示等。
- 增加更多武器，并让 `BP_WeaponBase` 支持更通用的射击参数。
- 增加敌人、受击、生命值和伤害判定系统。
- 增加可交互靶场或训练关卡目标。
- 继续完善不同物理材质下的命中特效和音效差异。
- 加入武器散布、后坐力曲线、换弹中断、空仓挂机等更细节的 FPS 机制。
- 整理打包流程，形成可运行的演示版本。

## 素材与版权说明

本项目用于学习 Unreal Engine 5 FPS 项目制作流程。项目中使用的教程、素材包、模型、动画、音效和特效资源应遵守其原始来源的授权协议。若需要公开发布、二次分发或商业使用，请先确认所有第三方资源的授权范围。
