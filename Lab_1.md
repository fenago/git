<img align="right" src="../logo-small.png">


# Committing Files
In this scenario you'll learn how to initialise a new Git repository and commit files into version control.

#### Tutorial Overview

Version control allows you to record changes to a file or set of files over time enabling you to go back to a specific version if required. Git is a Distributed Version Control System. This means that instead of having a snapshot of the latest files, you have a complete mirror of the repository on your local machine. The repository keeps track of the changes, when they occurred and who by. Having the complete repository on your local machine reduces delays due to network traffic and enables you to continue working when disconnected.

Let's get started.

#### Pre-reqs:
- Google Chrome (Recommended)

#### Lab Environment
There is no requirement for any setup.

**Note:** To copy and paste: use **Control-C** and to paste inside of a terminal, use **Control-V**

You can access lab at `<host-ip>:<port>/lab/workspaces/`

### Step 1 - Git Init
To store a directory under version control you need to create a repository. With Git you initialise a repository in the top-level directory for a project.

**Task**

As this is a new project, a new repository needs to be created. Use the git init command to 
create a repository.

**Protip**

After initialising a repository, a new hidden subdirectory called .git is created. This subdirectory contains the metadata that Git uses to store it's information. If you're interested in the details then use the command line to explore the contents.

### Step 2 - Git Status
When a directory is part of a repository it is called a Working Directory. A working directory contains the latest downloaded version from the repository together with any changes that have yet to be committed. As you're working on a project, all changes are made in this working directory.

You can view which files have changed between your working directory and what's been previously committed to the repository using the command git status.

The output of this command is called the "working tree status".

**Protip**

All files are "untracked" by Git until it's been told otherwise. The details of how is covered in the next step.

### Step 3 - Git Add
To save, or commit, files into your Git repository you first need to add them to the staging area. Git has three areas, a working directory, a staging area and the repository itself. Users move, otherwise referred to as promote, changes from the working directory, to a staging area before committing them into the repository.

One of the key approaches with Git is that commits are focused, small and frequent. The staging area helps to maintain this workflow by allowing you to only promote certain files at a time instead of all the changes in your working directory.

**Task**

Use the command git add <file|directory> to add hello-world.js to the staging area.

If you make an additional change after adding a file to the staging area then the change will not be reflected until you add the file again.

**Protip**

As described in Step 2, the git status command allows you to view the state of both the working directory and staging area at any point in time.

**Solution**

`git add hello-world.js`

`git status`

### Step 4 - Git Commit
Once a file has been added to the staging area it needs to be committed to the repository. The command git commit -m "commit message" moves files from staging to the repository and records the time/date, author and a commit message that can be used to add additional context and reasoning to the changes such as a bug report number.

Only changes added to the staging area will be committed, any files in the working directory that have not been staged will not be included.

**Task**

Use git commit -m "<commit message>" to commit the staged file.

**Protip**

Each commit is assigned a SHA-1 hash which enables you to refer back to the commit in other commands.

**Solution**

`git commit -m "Initial Hello World Commit"

### Step 5 - Git Ignore
Sometimes there are particular files or directories you never want to commit, such as local development configuration. To ignore these files you create a .gitignore file in the root of the repository.

The .gitignore file allows you to define wildcards for the files you wish to ignore, for example *.tmp will ignore all files with the extension .tmp.

Any files matching a defined wildcard will not be displayed in a git status output and be ignored when attempting the git add command.

**Task**

Add and commit a .gitignore file to the repository to ignore all *.tmp files.

**Protip**

The .gitignore should be committed to the repository to ensure the rules apply across different machines.

**Solution**

`echo '*.tmp' > .gitignore`

`git add .gitignore`

`git commit -m "gitignore file"`
