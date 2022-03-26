# Git

## Staging (add, rm, mv)

`git add`


```bash
$ git --help
usage: git [--version] [--help] [-C <path>] [-c name=value]
           [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
           [-p | --paginate | --no-pager] [--no-replace-objects] [--bare]
           [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
           <command> [<args>]

These are common Git commands used in various situations:

start a working area (see also: git help tutorial)
   clone      Clone a repository into a new directory
   init       Create an empty Git repository or reinitialize an existing one

work on the current change (see also: git help everyday)
   add        Add file contents to the index
   mv         Move or rename a file, a directory, or a symlink
   reset      Reset current HEAD to the specified state
   rm         Remove files from the working tree and from the index

examine the history and state (see also: git help revisions)
   bisect     Use binary search to find the commit that introduced a bug
   grep       Print lines matching a pattern
   log        Show commit logs
   show       Show various types of objects
   status     Show the working tree status

grow, mark and tweak your common history
   branch     List, create, or delete branches
   checkout   Switch branches or restore working tree files
   commit     Record changes to the repository
   diff       Show changes between commits, commit and working tree, etc
   merge      Join two or more development histories together
   rebase     Reapply commits on top of another base tip
   tag        Create, list, delete or verify a tag object signed with GPG

collaborate (see also: git help workflows)
   fetch      Download objects and refs from another repository
   pull       Fetch from and integrate with another repository or a local branch
   push       Update remote refs along with associated objects

'git help -a' and 'git help -g' list available subcommands and some
concept guides. See 'git help <command>' or 'git help <concept>'
to read about a specific subcommand or concept.
```

## git log

```bash
git log
commit fa2d1cb7bf57f2d6b705532fa6582adb07a808d1 (origin/airflow_task_context)
Author: matttriano <matt.triano@gmail.com>
Date:   Thu Mar 24 19:23:38 2022 -0500

    Took some notes on Terraform and Infra as Code as a concept.
```


```bash
git log --format=fuller
commit fa2d1cb7bf57f2d6b705532fa6582adb07a808d1 (origin/airflow_task_context)
Author:     matttriano <matt.triano@gmail.com>
AuthorDate: Thu Mar 24 19:23:38 2022 -0500
Commit:     matttriano <matt.triano@gmail.com>
CommitDate: Thu Mar 24 19:23:38 2022 -0500

    Took some notes on Terraform and Infra as Code as a concept.
```

Most of the time, `AuthorDate` and `CommitDate` will be the same, but if you `cherry-pick` a commit from one branch onto another branch, which will change the `committer` and `CommitDate` will refer to the person who did the cherry-pick and the time they did it.









































end