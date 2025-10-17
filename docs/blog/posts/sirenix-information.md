---
date: 2025-10-17
categories:
  - sirenix
tags:
  - sirenix
authors: 
  - Yuumix
slug: sirenix-information
---

# Sirenix 官方信息记录

Odin Inspector 是 Sirenix 公司的一个 Unity 插件，用于在 Unity 编辑器中提供更强大的属性绘制和调试功能。Sirenix 在 Discord 上有一个活跃的社区，用户可以在其中获取支持和分享他们的插件使用经验。本文记录一些可能有用的信息。

<!-- more -->

## Odin Inspector 4.0 Visual Designer 设计理念

Odin Inspector 4.0 Visual Designer 定义了一个新的文本文件类型，`ovdf`。这个文件类型用于定义可视化设计师的布局和行为。

1. 用户可以发布包含 `ovdf` 文件的插件，而无需任何代码依赖。此类型的文本不会影响项目，只有在用户安装了 Odin Inspector 4.0 或更高版本时，才会使得 `ovdf` 文件生效。这样的话，使用或者支持 Odin Inspector 4.0 变得更加简单和灵活，无需依赖任何代码。

2. 可视化设计器，默认是不允许修改 Unity 拥有自定义编辑器的脚本的样式的，不打算支持修改自定义编辑器的脚本的样式，未来会确保无法编辑带有自定义编辑器的类型，这样可以避免一些潜在的问题。
