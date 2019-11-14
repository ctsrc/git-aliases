# repotools

I've got a handful of aliases that have been very very essential
to me for many years now. It's all about identifying a select few
commands that you use a lot, and keeping in mind what the effect
would be if you were to apply any of them mindlessly.

You then define short and meaningful aliases that are fast to type
for the commands that you use the most but which don't require much
care.

Then the command line stays a joy to use.

## Installation

Clone repository

```bash
mkdir -p ~/src/github.com/ctsrc/
cd ~/src/github.com/ctsrc/
git clone git@github.com:ctsrc/repotools.git || \
git clone https://github.com/ctsrc/repotools.git
```

Create symlinks in your `~/bin/`.

```bash
mkdir -p ~/bin/
find ~/src/github.com/ctsrc/repotools/bin/ -type f \
  -exec ln -s {} ~/bin/ \;
```

Edit your `~/.profile` (bash and many others), `~/.zshenv` (zsh)
or whatever is the equivalent for your shell, adding `~/bin`
to your `$PATH` if it's not already there.

If your `~/.profile` or equivalent has a check for the existence
of `~/bin/` that would add it to `$PATH` if `~/bin/` exists, but
you had not created that directory prior to this then you should
just log out and log in again. Same goes if you edited the file
because `~/bin/` was not mentioned from before; log out and log
in again.

## Background

I used to keep aliases in my `.bashrc`, but now that I run different operating
systems on my server (FreeBSD), desktop (KDE Neon Linux) and laptop (macOS),
rather than running FreeBSD everywhere like I used to for some years, or
rather than even running just FreeBSD and one Linux distro like I used to
after that, I have decided that with the variations that I need to maintain
between the `.bashrc` files on each of my systems, it is better to keep my
`.bashrc` files as small as possible, and to create wrapping scripts
instead of keeping aliases in `.bashrc`.

Then I just clone this repo to `~/src/github.com/ctsrc/repotools`
and quickly symlink them all into my `~/bin/` as mentioned above
and then I am able to get productive on any host in no time,
without having to maintain messy build steps that deal with what to include
and what not to include in `.bashrc` for a given operating system or distro.
Now my `.bashrc` files contain pretty much only that which is
operating system/distro specific.

The most important "aliases" in my workflow are those that I use
practically all the time, namely those relating to git.

## A word on arguments and flags

With all of the "aliases" for which it makes sense to always or
sometimes pass arguments or additional flags to, the "aliases"
pass these on to the command that they invoke, like you will see
with `aa` and others below.

## Usage

### git

#### `dp [<pathspec>]` – diff pending, optionally limited to a given file or directory

When I look at what changes I want to stage for commit, I type `dp`.

`dp` calls `git diff` with the optionally provided argument.

#### `aa [<directory>...]` – add all, optionally limited to one or more directories

When I want to stage everything, or everything in certain directories,
for commit, I type `aa` or `aa <directory>...` respectively.

`aa` calls `git add -A` with the optionally provided arguments.

#### `st [<pathspec>]` – status, optionally limited to a given file or directory

My workflow usually goes, write some code, stage some code, research something,
test the changes, write some code, stage some code, and so on.

Every now and then between writing, staging, etc, I look at what
I've staged for commit so far, and what has not yet been staged.
For the high-level bird's eye view of that I have the alias `st`.

`st` calls `git status` with the optionally provided argument.

#### `di [<pathspec>]` – diff staged, optionally limited to a given file or directory

Prior to committing, and also now and then while still working on the code,
it is very useful to review what I am about commit, both to ensure that
I am about to commit what I think I am about to commit, and in order to
write useful commit messages that accurately describe the changes.

The alias `di` shows the diff for the staged changes against HEAD.

`di` calls `git diff --cached` with the optionally provided argument.

#### `dxne [<pattern>...]`

List untracked or ignored files and directories in current directory,
except matching `[<pattern>...]`.

#### `cm <message>` – commit with message

When it comes time to commit, I use the alias `cm`.

`cm` calls `git commit -m` with the provided argument.

#### `pu [--tags | -f]` – push, optionally including refs under refs/tags, or force push

Finally I push with `pu`.

`pu` calls `git push` with the optionally provided flag.

#### `le` – number of commits

Among all of the aliases, this is the only one that doesn't match
the meaning with its name. I would have named this alias as I initially
did; `nc`, if it wasn't for the fact that the netcat command on a lot
of systems is named `nc` already. And when I used aliases, I didn't feel
like having to remember to type `\nc` when I wanted netcat, nor did I
and nor do I feel like typing out the full path to netcat either, e.g.
`/usr/bin/nc`. So I took the "l" for "log" and the "e" for the `-e` flag.

`le` calls `git shortlog -se`.

#### But wait, that doesn't quite cover all the git commands I really use

The seven aliases above cover the git commands I use the most often.

There are a lot of other git commands that I use less frequently,
for which I have not defined any aliases. The reason for this is
that with the less frequently used commands the number of keystrokes
per session saved become quite few, and with many of them it's
actually easier to remember command name and options,
or check the relevant `git help <COMMAND>` for the options you need.

A non-exhaustive list of such commands includes:

* `git init .`
* `git tag -m <msg> <tagname>`
* `git tag -d <tagname>`
* `git add -p [<pathspec>...]`
* `git clone <repository> [<directory>]`
* `git log [--format=fuller]`
* `git commit --amend --no-edit --date=now` when there was something I was planning to include in the most recent commit, but forgot to, and then subsequently realized that, so I stage it with `aa` and then amend the commit while also updating the commit date so that it is correct. Preferably before pushing the commit, otherwise force push is needed.
* `git commit --amend --no-edit --reset-author` when I commited with an e-mail that I use for one project, while working on another project for which I use a different e-mail address.
* `git stash [-p]`
* `git stash list`
* `git stash show [<stash>]`
* `git stash pop`
* `git stash drop`
* `git remote -v`
* `git remote remove <name>`
* `git remote add <name> <url>`
* `git branch <branchname>`
* `git merge <branchname>`
* `git branch -D <branchname>`

And of course there is the two that `git status` (`st`) kindly remind you of:

* `git reset HEAD <paths>...` to unstage
* `git checkout -- <paths>...` to discard changes in working directory

These two I actually use quite a lot.

I feel that with their destructive nature, in particular the second one
of them, it's better to type those commands out in full while you are thinking
about which files you are about to apply these two commands to.

Same goes about typing and thinking with some of the other commands that
are destructive, like the already mentioned `git branch -D <branchname>`,
and with the following two that are also very destructive:

* `git reset --hard HEAD^` to remove the last commit from git.
* `git reset --hard HEAD~<n>` to remove the *n* last commits from git.

Related to this are

* `git reset --soft HEAD^`
* `git reset --soft HEAD~<n>`

Beyond that there's also the stuff that I do sufficiently rarely
that I'll rather just look it up on Google once in a blue moon
rather than bother to remember it. Like:

* `git checkout --orphan <new_branch>`
* Rebasing