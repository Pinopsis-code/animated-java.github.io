---
title: 你的第一个蓝图
description: 学习如何创建你的第一个蓝图并将它导出到 Minecraft。
---

# 你的第一个蓝图

在这篇指南中我们将创建、导出并在 Minecraft 中生成一个旋转方块，这包含完整的 Animated Java 的工作流程。

## 前提条件

-   已经安装了 Animated Java。若没有安装请前往[安装 Animated Java](/docs/getting-started/installing)。
-   一个 Minecraft 存档，且其 `datapacks` 文件夹中需要准备一个空的数据包。Minecraft 本体的 `resourcepacks` 文件夹中也需要准备一个空的资源包。它们都需要 `pack.mcmeta` 文件。

---

## 第一部分：创建蓝图

1. **创建新蓝图**

    前往 **文件 <i class="minecraft-right-arrow"></i> 新建 <i class="minecraft-right-arrow"></i> Blueprint**，或在开始界面选择 **Blueprint**。

2. **添加一个分组**

    在大纲面板点击 **添加分组** 并重命名分组为 `cube_bone`。分组是绑定体的骨骼，它们决定哪些部分可以做动画。

3. **添加一个方块**

    点击 **添加块** 并在大纲中将该块拖到 `cube_bone` 上。你可以随意调整其位置与大小。

4. **添加纹理**

    在 **纹理** 面板创建或导入纹理，并将其应用到方块的每个面。

---

## 第二部分：配置蓝图

Animated Java 需要知道在什么地方导出包。

1. **打开 Blueprint 设置**

    在菜单栏中点击 **Animated Java <i class="minecraft-right-arrow"></i> Blueprint Settings**。

2. **设定 Blueprint ID**

    在 **General** 界面设定一个独一无二的 **Blueprint ID**，比如`my_pack:spinning_cube`。Animated Java 生成的所有实体与函数都将依赖此ID。

3. **设定 Minecraft 版本**

    将 **Minecraft 版本**设为需要的版本，比如 `26.2`。

4. **选择资源包位置**

    在 **Resource Pack** 页面，选择 format 为 **Folder** ，并将目录位置设为你的资源包文件夹（包含 `pack.mcmeta` 的文件夹）。

5. **选择数据包位置**

    在 **Data Pack** 页面，选择 format 为 **Folder** ，并将目录位置设为你的存档中 `datapacks` 文件夹下的数据包文件夹。

6. **关闭界面**

    切换或关闭设置界面时设置将自动保存。

---

## 第三部分：让它动起来

1. **切换到动画模式**

    点击 Blockbench 右上角的**动画模式**。

2. **创建一个动画**

    在**动画**面板点击 **+** 创建动画，将其命名为 `spin` 并将 **Loop Mode** 设为 **Loop**。

3. **添加旋转关键帧**

    选中 `cube_bone`。在第 `0` 秒添加一个旋转关键帧，并将 Y 设为 `0`。将时间线移至后方任意时间点，添加旋转关键帧并将 Y 设为 `360`。

---

## 第四部分：导出与测试

1. **导出**

    点击 **Animated Java <i class="minecraft-right-arrow"></i> Export (Debug)**，或按下 `Ctrl + E`， Animated Java 会将模型与材质导出到资源包；将函数导出到数据包。

2. **加载包**

    在 Minecraft 中加载资源包（可以按下 `F3 + T` 重加载），然后运行命令 `/reload`。

3. **生成绑定体**

    运行这个函数可以在玩家当前的位置生成朝向南方的方块:

    ```mcfunction
    execute positioned ~ ~ ~ rotated 0 0 run function my_pack:spinning_cube/summon {args: {}}
    ```

    :::note[说明]
    将 `my_pack:spinning_cube` 替换为你自己的 Blueprint ID。
    :::

4. **播放动画**

    ```mcfunction
    execute as @e[tag=my_pack.spinning_cube.root] run function my_pack:spinning_cube/animations/spin/play
    ```

5. **移除绑定体**

    ```mcfunction
    execute as @e[tag=my_pack.spinning_cube.root] run function my_pack:spinning_cube/remove/this
    ```

---

## 后续步骤

-   [蓝图](/docs/core-concepts/blueprints) 包含所有蓝图设置。
-   [变种](/docs/core-concepts/variants) 与 [材质槽位](/docs/core-concepts/texture-slots) 让一个绑定体显示不同外观。
-   [函数 API](/docs/function-api/summon) 列出了你可以在自己的数据包中调用的所有函数。
