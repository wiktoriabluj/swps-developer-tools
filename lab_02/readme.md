# LAB 02. Learning Git.

Git is a distributed version control system that allows you to track file changes, create project histories and collaborate on code. Its operation is based on a repository (repo), which stores all versions of files and tracks every change reliably and efficiently. To better understand how Git works, let's review the key elements and concepts.

*Documentation?* [git-scm.com](https://git-scm.com/book/pl/v2).

## Basic information about the git system.

### 1. **Repository (Git repository)**.

A Git repository is a folder that contains a project and all the data needed to track its history. Git stores the information in the **directory `.git`** (a hidden folder in the root directory of the project). The repository contains:

- **Index (staging area)**: A temporary place where changes to be approved (commits) are stored.
- **Branches (branches)**: Separate lines of project development that can be merged.
- **Commits**: Approved change sets that make up the version history.

### 2. **Snapshots, not differences (Diffs)**.

One of the key aspects of Git is that, unlike other version control systems, Git **does not track differences between files**. Instead, Git **creates so-called snapshots** of the entire project directory with each approval (commit). This means that every time you approve changes, Git saves a complete snapshot of the files at that moment. If the file has not been changed, Git does not save a new copy of the file - instead, it creates a pointer to the previous version of the file.

### 3. **Index (Staging Area)**.

The index, also called the staging area, is a temporary place where files to be approved are stored. Before performing a commit, you must add changes to the index so that Git knows which files to include.

### 4. **Commit**.

Commit is a basic element in Git, which means permanently saving changes to the repository. Each commit contains:

- A set of files and their versions (snapshot).
- Metadata such as commit author and commit time.
- Change description message (commit message).
- The **Hash** (unique identifier) that identifies the commit.

During a commit, Git not only creates a new version of the files, but also keeps track of the relationships between commits, making it possible to reconstruct the history of the project.

### 5. **SHA-1 hash**.

Each commit is identified by a **unique SHA-1** hash, which is a 40-character string of numbers and letters. This hash is generated based on the content of the commit (files, their versions, metadata). This mechanism makes sure that every minor deviation in the commits creates a completely different hash, which makes Git very accurate and secure in version tracking.

### 6. **Branches (Branches)**.

A branch in Git is a movable pointer to a specific commit. By default, when you create a repository, a master branch named **main** (or historically `master`) is created. You can create new branches that allow you to work on different functions at the same time, without affecting the code in the main branch, because when you create a new branch, it is created, so to speak, as a copy of the branch on which the new branch is just created.

Creating and working with branches allows you to:

- Creating distinct development paths.
- Collaborate on projects without conflicts.
- Merging branches into one main stream when features are ready.

### 7. **Merging (Merging)**.

When you work on different branches, you can merge changes from one branch to another. Merging is the process by which changes from one branch (e.g. new functionality) are integrated into another branch (e.g. `main`).

- **Conflict-free merging**: Git automatically merges changes if there are no conflicts.
- **Conflicts**: If the same lines of code have been changed in both branches, Git will ask you to manually resolve conflicts.

### 8. **Local vs. remote repository**.

Git is a distributed system, which means that each copy of the repository is a full repository that contains the entire history of the project. You can work on a local repository on your computer, but you can also collaborate with others using remote repositories (such as on GitHub).

### 9. **Commit History**.

Git allows you to view the full history of commits, which makes it easier to track changes in a project, identify errors and restore previous versions of code. The `git log` command displays the commit history, and with various options (such as `--online`), you can view it in a compact form.

### How do repository versions work in Git?

Each change made to a project, when added to the index and approved as a commit, becomes a new version of the repository. Git saves these versions as snapshots, with the ability to identify them by unique hashes.

## 0. Installation.

Installing Git depends on the operating system you are working on. Below you will find instructions for installation on various platforms.

### 0.1 **Installing Git on Linux**.

On most Linux distributions, Git is available in the default repositories. To install Git, simply use the package manager appropriate for your distribution.

**Ubuntu/Debian:**
  ```bash
  sudo apt update
  sudo apt install git
  ```

- **Fedora:**
  ```bash
  sudo dnf install git
  ```

- **Arch Linux:**
  ```bash
  sudo pacman -S git
  ```

- **Slackware:**.
  On Slackware, Git can be installed using `pkgtool`. You can download the Git package from [SlackBuilds.org](https://slackbuilds.org/) and install it manually. The installation instructions look like this:
  1. download the installation script from the SlackBuilds.org website.
  2. unpack the archive:
     ```bash
     tar xvfz git.tar.gz
     cd git
     ```
  3. launch SlackBuild:
     ```bash
     ./git.SlackBuild
     ```
  4. Install the package:
     ```bash
     sudo installpkg /tmp/git-<version>.tgz
     ```

### 0.2 **Installing Git on macOS**.

There are several ways to install Git on macOS. The easiest is to use the Homebrew package manager.

1. install Homebrew (if you don't already have it installed):
   ```bash
   /bin/bash -c “$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)”
   ```

2. Install Git:
   ```bash
   brew install git
   ```

Git is also available as part of Xcode Command Line Tools. You can install it by running the following command, which will guide you through the installation process:
```bash
xcode-select --install
```

### 0.3 **Installing Git on Windows**.

On Windows, it's best to use the Git installer for Windows, which also provides a Git Bash utility that allows you to work with Git in a Linux terminal-like environment.

1. Download the installer from the official Git website: [https://git-scm.com](https://git-scm.com).
2. run the downloaded `.exe` file and go through the installation process, accepting the default options.

During the installation, you will have the option to configure several options, such as using Git Bash or integrating with PowerShell. Select the options according to your preference.

### 0.4 **Installation check**.

Once the installation is complete (on any system), you can check that Git has been installed correctly by typing the following command in a terminal (or Git Bash on Windows):

```bash
git --version
```

If Git was installed correctly, you will see the version number, e.g. `git version 2.42.0`.

---

Now that Git is installed on your system, you can get started by configuring your user credentials with the `git config` command, as described in the configuration section.

## 1. **Create a new repository**.
To start tracking the project, create a new repository in an existing folder.

```bash
git init
```
In the folder where the command was run, a hidden folder named `.git` will appear, which contains information about our repository. A branch `master` will also be created.

## 2. **Git user configuration**.
When you start working with Git, you need to configure your credentials, such as your name and email address, which will be used to sign commits.

```bash
git config --global user.name some_user_name
git config --global user.email some_email@example.com
```

When working with multiple people on the same OS account, configure these parameters as local instead of global as below

```bash
git config --local user.name akowalski
git config --local user.email akowalski@gmail.com
```

The *global* or *system* option sets these parameters for every repository in the account, while *local* only sets them for that specific one. Note then that we can still override these values locally for a specific repository.

Another thing, IF WE WORK ON COMPUTERS IN THE CLASSROOM, that needs to be done is to disable Visual Studio Code's default integration with the windows credential manager, which will store other users' GitHub login credentials making it difficult for subsequent users who want to push changes to their remote repository.
We can do this with the command:
```bash
git config --system --unset credential.helper
```
It's also worth checking for this setting in other configuration scopes.
```bash
git config --global -l
git config --local -l
```
If so, here we also remove this parameter.

Another possibility is to delete stored credentials in Windows credential manager after login in the student account and before logout as our credentials when logging into GitHub might also be stored.

## 3. **Clone an existing repository**.
To retrieve a copy of a remote repository that already exists, use the `git clone` command.

```bash
git clone <url_repository> [directory]
```

The [directory] parameter is optional and when not present a directory with the exact name of the cloned repository will appear in the folder when the command was executed.

## 4. **Add files to the repository**.
If you have added new files to the project or changed existing ones, you need to add them to what is called an index so that they are tracked by Git.

```bash
git add file.py
```
The above command adds the `file.py` file to the waiting room (stage area) and in Visual Studio Code it will be marked with `A` (added means added).

To add all modified files we use the command:

```bash
git add . or git add *
```

## 5. **Automatically exclude files from being added to the repository.**

However, it is important to remember that different tools add 'something from themselves' and there may firstly be configuration files that are unlikely to be needed when we want to share the repository with other users who may want to use a different tool than ours and will have to 'clean up' the repository a bit on their own. Therefore, the recommended step before adding anything to the repository is to prepare a `.gitignore` file containing rules (regular expressions) that will cause fulfilling resources to be ignored by the Git tool when tracking changes. However, note that when modifying `.gitignore` we should remove resource tracking, most conveniently simply:

```bash
# NOTE: the following command will remove the file from the waiting room, but also from the disk
git rm *
# command with the --casched switch will remove resources only from the waiting room
git rm --cached *
```

We create a `.gitignore` file and in its contents we write the names of files or folders that we do not want to be added to our repository. It is important that each file/folder name be on a new line. Remember to add the file extension at the end of the file name. If we want to exclude from adding all files of a given type we can use simple regular expressions, i.e. for example, if we want to exclude from adding all files of type `.txt` we should type `*.txt` into the file....

... and again:
```bash
git add .
```
so that Git adds everything ignoring those contained in `.gitignore`.

## 6. **Approve changes (commit)**.
After adding files to the index, changes can be approved.

```bash
git commit -m “Description of changes made”.
```
The `-m` option allows you to add a message to the commit that describes the changes made.

This command creates a new snapshot of the resources located in the working folder (that is, everything that is currently visible in the project explorer) that is from the waiting room. Each commit has a unique hashcode.

## 7. **Tracking repository status**.
To see which files have been modified and which are waiting for approval:

```bash
git status
```

## 8. **Viewing the history of changes**.
To see the history of commits, you can use the command:

```bash
git log
```

To view the log in a more compact form:

```bash
git log --oneline
```

## 9. **Compare changes**.
To see the differences between local files and approved changes:

```bash
git diff
```

## 10. **Creating branches (branching)**.
Branching allows you to develop new functionality without interfering with the main version of the project.

```bash
git branch <branch_name>.
```

To switch to a new branch:

```bash
git checkout <branch_name>.
```

You can also create and switch to a branch in one step:

```bash
git checkout -b <branch_name>.
```

## 11. **Scaling a branch (merge)**.
When you are done working on a branch, you can merge its changes with the main branch (usually `master` or `main`).

```bash
git checkout main
git merge <branch_name>.
```

## 12 **Downloading changes from a remote repository**.
To retrieve changes from a remote repository without automatic merge:

```bash
git fetch
```

## 13. **Fetch and merge changes**.
To fetch changes and automatically merge them to the current branch:

```bash
git pull
```

## 14 **Send changes to a remote repository**.

To send local changes to a remote repository:

```bash
git push origin <branchname>.
```

## 15 **Resetting and undoing changes**.

Sometimes it is necessary to undo or discard changes. Git offers several mechanisms:

- **git reset**: Restores the repository to an earlier state. You can undo changes in the staging area or fully discard a commit.
  
  ```bash
  git reset --hard <commit_hash>.
  ```

- **git revert**: Creates a new commit that reverses the changes made by the previous commit, without deleting its history.
  
  ```bash
  git revert <commit_hash>.
  ```

### 16 **Tags (Tags)**.

Tags are special indicators used to mark important commits, such as software versions. Tags are immutable and help you easily find specific points in the project history.

```bash
git tag <tag_name>.
```

# __Hooking up a local repository to a new remote repository__.

__Step 1__.

We create a new remote repository, using the GitHub service during the class. After creating it, a page with commands that should help us with the synchronization process will be displayed. The commands including the first commit can be skipped, as this has already been done in a slightly different way.
For the local repository, we indicate the remote repository to which we will want to push the code.

```bash
git remote add origin https://github.com/user/repo.git
```
The above command is appropriate if we do not use an SSH key (a better solution if we are the only ones using a given account on the operating system), as it does not require us to provide credentials each time we push out changes. We add information about the new remote repository with the origin alias and the given url.

If we want to check what the address of the remote repository is then
```bash 
git remote get-url origin
```
and if we need to change it
```bash
git remote set-url origin https://new.repository.url
```

__Step 2__

Command
```bash
git push -u origin main
```
tries to send the current branch (and its state from the nostatically executed commit) to the remote repository from the address assigned to the alias (origin) to the remote master branch.
Here, several problems (errors) may arise that can prevent this operation.
If you get a message saying that stuffing is not possible because the user named xxx does not have credentials, it means that there are credentials stored in the `credentials manager` for another user's GitHub service (they may come from another tool). You should remove all credentials that refer to the GitHub service and retry pushing out the changes. You can retry the change-pushing operation.

There may also be a problem here if there are some resources in the remote repository that are not there locally (such as the added default `readme.md` file). Then we can execute the push command with an additional parameter, forcing the push (force):
```bash
git push -f origin main
```

### __Download the repository to another location/computer__.

If you want to continue working with the contents of the repository on another computer, tool then just go to the folder where you want to place the repository, for example, a folder with other projects and execute the command:
```bash
git clone http://repository.url
```
This command will __CREATE__ a new folder with a name like the name of the remote repository, and it will already put there information about its state (the .git folder) and information about the remote (the same one from which it was cloned). Now all you need to do is configure `user.name` and `user.email` in the `--local` space, if the global settings are not correct (you are working with the same account as other users) and you can continue working, approving changes and pushing them out again to the remote repository.


**Exercises**

1. Create new content in the local repository and add it to staging area. Commit and push the changes to your remote repository.

2. Add your tutor as a collaborator of your repository (only with the default permissions). My Github username is visible in the link with these materials.

3. Change the visibility of your remote repository to private.