[link: 官网|github](https://github.com/LeetCode-OpenSource/vscode-leetcode/blob/master/docs/README_zh-CN.md)

- 无法登录 LeetCode 节点的临时解决办法
- 运行条件
  - [VS Code 1.23.0+](https://code.visualstudio.com/)
  - [Node.js 10+](https://nodejs.org/)
- 



# VSCode中安装LeetCode插件

PATH路径：注意：请确保`Node`在`PATH`环境变量中。您也可以通过设定 `leetcode.nodePath` 选项来指定 `Node.js` 可执行文件的路径

## 登录账户

[link: 掘金](https://juejin.cn/post/6844904105782018055#heading-2)

1 国际账户 VS 中国 (2018年进入中国)

- 快捷键shift + ctrl(command) + p 搜索leetcode 

2 sign in：邮箱登录 VS 第三方登录 VS cookie登录

- 邮箱登录有问题



## 配置LeetCode插件

1 我们每个问题代码存放的文件路径，方便以后我们找到这些写好的代码

这个配置名字叫做**leetcode.workspaceFolder**，默认的路径是$HOME/.leetcode。这里的HOME是你系统的环境变量，不同的系统这个变量指定的位置不一样。

![image-20210304220647244](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210304220656.png)

可以打开终端输入：`echo $HOME`



2 编辑器的快捷方式：显示Solution

在**leetcode.editor.shortcuts**配置当中进行修改

![image-20210304220811461](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210304220811.png)

支持五种不同的快捷方式（Code Lens）:

- `Submit`: 提交你的答案至 LeetCode；
- `Test`: 用给定的测试用例测试你的答案；
- `Star`: 收藏或取消收藏该问题；
- `Solution`: 显示该问题的高票解答；
- `Description`: 显示==该问题的题目描述==。

| 配置项名称                        | 描述                                                         | 默认值             |
| --------------------------------- | ------------------------------------------------------------ | ------------------ |
| `leetcode.hideSolved`             | 指定是否要隐藏已解决的问题                                   | `false`            |
| `leetcode.showLocked`             | 指定是否显示付费题目，只有付费账户才可以打开付费题目         | `false`            |
| `leetcode.defaultLanguage`        | 指定答题时使用的默认语言，可选语言有：`bash`, `c`, `cpp`, `csharp`, `golang`, `java`, `javascript`, `kotlin`, `mysql`, `php`, `python`,`python3`,`ruby`, `rust`, `scala`, `swift`, `typescript` | `N/A`              |
| `leetcode.useWsl`                 | 指定是否启用 WSL                                             | `false`            |
| `leetcode.endpoint`               | 指定使用的终端，可用终端有：`leetcode`, `leetcode-cn`        | `leetcode`         |
| `leetcode.workspaceFolder`        | ==指定保存文件的工作区目录==                                 | `""`               |
| `leetcode.filePath`               | 指定生成题目文件的相对文件夹路径名和文件名。点击查看[更多详细用法](https://github.com/LeetCode-OpenSource/vscode-leetcode/wiki/自定义题目文件的相对文件夹路径和文件名)。 |                    |
| `leetcode.enableStatusBar`        | 指定是否在 VS Code 下方显示插件状态栏。                      | `true`             |
| `leetcode.editor.shortcuts`       | ==指定在编辑器内所自定义的快捷方式==。可用的快捷方式有: `submit`, `test`, `star`, `solution`, `description`。 | `["submit, test"]` |
| `leetcode.enableSideMode`         | 指定在解决一道题时，是否将`问题预览`、`高票答案`与`提交结果`窗口集中在编辑器的第二栏。 | `true`             |
| `leetcode.nodePath`               | 指定 `Node.js` 可执行文件的路径。如：C:\Program Files\nodejs\node.exe | `node`             |
| `leetcode.showCommentDescription` | 指定是否要在注释中显示题干。                                 | `false`            |



## 管理文档

- 点击位于 VS Code ==底部状态栏==的 `LeetCode: ***` 管理 `LeetCode 存档`。你可以**切换**存档或者**创建**，**删除**存档。



## 各语言对应版本和环境

|            |                     |                                                              |
| ---------- | ------------------- | ------------------------------------------------------------ |
| 语言       | 版本                | 详细                                                         |
| C++        | g++ 8.2             | C++14 标准                                                   |
| Java       | java 1.8.0          |                                                              |
| Python     | python 2.7.12       |                                                              |
| Python3    | Python 3.6.7        |                                                              |
| MySQL      | mysql-server 5.7.21 |                                                              |
| C          | gcc 8.2             | 您可以使用 [uthash](https://troydhanson.github.io/uthash/) 来操作哈希表。 `"uthash.h"` 已经被默认包含了。 |
| C#         | mono 5.18.0         | `/debug` 标记执行，使用 [C# 7 的完整支持](https://docs.microsoft.com/zh-cn/dotnet/csharp/whats-new/csharp-7)。 |
| JavaScript | nodejs 10.15.0      | 使用 `--harmony` 标记执行，开启 [ES6 功能](http://node.green/)。 [lodash.js](https://lodash.com/) 库已经被默认包含了。 |
| Ruby       | ruby 2.4.5          |                                                              |
| Rust       | 1.31.0              | edition = 2018，支持 crates.io 的 [rand](https://crates.io/crates/rand) |
| Bash       | bash 4.3.30         |                                                              |
| Swift      | swift 4.2           |                                                              |
| Go         | go 1.11.4           |                                                              |
| Scala      | Scala 2.11.6        |                                                              |
| Kotlin     | Kotlin 1.2.50       |                                                              |
| PHP        | PHP 7.2             |                                                              |





# 问题|nodeJS版本

报错：Warning: Accessing non-existent property ‘padLevels‘ of module exports inside circular dependency



1 [Windows统一开发环境的基础-Chocolatey](https://zhuanlan.zhihu.com/p/53421288)

- 没用过苹果，但是一心希望windows能有一个类似apt-get或者yum的工具

Chocolatey是Windows上的包管理工具，就是安装软件包的。开发人员可以用来安装和配置自己的开发环境，例如我需要的JDK、**Node**、git、Chrome、VS Code、Android Studio、IntelliJ IDEA、WebStorm、7-zip、Hyper....



2 官网下载

[官网低版本](https://nodejs.org/en/blog/release/v10.24.0/)



3 使用nvm安装和管理不同的版本⭐

[link: 思否|windows下切换node版本](https://segmentfault.com/a/1190000015962368)

版本可切换



## nodeJS额外工具

[link: 官网|github](https://github.com/nodejs/node-gyp#on-windows)

没必要勾选：只要安装基本的nodeJS即可刷题





# 问题|报错http error [code=404]

验证邮箱或手机可以解决