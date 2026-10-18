---
title: 蓝图
description: 学习蓝图与其配置
---

# 蓝图

## 蓝图是什么？

**蓝图**（`.ajblueprint`）是 Animated Java 的 BlockBench 项目文件。它包含 Animated Java 建立动画模型所需的一切：模型、材质、动画、[变种](/docs/core-concepts/variants)、[材质槽位](/docs/core-concepts/texture-slots)与导出配置。

当蓝图[导出](/docs/core-concepts/exporting)时， Animated Java 将其转变为 **资源包** （模型与材质）与 **数据包** （游戏内模型生成、动画控制与绑定的命令）。

打开旧版本的蓝图时 Animated Java 将自动更新其版本。 `0.x` 版本的项目（`.ajmodel` 类型文件）也可以转换，见 [历史发布版本](/docs/resources/legacy-releases).

---

## 蓝图设置

在顶部目录中点击 **Animated Java <i class="minecraft-right-arrow"></i> Blueprint Settings** 打开蓝图设置。设置界面被分为多个页面，有些界面仅在特定 [目标环境](#target-environment) 中出现。切换界面或关闭都会保存修改。

### 常规

-   #### 目标环境

    你的模型在游戏中将如何运行：

    -   **Datapack** ：导出一个资源包与数据包。这是默认选项，也是大多数指南中描述的类型。
    -   **Plugin** ：导出单独的 JSON 文件供服务器插件读取。数据包、资源包、绑定与函数都由 [插件](#plugin) 替换。见 [插件导出](/docs/core-concepts/exporting#plugin-exports)。

-   #### 蓝图名

    蓝图的名字。同时也作为文件名。

-   #### 蓝图 ID

    一个命名空间的 ID (例如 `my_pack:my_rig`) 标识了该蓝图。所有生成的函数、标签、存储与模型都将存于此 ID 下。

    :::info[例子]
    `my_pack:my_rig` 会将函数放置于 `data/my_pack/function/my_rig/`，所以你需要使用`function my_pack:my_rig/summon`生成它。
    :::

    :::tip[提示]
    给每个蓝图设置单独的 ID。两个同 ID 的蓝图导出时会互相覆盖。
    :::

    :::danger[限制]

    -   必须包含命名空间与路径，例如 `namespace:path`。
    -   命名空间只允许使用小写字母、数字与下划线。路径还可以使用 `/`。
    -   不能使用 `minecraft` 命名空间。
    -   不能使用 `animated_java:global`。

    :::

-   #### 目标 Minecraft 版本

    需导出的 Minecraft 版本，例如 `26.2` (默认) 或 `1.20.4`。Animated Java 支持 **1.20.4** 及以上。

    这将决定包如何生成与你可以使用哪些特性。例如：

    -   [方块旋转](/docs/nodes/cubes#rotation-limitations) 在 1.21.11 版本前受限制。
    -   [材质槽位](/docs/core-concepts/texture-slots) 命令、材质槽位帧与 [物品模型属性](/docs/configs/item-model-properties) 需要 1.21.4 及以上版本。
    -   [使用生物堆栈](#use-entity-stacking) 只允许在 1.21.4 或更高版本关闭。

    修改后需重导出。

-   #### 材质尺寸

    UV 编辑器的分辨率。它需要与你使用的最大的材质尺寸相同且是 2 的整数倍，否则 Animated Java 将会发出警告。

### 资源包

仅在目标环境选为 **Datapack** 时出现。

-   #### 资源包导出格式

    -   **Folder**：将生成的文件放入已有的资源包文件夹。当项目正在制作时使用最佳。
    -   **Zip**：将生成的文件打包为单独的 `.zip` 资源包。
    -   **None**：不生成资源包。只在蓝图不包含方块时可用，例如仅使用 [物品展示](/docs/nodes/item-displays)、[方块展示](/docs/nodes/block-displays) 与 [文本展示](/docs/nodes/text-displays) 的模型。

-   #### 资源包文件夹 / 压缩包

    写入资源包的位置。文件夹必须包含 `pack.mcmeta` 文件。压缩包路径必须指向 `.zip` 类型文件。

-   #### 展示物品

    用于展示模型而替代覆盖的物品。只在 1.21.2 版本以下可见。新版本使用 `minecraft:item_model` 物品组件代替，所以不需要覆盖物品。

    需选择一个模型的父节点为 `minecraft:item/generated` 的物品，例如 `minecraft:white_dye` （默认）。不同的蓝图可以共用一个展示物品。

### 数据包

仅在目标环境选为 **Datapack** 时出现。

-   #### 数据包导出格式

    **Folder**、**Zip** 或 **None**，与 [资源包导出格式](#resource-pack-export-format) 相同。

-   #### 数据包文件夹 / 压缩包

    写入数据包的位置。文件夹必须包含 `pack.mcmeta` 文件。压缩包路径必须指向 `.zip` 类型文件。

-   #### 动画系统

    决定动画如何存储：

    -   **Functions** （默认）：为每一帧写一个函数。游戏内运行很快，但是会生成很多文件。
    -   **Storage** ：在 storage 中存储帧数据。生成的文件很少，但是运行稍慢。

### 模型绑定

仅在目标环境选为 **Datapack** 且数据包已经导出过之后才出现。

-   #### 自定义绑定实体标签

    额外的标签，使用逗号分隔。生成模型时它将会添加到 **每一个** 绑定的模型。

    :::note[例子]
    `my_pack.boss, my_pack.hostile`
    :::

-   #### 插值时长

    Minecraft 在动画帧之间平滑移动所需的刻数（即 NBT 标签 `interpolation_duration`）。更高的值看起来更平滑，但是反应更慢。默认为 `1`。

-   #### 传送时长

    Minecraft 在传送模型时平滑移动所需的刻数（即 NBT 标签 `teleport_duration`）。 Minecraft 最高允许值为 `59`，默认为 `1`。

-   #### 阴影半径

    根实体的阴影的半径。设为 `0`（默认）以禁用。

-   #### 阴影强度

    根实体的阴影的黑暗程度。只在阴影半径大于 `0` 时有效。默认为 `1`。

-   #### Auto Update Rig Orientation

    When **enabled** (the default), the rig follows its root entity every tick: bones turn to match the root's rotation, and floating entities (Locator entities, Cameras, and Interactions) move to their place on the rig. You can move a rig by teleporting its root entity.

    This is expensive. When **disabled**, the rig only moves when you call [`move`](/docs/function-api/utilities#move). Turn it off for rigs that don't move every tick.

-   #### Use Entity Stacking

    When **enabled** (the default), every bone entity rides the root entity as a passenger. You can reach them with `execute on passengers`, but their rotation is less precise.

    When **disabled**, bone entities are separate entities teleported into place every tick. Rotation is much more precise, so the rig turns smoothly while it moves. Use [`as_node`](/docs/function-api/utilities#as_node) to run commands as a bone.

    Turning this off requires Minecraft 1.21.4 or newer. Older versions always stack.

### Event Functions

Only shown when the Target Environment is **Datapack**.

Commands that run at specific points in the rig's life. Each one runs `as` and `at` the root entity, and supports [MC-Build](https://mcbuild.dev) syntax. Write them like a `.mcfunction` file.

-   #### On-Summon Function

    Runs at the end of the [`summon`](/docs/function-api/summon) function, after every node is set up.

-   #### On-Remove Function

    Runs at the start of [`remove/this`](/docs/function-api/remove#removethis), before the rig's entities are removed.

-   #### Pre-Tick Function

    Runs every tick, **before** Animated Java's own tick logic.

-   #### Post-Tick Function

    Runs every tick, **after** Animated Java's own tick logic.

### Plugin

Only shown when the Target Environment is **Plugin**.

-   #### JSON File

    Where to write the exported JSON file.

-   #### Baked Animations

    When enabled, every animation frame is calculated ahead of time and stored in the JSON file. When disabled, the raw keyframes are stored instead, and the plugin has to interpolate them itself. Some plugins require baked animations.

### Preview

Loads extra Resource Packs into the editor, so custom fonts, item and block models, and textures show up in your Blueprint's previews. Preview packs only affect the editor: they're never exported, and they don't replace your Blueprint's own Resource Pack.

-   #### Preview Resource Packs

    A list of Resource Packs, each one a folder containing `pack.mcmeta` or a `.zip` file. Packs higher in the list override packs below them. Use the arrows to reorder them.

    Click **Reload Preview** after changing a pack's files.

### Misc

The render box is the area Minecraft uses to decide whether your rig is on screen. If it's too small, parts of the rig vanish when the root entity is just out of view.

-   #### Preview Render Box

    Shows the render box in the viewport.

-   #### Auto Render Box

    Fits the render box to your model automatically. Recommended.

-   #### Render Box Size

    The render box's size, when Auto Render Box is off.
