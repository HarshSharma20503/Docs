# Git-GitHub by Hitesh(Youtube)

**Reference Link** : [Youtube Link](https://www.youtube.com/watch?v=tz82ola3oy0&t=1667s)

## Git

### Check Version

`git --version` or `git -v`

### Initialising git repo

`git init` → Creates a hidden .git folder which keeps track of all the track-able files

### Check Status

`git status` → Tell the status of the working directory, staging area, files tracked, untracked, etc.

### Pulling from the Remote Repo using rebase

```bash
git pull --rebase
# if the merge conflict arise than
git rebase --abort
# than do it the normal way 
git pull
# and resolve merge conflict through traditional ways.
```

### Stage a file

`git add <file-name>`

### Unstage a file

`git rm --cached <file-name>` → do not use it without —cached as it would delete the file itself.

`git restore --staged <file-name>` → unstages the file, without the —stage it would completely discard the changes.

`git restore --dry-run <file>` → f you want to see what changes would be made without actually applying them

### Make a commit

`git commit -m “commit-message”`

### View all commits

`git log` → shows you commit logs (author, time, message, hash)

`git log --oneline` → shows you commit in one line (hash, message)

### Best Practices for Git Commit Messages

They should focus on one feature and be in the present tense.

for e.g. “Add user authentication feature”, “Update README with installation instructions”

### Changes in git configuration (git config)

`git config --global user.name "Your Name"` → This specifies your username for Git Globally. `git config --global user.email "your.email@example.com"` →This specifies your email address associated with Git commits globally.

`git config --global core.editor "code --wait"` → This sets your preferred text editor for Git i.e. vs-code and —wait tells git to wait till the vs-code closes before resuming.

### How to ignore a file

Create a file name .gitignore and inside it write the file name of the file you do not want git to track. The files inside the .gitignore won’t be track able.

### How to create a branch

`git branch "branch-name"` → creates a new branch but remains on the same branch as before

`git switch -c "branch-name"` → creates a new branch and switches to that newly created branch

`git checkout -b "branch-name"` → creates a new branch and switches to that newly created branch

### How to switch to a branch

`git checkout “branch-name”` → switches to the mentioned branch

`git switch "branch-name"` →switches to the mentioned branch

### Discarding changes in the working directory for a specific file

`git checkout -- "file-name"` → removes all the changes done in a file

### Merging the branches

switch to the main/master branch (the primary branch in which you want the secondary branch to merge into)

`git merge "branch-name"` → merges the branch to the current branch

### Delete a branch

`git branch -d "branch-name"`

### Understand merge conflicts

Everything above the `===========` and below `<<<<<<<<<<<` is from the current branch of local repo, and everything below is the other branch code. You can go and keep the file as you want and than make another commit.

### How to compare a file between different commits

we use `git diff` to compare a file between 2 point of time (commit, staging, etc)

How to read diff:

a→file1 & b→file2 (same file over time)

`-----` file1 (indicates code of file1)

`+++++` file2 (indicates code of file2)

changes in lines & little preview of it

minus and plus does not mean insertion and deletion

### Difference between files in staging area and last commit

`git diff --staged` : shows the differences between the files in the staging area (index) and the last commit.

### Difference between snapshots of two commits

`git diff <commit-hash 1> <commit-hash 2>` : shows the differences between the snapshots of two specified commits.

or

`git diff <commit-hash 1>..<commit-hash 2>` : shows the differences between the snapshots of two specified commits.

### Difference between snapshots of two branch

`git diff <branch-name 1> <branch-name 2>` or `git diff <branch-name 1>..<branch-name 2>` : shows the difference between the snapshots of two branches.

## Understanding git stash

When you are working on a branch and you work is not completed so you cannot commit yet but you need to switch branch to do some changes in other branch. Git does not allow you to change branch without committing the changes. This is resolved by the `git stash`.

`git stash` : temporarily saves changes in the working directory that are not yet committed, allowing you to work on a clean slate without committing those changes.

`git stash pop` : applies the most recently stashed changes back to the working directory and removes that stash from the stash list.

**Note:** I can even pop the stash in other branches too.

`git stash list` : displays a list of all stashed change-sets.

`git stash apply` : applies the most recently stashed changes back to the working directory without removing that stash from the stash list

### Advance Commands

`git checkout <commit-hash>` : switches the working directory to the state of the specified commit, putting the repository in a "detached HEAD" state where you can view and work with the files from that commit without being on any branch

`git switch main` : “re-attach Head”

`git checkout HEAD~2` : checks out the commit that is two commits before the current HEAD, putting the repository in a "detached HEAD" state.

`git restore <file-name>` : reverts the file to its last committed state, discarding any local changes.

`git restore .` : reverts all files in the working directory to their last committed state, discarding any local changes

`git restore --staged --worktree .` reverts all files in both the staging area and the working directory to their last committed state, discarding all local changes.

## Understanding git rebase

**Note:** Be cautious when running this command as it rewrites the git history.

Run this command from side branch, never run this command on main or master branch.

run `git rebase master` on other branch like bugfix. It takes bugfix commits and put it ahead of master branch.

## GitHub

### Generating ssh key and adding it to the ssh-agent

[https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

Go to settings→SSH GPG keys → new SSH key → authentication key

### Create new repo and upload the project

`git branch -M main` : renames the current branch to "main”

`git remote add origin <repo url>`

`git push -u origin main`

### Cloning repo

`git clone <repo-url>` : creates a local copy of a Git repository from the provided `<repo-url` (usually from a Git hosting platform).

### Remote commands

`git remote -v` : It displays information about all configured remotes, including their URLs and the configured fetch refspec.

`git remote add <name> <url>` : creates a shortcut named <name> for a remote Git repository at <URL>.

`git remote rename <old-name> <new-name>` : updates the local reference for a remote repository, changing its name from <old-name> to <new-name>pen_spark

`git remote remove <name>` : removes the configured remote repository named `<name>` from your local Git repository.

### Push Commands

`git push <remote> <branch>` : uploads local commits from your branch named `<branch>` to the corresponding branch named `<branch>` on the remote repository named `<remote>`.

`git push <remote> <local-branch>:<remote-branch>` : pushes commits from your local branch `<local-branch>` to a specific branch named `<remote-branch>` on the remote repository named `<remote>`.

`git push -u origin main` : uploads your local "main" branch to "origin" and makes it the default for future pushes. After this you can directly use `git push` command.

`git pull` = `git fetch` + `git merge`
