<img align="right" src="../logo-small.png">

# Lab: Storing Additional Information in Your Repository
In this chapter, we will cover the following recipes:

Adding your first Git note
Separating notes by category
Retrieving notes from the remote repository
Pushing Git notes to a remote repository
Tagging commits in the repository

#### Tutorial Overview

Git is powerful in many ways. One of the most powerful features of Git is that it has immutable history. This is powerful because nobody can squeeze something into the history of Git without it being noticed by the people who have cloned the repository. This also causes some challenges for developers, as some would like to change the commit messages after a commit has been released. This is possible in many other version control systems, but because of the immutable history with Git, it has Git notes. A Git note is essentially an extra refs/notes/commits reference in Git. Here, you add additional information to the commits that can be displayed when running a git log command. You can also release the notes into a remote repository so that people can fetch the notes.


#### Pre-reqs:
- Google Chrome (Recommended)

#### Lab Environment
There is no requirement for any setup.

There should be terminal opened already. You can also open New terminal by Clicking `File` > `New` > `Terminal` from the top menu.

Run the following command in **terminal**:
`cd ~/work/git/12/ && mv git .git`

**Note:** To copy and paste: use **Control-C** and to paste inside of a terminal, use **Control-V**

You can access lab at `<host-ip>:<port>/lab/workspaces/lab6`

