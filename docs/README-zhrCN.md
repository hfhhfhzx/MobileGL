<h1 align="center">MobileGL</h1>

<p align="center">
[English](./README.md) | **简体中文**
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Language-C%2B%2B-00599c?style=flat&logo=c%2B%2B" alt="C++">
  <img src="https://img.shields.io/badge/License-GNU%20LGPL%203.0-00399c?style=flat" alt="GNU LGPL 3.0">
  <img src="https://img.shields.io/badge/Status-Development-0078d7?style=flat" alt="Development">
</p>

<p align="center"><em>
桌面 OpenGL 实现
</em></p>

MobileGL 是一个 *免费* 且 *开源* 的项目，实现了桌面 **OpenGL** API。目标是提供一个完整的桌面OpenGL实现，包含状态管理层和多后端支持。

> [!NOTE]
>
> **状态：** 开发中。代码库的部分内容还不完整。当前短期目标：**OpenGL 3.3 (核心配置)**.

## 项目定位

MobileGL 是桌面 OpenGL 库的实现。其目标是：

* 完整 OpenGL 状态管理
* 一个揭露 OpenGL 函数的前端
* 多个独立的后端实现，每个后端针对特定的图形 API，并与其他后端完全隔离

本项目旨在作为一个实现/翻译层

## 关键组成部分

该仓库由以下顶层模块组织：

1. **MG_State** — 图形 API 的状态跟踪与管理逻辑
2. **MG_Impl** — 与 `MG_State` 和 `MG_Backend` 交互的图形 API 前端实现
3. **MG_Backend** — 每个后端的翻译层，将前端图形 API 的语义和状态映射为具体的后端 API 调用（例如 OpenGL ES、Vulkan）
4. **MG_Util** 及其他实用模块

## 第三方组件

MobileGL重新使用了几个开源项目：

* **SPIRV-Cross** by **KhronosGroup** - [Apache License 2.0](https://github.com/KhronosGroup/SPIRV-Cross/blob/master/LICENSE): [github](https://github.com/KhronosGroup/SPIRV-Cross)
* **glslang** by **KhronosGroup** - [Various Licenses](https://github.com/KhronosGroup/glslang/blob/main/LICENSE.txt): [github](https://github.com/KhronosGroup/glslang)
* **DiligentCore** by **Diligent Graphics** - [Apache License 2.0](https://github.com/DiligentGraphics/DiligentCore/blob/master/License.txt): [github](https://github.com/DiligentGraphics/DiligentCore)

各组件的确切许可文本请参考其各自仓库。本仓库中包含的任何第三方代码均遵循其上游项目的许可证

## 兼容性 & 目标

* **短期目标：** `OpenGL 3.3 (核心配置)`.
* **当前发展重点**
  * 针对`OpenGL 3.3 (核心配置)` 的 `MG_State` 和 `MG_Impl`
  * `Direct (Vulkan)` 后端
  * `Direct (OpenGL ES)` 后端

## 为何创建分支？

上游“currently provide **no releases** and **no precompiled binaries**.”

此分支提供一个 Github Workflow 来构建二进制文件，并发布到 Github Artifact 和 Release

还提供了 FCL 渲染器插件(尽管很简陋)，见[这里](https://github.com/hfhhfhzx/MobileGL-Plugin)

## 构建说明

我们（上游）目前不提供**任何版本**和**预编译的二进制文件**

如果你现在想尝试这个项目，你需要自己动手构建：

1. 克隆仓库：

   ```sh
   git clone https://github.com/MobileGL-Dev/MobileGL.git
   ```

2. 递归地初始化并更新所有子模块：

   ```sh
   git submodule update --init --recursive
   ```

3. 请按照 glslang 自己的文档了解其所需的入门流程

4. 使用 CMake 来配置和构建项目：

   ```sh
   cmake -B build
   cmake --build build
   ```
   
   或者使用现代的方式：
   
   ```sh
   cmake -S . -B build -G Ninja -DCMAKE_C_COMPILER=clang -DCMAKE_CXX_COMPILER=clang++
   cmake --build build
   ```
   
   另外，你也可以根据需要使用平台特定的构建命令。
   
   完整构建方案：参考 [Workflow](../.github/workflows/build.yml)

## 构建选项

| 选项                       | 描述                                           | 默认 |
|------------------------------| ----------------------------------------------------- | ------- |
| `MOBILEGL_BUILD_TEST`        | 构建 MobileGL 测试 (需要 Clang)                 | 开启      |
| `MOBILEGL_BUILD_BENCHMARK`   | 构建 MobileGL 基准测试 (需要 Clang)            | 开启      |
| `MOBILEGL_FORCE_RELEASE_OPT` | 在 Debug 构建中启用 O3 和 LTO                      | 开启      |
| `MOBILEGL_ENABLE_TRACY`      | 启用 Tracy 分析器进行性能分析        | 关闭     |

   **注意：**

* 该项目需要 C++23.
* `MG_Test` 和 `MG_Benchmark` 只能使用 Clang 构建，不能使用 GCC. 要强制使用 Clang，请在命令中添加 `-DCMAKE_C_COMPILER=clang -DCMAKE_CXX_COMPILER=clang++`
* 在 Android上， 测试和基准测试始终被禁用

## 环境变量

MobileGL 支持通过环境变量进行运行时的配置

### 支持的 Key

| 变量                | 介绍                                      | 允许的值                       | 默认        |
|-------------------------|--------------------------------------------------|--------------------------------------|----------------|
| `MOBILEGL_BACKEND_TYPE` | 启动时选择主动后端实现。	 | `DirectGLES`, `DirectVulkan`         | `DirectGLES`   |

## 须知

* MobileGL 目前**尚未**达到生产就绪状态
* 部分 **OpenGL 3.3（核心配置）** 功能仍然缺失或正在开发中

## 许可证

本项目采用 **GNU LGPL v3.0** 许可证发布。详细信息请参阅仓库中的 `LICENSE` 文件
