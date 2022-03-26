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

## .git directory

```bash
(base) matt@matt:~/projects/notes/.git$ ls -la
total 60
drwxr-xr-x   8 matt matt 4096 Mar 26 12:11 .
drwxr-xr-x   8 matt matt 4096 Aug 29  2021 ..
drwxr-xr-x   2 matt matt 4096 Dec  1  2020 branches
-rwxr-xr-x   1 matt matt   61 Mar 24 19:23 COMMIT_EDITMSG
-rwxr-xr-x   1 matt matt  436 Mar 26 12:10 config
-rwxr-xr-x   1 matt matt   73 Dec  1  2020 description
-rw-rw-r--   1 matt matt  319 Mar 26 12:11 FETCH_HEAD
-rw-rw-r--   1 matt matt   37 Mar 26 12:10 HEAD
drwxr-xr-x   2 matt matt 4096 Dec  1  2020 hooks
-rw-rw-r--   1 matt matt 3398 Mar 26 12:10 index
drwxr-xr-x   2 matt matt 4096 Dec  1  2020 info
drwxr-xr-x   3 matt matt 4096 Dec  1  2020 logs
drwxr-xr-x 142 matt matt 4096 Mar 26 12:11 objects
-rw-rw-r--   1 matt matt   41 Mar 26 12:11 ORIG_HEAD
drwxr-xr-x   5 matt matt 4096 Dec  1  2020 refs
```

### /.git/objects
The objects folder conains many directories with 2-letter names, as well as `info` and `pack`.

```bash
matt@matt:~/projects/notes/.git/objects$ ls -t
08  1a  ea  36  13  dd  07  46  c7  e3  83  2d  7d  7f  8e  54  d6  52  16  cb
0e  27  fa  e1  2a  de  26  82  60  ee  90  43  0d  d0  ed  dc  9a  20  3b  cf
10  3d  95  f2  40  92  e4  14  f4  87  d9  c6  23  ff  0a  bc  ad  e5  b7  1d
21  4e  6c  c0  98  b1  a6  99  84  bb  f6  d4  28  3f  69  d7  3e  f5  22  b8
59  eb  76  c1  ba  37  0b  fe  56  b0  47  35  6d  8f  15  73  1e  88  24  4b
8d  f1  aa  12  63  d1  a5  2e  f0  df  70  55  b6  be  2f  11  25  ab  c4  info
f9  8c  f8  9e  75  29  ca  6f  c5  9f  49  80  1b  8b  38  34  79  05  64  pack
```

I've been mainly using commit `fa2d1cb7bf57f2d6b705532fa6582adb07a808d1` as my example so far. There's a `fa` directory, which happens to contain two files, and when you append `fa` to the front of the file name of one of them, we get the hash of the example commit. So it seems pretty likely that `/.git/objects` is mainly a hashmap-type data structure that makes it quick to navigate to a commit based on only the first few characters of a commit-hash.

```bash
matt@matt:~/projects/notes/.git/objects/fa$ ls
2d1cb7bf57f2d6b705532fa6582adb07a808d1  a6e4c773b75629baa97a96f18494eff7475542
```

If we try to view the file with `cat`, we'll see that the contents are compressed (as they'll look like garbled nonsense). Git uses the zlib compression library.

But as we've seen, we get a sensible output by using `git cat-file -p fa2d`, which decompresses the file. 

**Nice option**:
`git log --oneline`

This presents commits just with their commit message.

# How Git Works

Unlike other version control systems (which just track the deltas, or changes to a file's content, in each commit and then stack or unwind those deltas to roll the code-state back or forwards), each commit in git stores full copies of files with any edits as well as a full snapshot of the entire (committed) repository in the form of a map to the locations of files.














end