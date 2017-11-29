class: middle, center

# git

---

## What is git?

Git (/ɡɪt/) is a **version control system**

![git branch](/images/git-branch.svg)

---

## VCS

> Version control is a system that records changes to a file or set of files<br/> over time so that you can recall specific versions later. 

— Pro git book

---

## Why VCS is *A Good Thing™*

Does this seem familiar to you?

```
my-awesome-code.scd
my-awesome-code1.scd
my-awesome-code2.scd
my-awesome-code2-FINAL.scd
my-awesome-code2-FINAL2.scd
my-awesome-code2-FINAL2-FINAL-FOR-REALZ.scd
my-awesome-code2-FINAL2-FINAL-FOR-REALZ_1.scd
```

--
**git** will help you.

---
class: middle, center

# Basics

---

## git init

Create a new directory somewhere on your system

```
$ mkdir ~/Desktop/git-test && cd ~/Desktop/git-test
```

--
Create something for git to track

```
$ echo "This is a README" > README
```

--
Setup git in this directory

```
$ git init

-> Initialized empty Git repository in /Users/user/Desktop/git-test/.git/
```


Git is now aware of this directory and can start to track changes to files.

---

## git status

Now that we have set up `git` what can we do with it?

One useful command we could try is the `status` command.

```
$ git status

-> On branch master

   No commits yet

   Untracked files:
     (use "git add <file>..." to include in what will be committed)

           README

   nothing added to commit but untracked files present (use "git add" to track)
```

Nice. Git just gave us some very useful information!

It told us that we have no `commits` yet, and also that we have an `Untracked file`. Let's go ahead and `add` this file so git can start to track it.

---

## git add

Add untracked files to git using the `add` command

```
$ git add README
```

Let's ask `git` about its status again

--

```
$ git status

-> On branch master

   No commits yet

   Changes to be committed:
     (use "git rm --cached <file>..." to unstage)

           new file:   README
```

We can see that the status has changed. The file is now to git in a state called "staged".

In order for git to begin to track it we need to `commit` it.

---

## git commit

The `commit` command tells git that we want to record the current state of its staged files.

```
$ git commit -m Initial commit

-> [master (root-commit) 9319ba3] Initial commit
    1 file changed, 1 insertion(+)
    create mode 100644 README
```

We passed the `-m` flag in order to supply a commit message from the command line. Its common to call the first commit "Initial commit". The initial commit is also known as the "root commit", from where our tree of recored changes will grow.

Let's take a look at the status again

```
$ git status

-> On branch master
   nothing to commit, working tree clean
```

---

## Life-cycle of file changes

.center[![git lifecycle](images/lifecycle.png)]

---
class: center

## Summary of what we know so far

.center-table[

| Command                     | Description                          |
|:--------------------------- | ------------------------------------:|
| `git init`                  | Initialize a git repository          |
| `git add`                   | Add files for git to track           |
| `git commit`                | Commit files and/or changes to files |

]

---

class: middle, center

# Committing changes

---

## git commit

Now that we have added the README file and commited it, git will know about its state and it will tell us about any changes done to it. So let's make some changes.

```
$ echo "This is a new line" >> README
$ git status

-> On branch master
   Changes not staged for commit:
     (use "git add <file>..." to update what will be committed)
     (use "git checkout -- <file>..." to discard changes in working directory)

           modified:   README

   no changes added to commit (use "git add" and/or "git commit -a")
```

---

## git commit

Git now tells us that the file has been modified.

We can even ask `git` about what has changed since the last commit.

```
$ git diff

-> diff --git a/README b/README
   index b07f0ed..0d4740e 100644
   --- a/README
   +++ b/README
   @@ -1 +1,2 @@
   This is a README
   +This is a new line
```

---

## git commit

We can now repeat our add/commit cycle to record the new changes to the file

```
$ git add README
$ git commit
```

You will notice that if we don't use the `-m` flag we will be taken to our $EDITOR to write the commit message. On most systems this editor is `vi`.

There is also shorter way to commit changes by giving the file as an argument to the commit command

```
$ git commit README
```

---

## git log

Let's take a look at our commit history so far!

```
$ git log

-> commit af257613e14044199077bfdbe6c512b8f30208b7 (HEAD -> master)
   Author: David Granström <david@davidgranstrom.com>
   Date:   Mon Nov 27 21:14:05 2017 +0100

       Add new line of text

   commit 9319ba3d14492e304e600634f89d6c470076ac0d
   Author: David Granström <david@davidgranstrom.com>
   Date:   Mon Nov 27 20:54:17 2017 +0100

       Initial commit
```

---

## git log

We can even display a nice ASCII graph of the log output

```
$ git log --graph --pretty=format:"%h %d %s (%cr) <%an>" --abbrev-commit --date=relative

-> * af25761  (HEAD -> master) Add new line of text (2 days ago) <David Granström>
   * 9319ba3  Initial commit (2 days ago) <David Granström>
```

We can colorize the output

```
git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative
```

Let's make an alias of that

```
git config --global alias.lg "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative"
```

We can now get a colorized graph of the log output by typing `git lg`

---
class: middle, center

# Branching

---

## git branch

One of the powers of using a system such as `git` is that we can "branch out" and experiment freely with our files without losing any history.

Create a new branch

```
$ git branch experiment
```

--
Move to the new branch

```
$ git checkout experiment

-> Switched to branch 'experiment'
```

*ProTip:* We can compress the two commands above into one by using the `-b` flag for the checkout command

```
$ git checkout -b experiment

-> Switched to a new branch 'experiment'
```

---

## git branch

Let's take a look at the status

```
$ git status

-> On branch experiment
   nothing to commit, working tree clean
```

We are now on the "experiment" branch, anything we commit from here will end up on this new branch.

---

## git branch

Add something new to the README

```
$ echo "This is an experimental feature.." >> README
```

--
..and commit it

```
$ git commit README -m "Add experimental line"

-> [experiment fcb468e] Add experimental line
   1 file changed, 1 insertion(+)
```

```
$ git lg

-> * fcb468e  (HEAD -> experiment) Add experimental line (3 minutes ago) <David Granström>
   * af25761  (master) Add new line of text (2 days ago) <David Granström>
   * 9319ba3  Initial commit (2 days ago) <David Granström>
```

`HEAD` references the currently checked out commit which has now moved ahead of master

---

## git branch

Let's move back to the master branch

```
$ git checkout master

-> Switched to branch 'master'
```

We see that the commit we did on the "experiment" branch is gone

```
git lg

-> * af25761 (HEAD -> master) Add new line of text (2 days ago) <David Granström>
   * 9319ba3 Initial commit (2 days ago) <David Granström>
```

If we check the file content we see that the line we added on the "experiment" branch has vanished

```
$ cat README

-> This is a README
   This is a new line
```

---

class: middle, center

# git merge

---

## git merge

Let's switch back to the "experiment" branch

```
$ git checkout experiment
```

And add another commit

```
$ echo "Another experimental commit" >> README
```

```
$ git commit README -m "Add another experimental commit"

-> [experiment 8df2311] Add another experimental commit
   1 file changed, 1 insertion(+)
```

---

## git merge

Say that we are happy with the state of the "experiment" branch and want to merge back the commits to master

```
$ git checkout master
```

We do that with the `merge` command

```
$ git merge --no-ff experiment

-> Merge made by the 'recursive' strategy.
   README | 2 ++
   1 file changed, 2 insertions(+)
```

The `--no-ff` flag is optional and means "no fast forward" and gives us clearer representation of the commit history.

---

## git merge

Take a look at the history

```
$ git lg

-> *   ec21d77 - (HEAD -> master) Merge branch 'experiment' (2 minutes ago) <David Granström>
   |\
   | * 8df2311 - (experiment) Add another experimental commit (4 minutes ago) <David Granström>
   | * fcb468e - Add experimental line (44 minutes ago) <David Granström>
   |/
   * af25761 - Add new line of text (2 days ago) <David Granström>
   * 9319ba3 - Initial commit (2 days ago) <David Granström>
```

We can clearly see when the "experiment" branch was merged back into master.

It is now safe to delete the "experiment" branch

```
$ git branch -d experiment

-> Deleted branch experiment (was 8df2311).
```

---

## git merge (conflict)

```
$ echo "A new commit on master" >> README
```

```
$ git commit README -m "Add new line on master"

-> [master 62e82ac] Add new line on master
   1 file changed, 1 insertion(+)
```

```
$ git lg --all

-> * 62e82ac - (HEAD -> master) Add new line on master (7 seconds ago) <David Granström>
   | * fcb468e - (experiment) Add experimental line (31 minutes ago) <David Granström>
   |/
   * af25761 - Add new line of text (2 days ago) <David Granström>
   * 9319ba3 - Initial commit (2 days ago) <David Granström>
```







