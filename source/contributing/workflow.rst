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

Within your local repository, Git will have created an alias, **origin**, for the 
Github repository it is bound to. You want to create an alias for the shared
repository, so that you can "synch" the two, making sure that your repository
includes any other contributions that have been merged by us into the shared repo::

    git remote add upstream UPSTREAM_URL

Then synchronizing is done by pulling from us and pushing to you. This is normally
done locally, so that you can resolve any merge conflicts. For instance, to 
synchronize **develop** branches::

    git checkout develop
    git pull upstream develop
    git push origin develop

You might get merge conflicts when you pull from upstream. It is your responsibility
to resolve those locally, so that you can continue collaborating with the shared
repository. Basically, the shared repository is updated in the order that contributions 
are merged into it, not in the order that they might have been submitted. 
If two PRs update the same piece of code, then the first one to be merged 
will take precedence, even if it causes problems for other contributions.

It is a good idea to synchronize repositories when the shared one changes.

Branching Revisited
===================

The top of this page talked about the **master** and **develop** branches. 
The *best practice* for your work is to create a *feature branch* locally,
to hold a group of related changes (source, unit testing, documentation,
change log, etc). This local branch should be named appropriately,
for instance "fix/problem123" or "new/mind-reader".

For instance, make sure you are in the *develop* branch, and create a
new feature branch, based on *develop*, for a new feature you are creating::

    git checkout develop
    git checkout -b new/mind-reader

Saving changes only updates your local working area.

Committing
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
