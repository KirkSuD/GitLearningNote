# Git Learning Note

Last update: 2019/09/03

A Git learning note, based on a book, [Pro Git](https://git-scm.com/book/).

## 1.1 Getting Started - About Version Control

Distributed VCS copys all data on each local machine.

## 1.2 Getting Started - A Short History of Git

Created because of Linux.

## 1.3 Getting Started - What is Git?

Snapshots, Not Differences:  
Git links not-changed files with the old ones.

Nearly Every Operation Is Local:  
You can use it offline, upload later.

The Three States: modified, staged, and committed:  
Changed, marked as next commit snapshot, safely stored in local database.

## 1.4 Getting Started - The Command Line

CLI can run **all** commands; **all** users have CLI.

## 1.5 Getting Started - Installing Git

TODO: try GitHub Desktop: <https://desktop.github.com/>.

## 1.6 Getting Started - First-Time Git Setup

    3 levels of settings: --system, --global, --local
    $ git config --list --show-origin
    $ git config --show-origin user.name

    $ git config --global user.name "KirkSuD"
    $ git config --global user.email kirksud@gmail.com

## 1.7 Getting Started - Getting Help

    Getting Help:
    $ git help <verb>
    $ git <verb> --help
    $ man git-<verb>

## 2.1 Git Basics - Getting a Git Repository

You typically obtain a Git repository in one of two ways:

1. You can take a local directory that is currently not under version control, and turn it into a Git repository, or
2. You can **clone** an existing Git repository from elsewhere.

In either case, you end up with a Git repository on your local machine, ready for work.

### Initializing a Repository in an Existing Directory

Command `git init`:

    $ cd /c/user/my_project
    $ git init

`git add` & `git commit` example:

    $ git add *.c
    $ git add LICENSE
    $ git commit -m 'initial project version'

### Cloning an Existing Repository

Command `git clone <url> <dir>` copys all data from server.  
Git has different transfer protocols: `https://`; `git://` or `user@server:path/to/repo.git` use ssh.

## 2.2 Git Basics - Recording Changes to the Repository

2 states: tracked or untracked.

Tracked files are files that were in the last snapshot;  
they can be unmodified, modified, or staged. They are files that Git knows about.

Untracked files are everything else:
any files in the dir that were not in snapshot and staging area.

When first cloning a repo, all are tracked and unmodified.

![Figure 8. The lifecycle of the status of your files.](https://git-scm.com/book/en/v2/images/lifecycle.png)

### Checking the Status of Your Files

Command `git status`:

    $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    nothing to commit, working directory clean

    $ echo 'My Project' > README
    $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    Untracked files:
    (use "git add <file>..." to include in what will be committed)

        README

    nothing added to commit but untracked files present (use "git add" to track)

### Tracking New Files

Command `git add <files/dir>`:

    $ git add README
    $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    Changes to be committed: ## <- tracked and staged
    (use "git reset HEAD <file>..." to unstage)

        new file:   README

## Staging Modified Files

    $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

        new file:   README

    Changes not staged for commit: ## <- tracked but not staged
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   CONTRIBUTING.md

Run `git add` to stage tracked, modified, not staged files.  
Also, Git stages a file exactly as it is when you run the git add command.  
(You have to stage it again if you change it again.)

### Short Status

Command `git status -s` or `git status --short`:

    $ git status -s
    M README
    MM Rakefile
    A  lib/git.rb
    M  lib/simplegit.rb
    ?? LICENSE.txt

?? for "not tracked"  
A for "Added to staging area"  
M for "Modified"

2 columns: left for staging area, right for working tree.

### Ignoring Files

Use `.gitignore` file.

The rules for the patterns you can put in the .gitignore file are as follows:

* Blank lines or lines starting with `#` are ignored.
* Standard glob patterns work, and will be applied recursively throughout the entire working tree.
* You can start patterns with a forward slash (`/`) to avoid recursivity.
* You can end patterns with a forward slash (`/`) to specify a directory.
* You can negate a pattern by starting it with an exclamation point (`!`).

Example:

    # ignore all .a files
    *.a
    # but do track lib.a, even though you're ignoring .a files above
    !lib.a
    # only ignore the TODO file in the current directory, not subdir/TODO
    /TODO
    # ignore all files in any directory named build
    build/
    # ignore doc/notes.txt, but not doc/server/arch.txt
    doc/*.txt
    # ignore all .pdf files in the doc/ directory and any of its subdirectories
    doc/**/*.pdf

More good .gitignore file examples at <https://github.com/github/gitignore>.  
Multiple .gitignore files in subdirs is ok.

### Viewing Your Staged and Unstaged Changes

Command `git diff` to see unstaged differences. (staged <-> working tree)  
Command `git diff --staged` / `git diff --cached` to see staged differences. (staged <-> last commit)

### Committing Your Changes

Command `git commit`, or `git commit -v` for verbose, launches the editor set by `git config --global core.editor` before to type commit message.  
Alternatively, `git commit -m "<commit message>"` to type commit message inline.

Example output:

    $ git commit -m "Story 182: Fix benchmarks for speed"
    [master 463dc4f] Story 182: Fix benchmarks for speed
    2 files changed, 2 insertions(+)
    create mode 100644 README

### Skipping the Staging Area

Command `git commit -a` skips the staging area, makes Git automatically stage every file that is already tracked before doing the commit, lets you skip the `git add` part.

This is convenient, but be careful; sometimes this flag will cause you to include unwanted changes.

### Removing Files

Command `git rm` removes files from staging area, and also removes the file from working directory.  
If modified or added, must force the removal with the -f option.

Command `git rm --cached <files / dirs / file-glob patterns>` to keep the files in working tree but remove them from staging area.

### Moving Files

Command `git mv <file_from> <file_to>` to rename file.

Also,

    $ git mv README.md README

is equivalent to

    $ mv README.md README
    $ git rm README.md
    $ git add README

It's just a shortcut.

## 2.3 Git Basics - Viewing the Commit History

Command `git log` to view commit history.  
`-p` / `--patch` to show the difference.  
`-<n>` to show only the last n commits.  
`--stat` to see some abbreviated stats for each commit.  
`--pretty=[oneline|short|full|fuller|format:...]` to pretty print.  
See [Table 1. Useful options for git log --pretty=format](https://git-scm.com/book/en/v2/ch00/pretty_format).  
`--graph` to see ascii graph showing branch and merge history.  
See [Table 2. Common options to git log](https://git-scm.com/book/en/v2/ch00/log_options).

### Limiting Log Output

See [Table 3. Options to limit the output of git log](https://git-scm.com/book/en/v2/ch00/limit_options).

Option           | Description  
-----------------|-----------------------------------------------------------------------------  
-[n]             | Show only the last n commits  
--since, --after | Limit the commits to those made after the specified date.  
--until, --before| Limit the commits to those made before the specified date.  
--author         | Only show commits in which the author entry matches the specified string.  
--committer      | Only show commits in which the committer entry matches the specified string.  
--grep           | Only show commits with a commit message containing the string  
-S               | Only show commits adding or removing code matching the string  
--no-merges      | Not show merge commits

## 2.4 Git Basics - Undoing Things

Command `git commit --amend` to replace last commit.  
Command `git reset HEAD <file>...` to unstage a staged file.
 (dangerous especially if you provide the --hard flag.)  
Command `git checkout -- <file>...` to discard changes of modified file.
 (dangerous, this replaces local file with last-committed version.)  

## 2.5 Git Basics - Working with Remotes

Command `git remote [-v]` to list the shortnames of each remote handle you’ve specified.
 (`-v` to show URLs)  
Command `git remote add <shortname> <url>` to add a new remote Git repository as a shortname.  
Command `git fetch <shortname>` to fetch all info of it.
 (only download, not merge)  
Command `git pull` to fetch and merge the branch into current branch.  
Command `git clone` to set up local master branch to track remote master branch.  
Command `git push <remote> <branch>` to push to remote.  
Command `git remote show <remote>` to inspect a remote.  
Command `git remote rename <old-name> <new-name>` to rename a remote.  
Command `git remote [remove / rm] <name>` to remove a remote.

## 2.6 Git Basics - Tagging

Command `git tag [-l / --list] ...` to list tags.  
Command `git tag -a <tag-name> -m "<tag-description>"` to create an annotated tag.  
Command `git tag <tag-name>"` to create a lightweight tag.  
Command `git show <tag-name>` to show commit, and info if it's annotated.  
Specify a commit checksum to tag old commits. For example, `git tag -a v1.2 9fceb02`.

By default, the `git push` command doesn’t transfer tags to remote servers,  
run `git push origin <tagname>` to explicitly push tags to a shared server.  
Command `git push origin --tags` to transfer all tags.  
Note: `git push` pushes both types of tags.

Command `git tag -d <tagname>` to delete a tag on **local** repository.  
2 common variations for deleting a tag from a remote server:

1. `git push <remote> :refs/tags/<tagname>`
2. `git push origin --delete <tagname>` (more intuitive)

Command `git checkout <tagname>` to view the versions of files a tag is pointing to.
 (**Warning**: this puts your repository in “detached HEAD” state, which has ill side effects.)  
Command `git checkout -b <branch-name> <tag-name>` to create a new branch from a tag.

## 2.7 Git Basics - Git Aliases

An example is worth hundreds of sentences:

    $ git config --global alias.unstage 'reset HEAD --'

makes the following two commands equivalent:

    $ git unstage fileA
    $ git reset HEAD -- fileA

Common usage - `last`:

    $ git config --global alias.last 'log -1 HEAD'

External command by using `!`:

    $ git config --global alias.visual '!gitk'

## 3.1 Git Branching - Branches in a Nutshell
