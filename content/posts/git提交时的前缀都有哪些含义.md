---
title: "git提交时的前缀都有哪些含义"
date: 2024-07-19
draft: false
tags : [                    # 文章所属标签
    "Git"
]
categories : [              # 文章所属标签
    "技术",
]
---


在Git中，提交（commit）时使用的feat并不是一个Git本身定义的命令或关键字，而是遵循某种提交消息规范（如Angular的提交信息规范）时，用来表示提交类型的一种约定俗成的标记。这种规范通常用于帮助维护者快速理解每次提交的目的和范围，以及自动化生成变更日志（changelog）。

feat代表“feature”，即新功能。当你使用feat作为提交消息的一部分时，你正在向项目添加一个新的功能或特性。这是语义化版本控制（Semantic Versioning, SemVer）的一部分，它帮助开发者和用户理解项目的变化如何影响项目的版本号。

例如，一个包含feat的Git提交消息可能看起来像这样：

- feat: 添加用户登录功能  
  
这个提交实现了用户的登录功能，包括用户名和密码的验证。
除了feat之外，还有其他类型的标记，比如：

- fix：表示修复了一个bug。
- docs：表示仅修改了文档。
- style：表示仅修改了代码的格式（比如空格、分号、缩进等），不改变代码逻辑。
- refactor：表示重构了代码，但没有添加新功能或修复bug。
- perf：表示提高了性能。
- test：表示增加了测试。
- chore（或choreograph、choreo等变体）：表示对项目结构或工具的更改，例如更新依赖项或配置文件。

遵循这样的提交消息规范有助于保持项目历史的清晰和一致性，特别是在大型项目或多人协作的项目中。它还有助于自动化工具（如commitizen、semantic-release等）的集成，这些工具可以根据提交消息来自动执行诸如生成变更日志、更新版本号等操作。