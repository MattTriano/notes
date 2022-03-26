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


```bash
git log --format=raw
commit fa2d1cb7bf57f2d6b705532fa6582adb07a808d1
tree ea5cf8a680d24dfac000c0effdca12a73999c0d3
parent 6c89636d630a9832059b9197e3339e283a9203cc
author matttriano <matt.triano@gmail.com> 1648167818 -0500
committer matttriano <matt.triano@gmail.com> 1648167818 -0500

    Took some notes on Terraform and Infra as Code as a concept.
```

raw logs added in the hash of the tree and parent of that commit

We can also pull up that specific commit via the `cat-file`

```bash
$ git cat-file -p fa2d1
tree ea5cf8a680d24dfac000c0effdca12a73999c0d3
parent 6c89636d630a9832059b9197e3339e283a9203cc
author matttriano <matt.triano@gmail.com> 1648167818 -0500
committer matttriano <matt.triano@gmail.com> 1648167818 -0500

Took some notes on Terraform and Infra as Code as a concept.
```
## Porcelain commands vs Plumbing commands

The analogy is to toilets. The porcelain is the high level user interface, while the plumbing is the lower level infrastructure. 

Examples of porcelain commands
* `git add`
* `git commit`

Examples of plumbing commands
* `git cat-file -p`

### Git Objects

There are 4 differe "types" of git **objects**:
* commit
* tree
* blob
* annotated tag

## Trees

A tree is a container for blobs and trees. 

## Blobs

Blobs are binary files marked with a hash of the file's contents. It is deterministic. Repeatedly hashing (specifically, git uses SHA-1 hashes for this) a given input string will output the same hash.

```bash
$ echo "this is a blob" | git hash-object --stdin
046e12048379261aa0f93d263701c95192ba6e42
```




























end