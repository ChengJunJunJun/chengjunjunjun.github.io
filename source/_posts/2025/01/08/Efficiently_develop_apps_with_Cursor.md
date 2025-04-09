---
title: 使用Cursor高效开发Apple应用
date: 2025-01-08
last_modified: 2025-01-09
author: Cheng Jun
desc: 作为一个ios开发者新手，Xcode是入门开发者的绊脚石，Cursor作为AI编程助手，可以大大提高开发效率。帮助大家体会到开发Apple应用的乐趣。
tags: [Cursor, Apple, iOS, Swift]
categories: Technical sharing
---

#### 在Cursor中配置Apple开发环境
##### 1，安装Swift、CodeLLDB，SweetPad插件
- 插件的作用可以参考相应的插件介绍，也可以使用ChatGPT来了解。

##### 2，配置 Swift LSP 能够正确识别项目中的代码
经过上面的步骤，如果你发现编辑器还是报错，很多地方提示：`Cannot find '***' in scope SourceKit`。这是因为 `swift` 的 `sourcekit-lsp` 没有把你的项目文件加入到索引中，所以编辑器找不到你的方法。可以通过下面的方法解决这个问题：

用 Xcode-Build-Server 配置项目
首先安装

```bash
brew install xcode-build-server
```
然后在项目根目录下根据你的项目文件类型执行对应的命令 :

```bash
xcode-build-server config -workspace *.xcworkspace -scheme <XXX> 
xcode-build-server config -project *.xcodeproj -scheme <XXX>
```
例如你用的是 exampleProject.xcodeproj：
```bash
xcode-build-server config -project exampleProject.xcodeproj -scheme exampleProject
```
重新启动编辑器，即可开始享受用 Cursor/VSCode 开发 iOS 项目。

##### 3，配置 SwiftUI 热重载
在使用 Cursor/VSCode 开发 iOS 项目时，一个主要的痛点是缺少 Xcode 的实时预览功能。不过，我们可以通过配置热重载（Hot Reload）来解决这个问题，不仅能实现实时预览 UI 变化，而且响应速度比 Xcode Preview 更快。

配置步骤如下：

1. 配置项目链接器设置
   - 在 Xcode 中打开项目的 Target 设置
   - 导航至 `Build Settings` -> 搜索 `Other Linker Flags`
   - 添加两个参数：`-Xlinker` 和 `-interposable`

2. 安装并配置 InjectionIII
   - 下载并安装最新版本的 InjectionIII
   - 启动后会在菜单栏显示注入图标
   - 选择你的项目目录
   - 点击菜单栏图标，执行 `Prepare Project` 命令

3. 注入代码说明
   InjectionIII 会自动为 SwiftUI 视图添加必要的注入代码：
   - `.enableInjection()` 方法
   - `@ObserveInjection var forceRedraw` 属性
   > 提示：如果不希望全局注入，可以手动为指定视图添加以上代码

运行项目后，终端会显示连接成功信息：
```bash
💉 InjectionIII connected ...
💉 Watching files under the directory ...
```

此时在 Cursor/VSCode 中编辑 SwiftUI 文件时，保存后可立即在模拟器中看到更新效果。这种开发方式不需要重新编译整个项目，显著提升了开发效率。

> 故障排除：如果 UI 没有及时更新，请查看 Xcode 控制台的错误信息进行诊断。

更多高级配置和故障排除指南，请参考 [Inject 官方文档](https://github.com/johnno1962/injectionforxcode)。

#### 其他常见配置和问题解决

##### 1. 代码格式化配置
- 通过 SweetPad 插件管理器安装 `swift-format`
- 支持的格式化选项：
  - 代码缩进
  - 空格规范化
  - 括号对齐
  - 导入语句排序

##### 2. Xcode 与 Cursor/VSCode 文件结构同步
Xcode 和 Cursor/VSCode 的文件组织方式存在差异：
- Xcode 使用虚拟的 Group 结构
- Cursor/VSCode 使用实际的文件系统结构

解决方案：
1. 在 Xcode 中选择需要同步的 Group
2. 右键选择 `Convert to Folder`
3. 重复以上步骤直到所有需要的 Group 都转换完成

> 注意：转换后的文件夹结构将在文件系统中实际存在，便于两个 IDE 之间的同步。

##### 3. 热重载故障排除
当遇到热重载失败时，可能会遇到几种常见问题：如果是 CoreData 发生变更，需要重启 InjectionIII；当 Packages 有更新时，需要清理构建并重新运行项目；如果项目的文件结构发生变动，则需要重新执行 `Prepare Project` 命令来更新配置。

#### 总结
通过以上的设置，就可以开心在Cursor中开发Apple应用了。本blog主要参考的是[JUNPING](https://blog.imjp.uk/fxxk-xcode)，感谢[原作者](https://blog.imjp.uk/)的分享。