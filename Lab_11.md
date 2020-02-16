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

You can access lab at `<host-ip>:<port>/lab/workspaces/lab11`


Creating patches
----------------

* * * * *

In this recipe, we'll learn how to make patches out of commits. Patches
can be sent via email for quick sharing or can be copied to sneakernet
devices (USB sticks, memory cards, external hard disk drives, and so on)
if they need to be applied to an offline computer or suchlike. Patches
can be useful methods to review code, as the reviewer can apply a patch
to their repository, investigate the difference, and check the program.
If the reviewer decides that the patch is good, they can publish
(`push`) it to a public repository, given that the reviewer is
the maintainer of the repository. If the reviewer rejects the patch,
they can simply reset their branch to the original state and inform the
author of the patch that more work is needed before it can be accepted.

### Getting ready

In this example, we'll clone and use a new repository. The repository is
just an example repository for Git commands and only contains some
example commits:

```
$ git clone https://github.com/PacktPublishing/Git-Version-Control-Cookbook-Second-Edition_offline-sharing.git
$ cd Git-Version-Control-Cookbook-Second-Edition_offline-sharing
$ git checkout master
```

### How to do it...

Let's see the history of the repository, as shown by `gitk` 

```
$ git log --graph --all --oneline
* 4bc2b08 (master) Calculate pi with more digits
| * 971ac91 (doc) Adds Documentation folder
| * 2a0c8d6 Add build information
| * 9d00fcc Update readme
| | * 583225a (HEAD -> develop) Adds functionality to prime-test a range of numbers
| | * f6c5713 Adds Makefile for easy building
| | * d00ffc0 Move print functionality of is_prime
| |/
|/|
* | 6e46ff8 Adds checker for prime number
|/
* 8bddff2 Adds math, pi calculation
* 6de7cef Offline sharing, patch, bundle and archive
```

 

 

There are three branches in the repository: `master`
`develop` and `doc`. All of them differ from the
others by one or more commits. On the `master` branch, we can
now create a patch file for the latest commit on the `master`
branch and store it in the `latest-commit` folder, as shown in
the following command:

```
$ git format-patch -1 -o latest-commit
latest-commit/0001-Calculate-pi-with-more-digits.patch
```

If we look at the file created by the `patch` command, we will
see the following:

```
$ cat latest-commit/0001-Calculate-pi-with-more-digits.patch

From 4bc2b08517141c2b84ae76ccaab3a380c19de8a6 Mon Sep 17 00:00:00 2001
From: John Doe <john.doe@example.com>
Date: Thu, 10 Apr 2014 09:19:29 +0200
Subject: [PATCH] Calculate pi with more digits

Dik T. Winter style

Build: gcc -Wall another_pi.c -o pi
Run: ./pi
---
 another_pi.c | 21 +++++++++++++++++++++
 1 file changed, 21 insertions(+)
 create mode 100644 another_pi.c

$ diff --git a/another_pi.c b/another_pi.c
new file mode 100644
index 0000000..86df41b
--- /dev/null
+++ b/another_pi.c
@@ -0,0 +1,21 @@
+/* Pi with 800 digits
+ * Dik T. Winter style, but modified sligthly
+ * https://crypto.stanford.edu/pbc/notes/pi/code.html
+ */
+ #include <stdio.h>
+
+void another_pi (void) {
  + printf("800 digits of pi:\n");
  + int a=10000, b=0, c=2800, d=0, e=0, f[2801], g=0;
  + for ( ;b-c; )f[b++]=a/5;
  + for (;d=0,g=c*2;c-=14,printf("%.4d",e+d/a),e=d%a)
  + for (b=c; d+=f[b]*a, f[b]=d%--g,d/=g--,--b; d*=b);
  +
  + printf("\n");
+}
+
+int main (void){
+ another_pi();
+
+ return 0;
+}
--
2.14.0
```

The previous snippet is the contents of the produced patch file. It
contains a header much like an email with the `From`
`Date` and `Subject` fields, a body with the commit
message, and, after the three dashes (`---`), the actual
patch, followed finally by two dashes (`--`),  and the Git
version used to generate the patch. The patch generated by
`git format-patch` is in the **UNIX** mailbox format, but with
a magic fixed timestamp to identify that it comes from
`git format-patch` rather than a real mailbox. You can see the
timestamp in the first line after the `sha-1` ID
**`Mon Sep 17 00:00:00 2001`**.

### How it works...

When generating the patch, Git will `diff` the commit at
`HEAD` with its parent commit and use this `diff` as
the patch. The `-1` option tells Git to only generate patches
for the last commit, and `-o latest-commit` tells Git to store
the patch in the `latest-commit` folder. The folder will be
created if it does not already exist.

### There's more...

If you want to create patches for several commits, say the last three
commits, you just pass on `-3` to `git format-patch`
instead of `-1`.

Format the latest three commits as patches in the
`latest-commits` folder:

```
$ git format-patch -3 -o latest-commits
latest-commits/0001-Adds-math-pi-calculation.patch
latest-commits/0002-Adds-checker-for-prime-number.patch
latest-commits/0003-Calculate-pi-with-more-digits.patch

$ ls -1 latest-commits
0001-Adds-math-pi-calculation.patch
0002-Adds-checker-for-prime-number.patch
0003-Calculate-pi-with-more-digits.patch
```

Creating patches from branches
------------------------------

* * * * *

Instead of counting the number of commits you need to make patches for,
you can create patches by specifying the target branch when running the
`format-patch` command.

### Getting ready

We'll use the same repository as in the previous example:

```
$ git clone https://github.com/PacktPublishing/Git-Version-Control-Cookbook-Second-Edition_offline-sharing.git
$ cd Git-Version-Control-Cookbook-Second-Edition_offline-sharing
```

Make sure we have the `develop`  branch checked out:

```
$ git checkout develop
```

### How to do it...

We'll pretend that we have been working on the `develop` 
branch and have made some commits. Now, we need to format patches for
all those commits so that we can send them to the repository maintainer
or carry them to another machine.

Let's see the commits on `develop`, not on `master` :

```
$ git log --oneline master..develop
583225a (HEAD -> develop) Adds functionality to prime-test a range of numbers
f6c5713 Adds Makefile for easy building
d00ffc0 Move print functionality of is_prime
```

Now, instead of running `git format-patch -3` to get patches
made for these three commits, we'll tell Git to create patches for all
of the commits that are not on the `master` branch:

```
$ git format-patch -o not-on-master master
not-on-master/0001-Move-print-functionality-of-is_prime.patch
not-on-master/0002-Adds-Makefile-for-easy-building.patch
not-on-master/0003-Adds-functionality-to-prime-test-a-range-of-numbers.patch
```

### How it works...

Git makes a list of commits from `develop` that are not on the
`master` branch, much like we did before creating the patches,
and makes patches for these. We can check the contents of
the `not-on-master` folder, which we specified as the output
folder (`-o`) and verify that it contains the patches as
expected:

```
$ ls -1 not-on-master
0001-Move-print-functionality-of-is_prime.patch
0002-Adds-Makefile-for-easy-building.patch
0003-Adds-functionality-to-prime-test-a-range-of-numbers.patch
```

### There's more...

The `git format-patch` command has many options besides the
`-<n>` option to specify the number of commits in order to
create patches for the `-o <dir>` for the target directory.
Some useful options are as follows:

-   `-s`, `--signoff` : Adds a
    `Signed-off-by` line to the commit message in the patch
    file with the name of the committer. This is often required when
    mailing patches to the repository maintainers. This line is required
    for patches to be accepted when they are sent to the Linux kernel
    mailing list and the Git mailing list.
-   `-n`, `--numbered` : Numbers the patch in the
    subject line as `[PATCH n/m]` .
-   `--suffix=.<sfx>` : Sets the suffix of the patch; it can be
    empty and does not have to start with a dot.
-   `-q`, `--quiet` : Suppresses the printing of
    patch filenames when generating patches.
-   `--stdout` : Prints all commits to the standard output
    instead of creating files.


Applying patches
----------------

* * * * *

Now we know how to create patches from commits, it's time to learn how
to apply them.

### Getting ready

We'll use the repository from the previous examples, along with the
generated patches, as follows:

 

```
$ cd Git-Version-Control-Cookbook-Second-Edition_offline-sharing 
$ git checkout master
$ ls -1a
.
..
.git
Makefile
README.md
another_pi.c
latest-commit
math.c
not-on-master
```

### How to do it...

First, we'll check out the `develop` branch and apply the
patch generated from the `master` branch
(`0001-Calculate-pi-with-more-digits.patch`) in the first
example.

We use the Git `am`  command to apply the patches;
`am` is short for `apply from mailbox` :

```
$ git checkout develop
Your branch is up-to-date with 'origin/develop'.
$ git am latest-commit/0001-Calculate-pi-with-more-digits.patch
Applying: Adds functionality to prime-test a range of numbers
error: patch failed: math.c:47
error: math.c: patch does not apply
Patch failed at 0001 Adds functionality to prime-test a range of numbers
The copy of the patch that failed is found in: .git/rebase-apply/patch
When you have resolved this problem, run "git am --continue".
If you prefer to skip this patch, run "git am --skip" instead.
To restore the original branch and stop patching, run "git am --abort".
```

We can resolve the conflict in line 47 (an empty line to be removed) and
continue:

```
$ git add math.c
$ git am --continue
Applying: Adds functionality to prime-test a range of numbers
```

We can also apply the `master` branch to the series of patches
that was generated from the `develop` branch, as follows:

```
$ git checkout master
Switched to branch 'master'
Your branch is up-to-date with 'origin/master'.
$ git am not-on-master/*
Applying: Move print functionality of is_prime
Applying: Adds Makefile for easy building
Applying: Adds functionality to prime-test a range of numbers
```

### How it works...

The `git am` command takes the mailbox file specified in the
input and applies the patch in the file to the files needed. Then, a
commit is recorded using the commit message and author information from
the patch. The committer identity of the commit will be the identity of
the person performing the `git am` command. We can see the
author and committer information with `git log`, but we need
to pass the `--pretty=fuller` option to also view the
committer information:

```
$ git log -1 --pretty=fuller
commit 45e49d0c4fcd44b73e11d61e025a62ab2655e42d (HEAD -> master)
Author: John Doe <john.doe@example.com>
AuthorDate: Wed Apr 9 21:50:18 2014 +0200
Commit: John Doe <john.doe@example.com>
CommitDate: Sun Jun 3 21:58:46 2018 +0200

Adds functionality to prime-test a range of numbers
```

### There's more...

The `git am` command applies the patches in the files
specified and records the commits in the repository. However, if you
only want to apply the patch to the working tree or the staging area and
not record a commit, you can use the `git apply` command.

We can try to apply the patch from the `master` branch to the
`develop` branch once again; we just need to reset the
`develop` branch first:

```
$ git checkout develop
Switched to branch 'develop'
Your branch is ahead of 'origin/develop' by 1 commit.
      (use "git push" to publish your local commits)
$ git reset --hard origin/develop
HEAD is now at c131c8b Adds functionality to prime-test a range of numbers
$ git apply latest-commit/0001-Calculate-pi-with-more-digits.patch
```

We can check the state of the repository with the `status` 
command:

```
$ git status
On branch develop
Your branch is up-to-date with 'origin/develop'.
    
Untracked files:
  (use "git add <file>..." to include in what will be committed)
    
  another_pi.c
  latest-commit/
  not-on-master/
    
nothing added to commit but untracked files present (use "git add" to track)
```

We successfully applied the patch to the working tree. We can also apply
it to the staging area and the working tree using the
`--index` option, or only to the staging area using the
`--cached` option.



Sending patches
---------------

* * * * *

In the previous example, you saw how to create and apply patches. You
can, of course, attach these patch files directly to an email, but Git
provides a way to send patches directly as emails with the
`git send-email` command. The command requires some setting
up, but how you do that is heavily dependent on your general mail and
SMTP configuration. A general guide can be found in the Git help pages
or by visiting:
[http://git-scm.com/docs/git-send-email](http://git-scm.com/docs/git-send-email)[.](http://git-scm.com/docs/git-send-email)

### Getting ready

We'll set up the same repository as in the previous example:

```
$ git clone https://github.com/PacktPublishing/Git-Version-Control-Cookbook-Second-Edition_offline-sharing.git
$ cd Git-Version-Control-Cookbook-Second-Edition_offline-sharing
```

### How to do it...

First, we'll send the same patch as the one we created in the first
example. We'll send it to ourselves using the email address we specified
in our Git configuration. Let's create the patch again with
`git format-patch` and send it with
`git send-email` :

```
$ git format-patch -1 -o latest-commit
latest-commit/0001-Calculate-pi-with-more-digits.patch
```

 

 

 

 

Save the email address from the Git configuration to a variable as
follows:

```
$ emailaddr=$(git config user.email)
```

Send the patch using the email address in both the `--to` and
`--from` fields:

```
$ git send-email --to $emailaddr --from $emailaddr latest-commit/0001-Calculate-pi-with-more-digits.patch
latest-commit/0001-Calculate-pi-with-more-digits.patch
(mbox) Adding cc: John Doe <john.doe@example.com> from line 'From: John Doe <john.doe@example.com>'
  OK. Log says:
  Server: smtp.gmail.com
  MAIL FROM:<john.doe@example.com>
  RCPT TO:<john.doe@example.com>
  From: john.doe@example.com
  To: john.doe.example.com
  Subject: [PATCH] Calculate pi with more digits
  Date: Mon, 14 Apr 2014 09:00:11 +0200
  Message-Id: <1397458811-13755-1-git-send-email-john.doe@example.com>
  X-Mailer: git-send-email 1.9.1 
```

Checking your email will reveal a new email in your inbox.

### How it works...

As we saw in the previous examples, `git format-patch` creates
the patch files in the Unix mbox format, so only a little extra effort
is required to allow Git to send the patch as an email. When sending
emails with `git send-email`, make sure your **Mail User
Agent** (**MUA**) does not break the lines in the patch files, replace
tabs with spaces, and so on. You can test this easily by sending a patch
to yourself and checking whether it can be applied cleanly to your
repository.

 

### There's more...

The `send-email` command can, of course, be used to send more
than one patch at a time. If a directory is specified instead of a
single patch file, all the patches in that directory will be sent. We
don't even have to generate the patch files before sending them; we can
just specify the same range of revisions we want to send as we would
have specified for the `format-patch` command. Then, Git will
create the patches on the fly and send them. When we send a series of
patches this way, it is good practice to create a cover letter with a
bit of explanation about the patch series that follows. A cover letter
can be created by passing `--cover-letter` to the
`send-email` command. We'll try sending patches for the
commits on `develop`, since it is branched from
`master` (the same patches as in the second example), as
follows:

```
$ git checkout develop
Switched to branch 'develop'
Your branch is up-to-date with 'origin/develop'.
$ git send-email --to john.doe@example.com --from  
 john.doe@example.com --cover-letter --annotate origin/master
   /tmp/path/for/patches/0000-cover-letter.patch
   /tmp/path/for/patches/0001-Move-print-functionality-of-is_prime.patch
   /tmp/path/for/patches/0002-Adds-Makefile-for-easy-building.patch
   /tmp/path/for/patches/0003-Adds-functionality-to-prime-test-a-range-of-numbers.patch
   (mbox) Adding cc: John Doe <john.doe@example.com> from line 'From: John Doe <john.doe@example.com>'
   OK. Log says:
   Server: smtp.gmail.com
   MAIL FROM:<john.doe@example.com>
   RCPT TO:<john.doe@exmample.com>
   From: john.doe@example.com
   To: john.doe@example.com
   Subject: [PATCH 0/3] Cover Letter describing the patch series
   Date: Sat, 14 Jun 2014 23:35:14 +0200
   Message-Id: <1397459884-13953-1-git-send-email-john.doe@example.com>
   X-Mailer: git-send-email 1.9.1
   ...
```

We can check our email inbox and see the four emails we sent: the cover
letter and the three patches.

 

Before sending the patches, the cover letter is filled out and, by
default, has `[PATCH 0/3] ` (if sending three patches) in the
subject line. A cover letter with only the default template subject and
body won't be sent as default. In the scripts that come with this
chapter, the `git send-email` command invokes the
`--force` and `--confirm=never` options. This was
done for script automation to force Git to send the mails even though
the cover letter has not been changed from the default. You can try to
remove these options, put in the `--annotate` option, and run
the scripts again. You should then be able to edit the cover letter and
emails that contain the patches before sending them.

