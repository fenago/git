Step 1 - Git Remote
Remote repositories allow you to share changes from or to your repository. Remote locations are generally a build server, a team members machine or a centralised store such as Github.com. Remotes are added using the git remote command with a friendly name and the remote location, typically a HTTPS URL or a SSH connection for example https://github.com/OcelotUproar/ocelite.git or git@github.com:/OcelotUproar/ocelite.git.

The friendly name allows you to refer to the location in other commands. Your local repository can reference multiple different remote repositories depending on your scenario.

Task
This environment has a remote repository location of /s/remote-project/1. Using git remote, add this remote location with the name origin.

Protip
If you use git clone, discussed in a future scenario, then the location you're cloning from will be automatically added as a remote with the name origin.


Step 1 - Git Remote
git remote add origin /s/remote-project/1


Step 2 - Git Push
When you're ready to share your commits you need to push them to a remote repository via git push. A typical Git workflow would be to perform multiple small commits as you complete a task and push to a remote at relevant points, such as when the task is complete, to ensure synchronisation of the code within the team.

The git push command is followed by two parameters. The first parameter is the friendly name of the remote repository we defined in the first step. The second parameter is the name of the branch. By default all git repositories have a master branch where the code is worked on.

Task
Push the commits in the master branch to the origin remote.


Step 2 - Git Push
git push origin master



Step 3 - Git Pull
Where git push allows you to push your changes to a remote repository, git pull works in the reverse fashion. git pull allows you to sync changes from a remote repository into your local version.

The changes from the remote repository are automatically merge into the branch you're currently working on.

Task
Pull the changes from the remote into your master branch.

In the next step we'll explore what changes have been made.

Step 3 - Git Pull
git pull origin master

Step 4 - Git Log
As described in the previous scenario you can use the git log command to see the history of the repository. The git show command will allow you to view the changes made in each commit.

In this example, the output from git log shows a new commit by "DifferentUser@JoinScrapbook.com" with the message "Fix for Bug #1234". The output of git show highlights the new lines added to the file in green.

Protip
Use the command git log --grep="#1234" to find all the commits containing #1234


Step 5 - Git Fetch
The command git pull is a combination of two different commands, git fetch and git merge. Fetch downloads the changes from the remote repository into a separate branch named remotes/<remote-name>/<remote-branch-name>. The branch can be accessed using git checkout.

Using git fetch is a great way to review the changes without affecting your current branch. The naming format of branches is flexible enough that you can have multiple remotes and branches with the same name and easily switch between them.

The following command will merge the fetched changes into master.

git merge remotes/<remote-name>/<remote-branch-name> master
We'll cover merging in more detail in a future scenario.

Task
Additional changes have been made in the origin repository. Use git fetch to download the changes and then checkout the branch to view them.

Protip
You can view a list of all the remote branches using the command git branch -r


Step 5 - Git Fetch
git fetch

git checkout remotes/origin/master



