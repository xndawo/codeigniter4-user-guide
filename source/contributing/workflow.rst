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

你使用的是 CodeIgniter4 的一个 fork，这个 fork 是你 github 账号下，我们仓库的一个副本。你可以对你 fork 来的仓库进行更改，
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

在你的本地仓库中，Git会为它所绑定的Github仓库（你的 fork 库）创建一个别名：**origin**。 你希望为共享库（官方库）创建一个别名，
以便“同步”这两个仓库，来确保你的仓库包含由我们合并到共享库中的任何其他贡献::

    git remote add upstream UPSTREAM_URL

然后通过我们拉取并推送给你来完成同步。这个过程通常在本地完成，这样你就可以解决任何合并冲突。例如，同步 **develop** 分支::

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

例如，切换到 *develop* 分支下，然后创建一个新功能分支，并切换到新功能分支下::

    git checkout develop
    git checkout -b new/mind-reader

更新你的本地工作区域才能保存更改。

提交
==========

Your local changes need to be *committed* to save them in your local repository.
This is where `contribution signing <signing>`_ comes in.

You can have as many commits in a branch as you need to "get it right".
For instance, to commit your work from a debugging session::

    git add .
    git commit -S -m "Find and fix the broken reference problem"

Just make sure that your commits in a feature branch are all related.

If you are working on two features at a time, then you will want to switch
between them to keep the contributions separate. For instance::

    git checkout new/mind-reader
    // work away
    git add .
    git commit -S -m "Added adapter for abc"
    git checkout fix/issue-123
    // work away
    git add .
    git commit -S -m "Fixed problem in DEF\Something"
    git checkout develop

The last checkout makes sure that you end up in your *develop* branch as a
starting point for your next session working with your repository.
This is a good practice, as it is not always obvious which branch you are working in.

Pushing Your Branch
===================

At some point, you will decide that your feature branch is complete, or that
it could benefit from a review by fellow developers.

.. note::
    Remember to synch your local repo with the shared one before pushing!
    It is a lot easier to resolve conflicts at this stage.

Synchronize your repository::

    git checkout develop
    git pull upstream develop
    git push origin develop
    
Bring your feature branch up to date::

    git checkout new/mind-reader
    git merge develop

And finally push your local branch to your github repository::

    git push origin new/mind-reader

Pull Requests
=============

On Github, you propose your changes one feature branch at a time, by
switching to the branch you wish to contribute, and then clicking
on "New pull request".

Make sure the pull request is for the shared **develop** branch, or it
may be rejected.

Make sure that the PR title is helpful for the maintainers and other developers.
Add any comments appropriate, for instance asking for review.

.. note::
    If you do not provide a title for your PR, the odds of it being summarily rejected
    rise astronomically.

When your PR is submitted, a continuous integration task will be triggered,
running all the unit tests as well as any other checking we have configured for it.
If the unit tests fail, or if there are merge conflicts, your PR will not
be mergeable until fixed.

Fix such changes locally, commit them properly, and then push your branch again.
That will update the PR automatically, and re-run the CI tests. You don't need 
to raise a new PR.

If your PR does not follow our contribution guidelines, or is incomplete,
the codebase maintainers will comment on it, pointing out what
needs fixing. 

Cleanup
=======

If your PR is accepted and merged into the shared repository, you can delete
that branch in your github repository as well as locally.
