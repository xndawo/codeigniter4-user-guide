=====================
贡献的工作流程
=====================

大部分为 CodeIgniter（或其它项目）提供贡献的工作流程都需要你了解 `Git <https://git-scm.com/>`_ 是如何管理一个共享仓库，
及如何为其提供贡献的。下面的例子使用 Git 的 bash shell 来尽可能地保持平台无关性，你用 IDE 干这些活可能更容易些。

下面是用到的一些约定，如果你要用它们请为它们提供合适的值 ::

    ALL_PROJECTS    // 存放你所有项目（一个项目一个子文件夹）的文件夹，如 /lampp/htdocs
    YOUR_PROJECT    // 你的单个项目文件夹, 在 ALL_PROJECTS 文件夹之内
    ORIGIN_URL      // 你 fork 来的仓库的克隆地址
    UPSTREAM_URL    // CodeIgniter4 仓库的克隆地址

分支
=========

CodeIgniter 使用 `Git-Flow
<http://nvie.com/posts/a-successful-git-branching-model/>`_ 分支模式，这个模式要求将所有的拉取请求都发送到“develop”分支。
这个分支是放下一个计划开发版的地方。“master”分支将始终包含最新稳定版并保持清洁，因此可以将“hotfix”（如：紧急安全修补程序）
应用于“master”分支来创建一个新版本，而无需担心其它分支受到影响。因此，必须将所有提交都应用到“develop”，任何发送给“master”的提
交都会被自动关闭。 如果你有多个更改需要提交，请将每个更改放入你 fork 的各自分支中去。

一次办一件事：一个拉取请求应该只包含一个更改。这并不是只能有一个提交的意思，是一次更改，但可以包含许多提交。这样规定的理由是，
假如你有两个更改 X 和 Y，但同时发送了这两者的拉取请求，可能我们真正想要的是 X 而不想要 Y ，这使得我们无法合并请求。
应用 Git-Flow 模式，你可以为这两个更改创建新分支并分别发送请求。

叉取
=======

你通常使用的是 CodeIgniter4 的一个 fork，这个 fork 是你 github 账号下，我们仓库的一个副本。你可以对你 fork 来的仓库进行更改，
但不能对原始仓库这样干，如果你想更改原始仓库，请提交拉取请求。

通过 Github 网站就可以 `创建一个 fork <https://help.github.com/articles/fork-a-repo>`_ 。 导航到 `我们的仓库 <https://github.com/bcit-ci/CodeIgniter4>`_，单击页面右上角的 Fork 按钮，然后选择用哪个帐号或组织来包含该 fork。

克隆
=======

你 *可以* 使用 GitHub 的 web 界面处理你的仓库，但这很笨拙，大多数开发者会将他们的仓库克隆到本地系统，然后在本地使用它。

在 Github 上，导航到你 fork 来的仓库，单击 **Clone** 或 **Download**，然后复制显示的克隆地址，我们称这个地址为 ORIGIN_URL

将你的仓库克隆到本地使用::

    cd ALL_PROJECTS
    git clone ORIGIN_URL

同步
========

在你的本地仓库中，Git 会为它所绑定的 Github 仓库（你的 fork 库）创建一个别名：**origin**。 你应该为共享库（官方库）创建一个别名，
以便“同步”这两个仓库，来确保你的仓库包含由我们合并到共享库中的任何其他贡献::

    git remote add upstream UPSTREAM_URL

然后通过拉取我们给你的推送来完成同步。这个过程通常在本地完成，这样你就可以解决任何合并冲突。例如，同步 **develop** 分支::

    git checkout develop
    git pull upstream develop
    git push origin develop

当你从 upstream 中拉取时可能会遇到合并冲突。你有责任在本地解决这些问题，这样你就可以继续与共享库合作。基本上，共享库按贡
献被合并的顺序更新，而不是按已提交的顺序更新。如果有2个拉取请求修改了同一块代码，也是首先被合并的贡献优先更新到共享库，哪怕因此导致另外一个贡献出问题。

一旦共享库有更改，同步仓库是个不错的主意。

重新审视分支
===================

在本页的最前面讨论过了 **master** 和 **develop** 分支。对于你的工作，*最佳实践* 是创建一个 *feature 本地分支* ，来保存一组相关的更改
（源代码，单元测试，文档，变更日志等）。这个本地分支应该有个合适的命字，例如“fix/problem123”或“new/mind-reader”。

例如，确保你在 *develop* 分支下，然后创建一个新功能分支，再从 *develop* 分支切换到你创建的新功能分支::

    git checkout develop
    git checkout -b new/mind-reader

更新你的本地工作区域才能保存更改。

提交
==========

你的本地更改必须提交保存到你的本地仓库。这就是你 `贡献签入 <signing>`_ 的地方。
你可以在分支中一直提交到它对为止。
例如，从一个调试会话中提交你的工作::

    git add .
    git commit -S -m "Find and fix the broken reference problem"

只要确保你在功能分支中的提交都是相关的。


如果你同时在处理两个功能，那么你要在它们之间切换来保持贡献间的隔离，例如::

    git checkout new/mind-reader
    // work away
    git add .
    git commit -S -m "Added adapter for abc"
    git checkout fix/issue-123
    // work away
    git add .
    git commit -S -m "Fixed problem in DEF\Something"
    git checkout develop

最后一个 checkout 确保你最终进入*develop*分支，作为下一次与你仓库会话的起点。这是一个良好的实践，因为你不会总是知道自己在哪个分支下。

推送你的分支
===================

有时，你可能想知道自己的功能分支是否完整，或想从其它开发者的审核中受益。

**注意 :**
    请记住在推送前将你的本地库与共享库同步！ 在这个阶段解决冲突要容易得多。

同步你的仓库::

    git checkout develop
    git pull upstream develop
    git push origin develop
    
更新你的功能分支::

    git checkout new/mind-reader
    git merge develop

最后将你的本地分支推送到 github 仓库::

    git push origin new/mind-reader

拉取请求
=============

在 Github 上，切换到你想贡献的分支，单击“New pull request”,建议你每次修改一个功能分支。

确保你的拉取请求是在共享的开发分支上，否则可能会被拒绝。

确保拉取请求的标题对维护者和其它开发者是有帮助的。添加合适的注释，如：要求审核。

**注意 :**
    如果你的拉取请求没有标题，会有极大的可能被拒绝。

当你拉曲请求提交后，会触发一个持续集成任务，执行所有单元测试以及我们为其配置的任何其他检查。 如果单元测试失败，或者存在合并冲突，那么在修复之前，你的拉取请求将不可合并。

在本地修复这些更改，正确提交它们，然后再次推送你的分支。 这将自动更新拉取请求，并重新运行CI测试。 你不需要提出新的拉取请求。

如果你的拉取请求没有遵循我们的贡献指南，或者不完整，代码库维护人员会对其给出评论，指出需要解决的问题。

清理
=======

如果你的拉取请求被接受并合并到共享库，你可以从 github 仓库及本地库中删除该分支。
