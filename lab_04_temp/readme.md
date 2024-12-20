# Git and Visual Studio Code

> Official tutorial for VSC nad Git: https://code.visualstudio.com/docs/sourcecontrol/github

> Download page: https://code.visualstudio.com/download

## 0. Configuration check

* [] Git installed?
* [] Visual Studio Code installed?

## 1. Scenarios

### 1.1 Cloning an existing repository into local computer.

We clone a remote repository by running command:

```bash
git clone https://url.to.repo [optional folder path]
```




### 1.2 Pushing local changes into remote repository.

```bash
# when run for the first time, we need to set
# the upstream
git push -u origin branch_name

# then it will be enough to run
git push
# which will push to branch which was alredy set up as upstream
```

### 1.3 Pulling remote changes into local repository.

We can run two command to get the changes from the remote repository:

* `git fetch` - this command will download the changes, but won't change anything in your local repository. You can use it to check what changes have been made on the remote repository.
* `git pull` - this command will download remote changes and directly after that run the `git merge` command so you migth be forced to resolve conflicts before checking what has actually changed in the remote repository.


Command:
```bash
# it will download remote changes
git fetch

# it will try to find remote branch with the same name as current local branch and download and merge the changes.
git pull
```

### 1.4 Managing branches with Visual Studio Code.

Creating a new branch with the name `dev`:
```bash
git branch dev
```

Change the active branch to `dev`:
```bash
git checkout dev
```

Doing the samy by runnig single command:
```bash
# create a branch if it does not exist and checkout to that branch
git checkout -b dev
```