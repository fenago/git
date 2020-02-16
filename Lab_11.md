<img align="right" src="../logo-small.png">

# Lab: Patching and Offline Sharing
In this chapter, we will cover the following recipes:

Creating patches
Creating patches from branches
Applying patches
Sending patches
Creating Git bundles
Using a Git bundle
Creating archives from a tree

#### Introduction
With the distributed nature of Git and the many existing hosting options available for it, it's very easy to share history between machines when they are connected through a network. In cases where the machines that need to share history are not connected or can't use the supported transport mechanisms, Git provides other methods to share history.

Git provides an easy way to format patches from existing history, sending them in an email and applying them to another repository. Git also has a bundle concept, where a bundle that contains only part of the history of a repository can be used as a remote for another repository. Finally, Git provides a simple and easy way to create an archive for a snapshot of the folder/subfolder structure of a given reference.

With the different methods provided by Git, it is easy to share history between repositories, especially where the normal push/pull methods are not available.

#### Pre-reqs:
- Google Chrome (Recommended)

#### Lab Environment
There is no requirement for any setup.

There should be terminal opened already. You can also open New terminal by Clicking `File` > `New` > `Terminal` from the top menu.

Run the following command in **terminal**:
`cd ~/work/git/12/ && mv git .git`

**Note:** To copy and paste: use **Control-C** and to paste inside of a terminal, use **Control-V**

You can access lab at `<host-ip>:<port>/lab/workspaces/lab6`

