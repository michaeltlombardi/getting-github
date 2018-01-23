# Getting GitHub
_Project for helping folks new to Git grasp the workflow._

This project will help walk you the [fork-and-branch workflow](https://blog.scottlowe.org/2015/01/27/using-fork-branch-git-workflow/) with git and GitHub.
We're borrowing heavily from Scott Lowe's blog post linked above.

This project also serves as a [kata](https://en.wikipedia.org/wiki/Kata_(programming)), a small daily exercise to help you get used to the workflow and build memory through repetition.

For this exercise, you will need:

+ An active GitHub account (we assume you're logged in) - you can [sign up here](https://github.com/join)
+ A terminal of some sort
+ The git commandline application installed on your computer - you can [download and install it from here](https://git-scm.com/downloads)
+ An editor of some sort to write text in - you can use whatever you like, but we recommend [Visual Studio Code](https://code.visualstudio.com/) (it's what all the images in this document are from) - you can [download and install it from here](https://code.visualstudio.com/download)

## Exercise Overview
In this exercise, we're going to do some one-time setup:

1. Fork this repository to your personal namespace
2. Clone your fork of the repository to your local system
3. Add a git remote branch for the upstream project

The following steps will be performed every time we want to practice this workflow:

0. Ensure your local clone is up to date with the upstream project
1. Create a feature branch to track your changes
2. Make your changes to the new branch
3. Commit the changes to the branch
4. Push your changes to your fork
5. Open a pull request from your new branch to the upstream repository
6. Clean up after your pull request is merged

### Fork This Repository
The first thing we're going to do is fork this repository.
You will need to navigate [to this project's GitHub page](https://github.com/michaeltlombardi/getting-github) and click the "Fork" button in the top right corner.

For more information on forking a repository on GitHub, see [GitHub's documentation](https://guides.github.com/activities/forking/).

### Clone Your Fork
Now that you have forked a copy of the project you will need to clone it - that is, download a copy - to your computer.

Helpfully, projects on GitHub include a green "Clone or download" button towards the top-right of the project page.

The cloning URL for your project should look something like this:

```text
https://github.com/username/getting-github.git
```

Where `username` is your github username.

**Note:** _We're using the HTTPS option for now to avoid working with SSH keys._

Once you've copied your clone url, you'll need to open your terminal and navigate to your home folder.
The following command should help with this if you're on Linux or MacOS:

```bash
cd ~
```

While on Windows you can use the following, if you've got PowerShell:

```powershell
Push-Location $HOME
```

In either case, once you're in your home folder it is time to clone your fork:

```bash
git clone https://github.com/username/getting-github.git
```

**Note:** _Make sure to replace `username` with your actual GitHub username, if you're copying from the example!

You should see something like the following (though the file count won't be the same):

```text
Cloning into 'getting-github'...
remote: Counting objects: 4, done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 4 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (4/4), done.
```

You can now set your location to be inside your clone of the project and verify it:

```bash
cd ./getting-github
git status
```

You should see output like this:

```text
On branch master
Your branch is up to date with 'origin/master'.
```

All going well, you should now have a full local copy of the project.

### Add Upstream as a Remote
When using the fork and branch workflow you will want to keep your fork up-to-date with the project's changes.
To do so, you need to track those changes.
We do this by adding the original project (known as the _upstream_ repository) to our list of remotes for our local fork.

First, we want to verify what remotes we are tracking:

```bash
git remote -v
```

You should get results that look like this (with your GitHub username instead of `username`):

```text
origin  https://github.com/username/getting-github.git (fetch)
origin  https://github.com/username/getting-github.git (push)
```

The way we add our upstream project is to locate the clone link for the parent project (using the same clone or download button you used to find your own fork's URL, but on the [upstream project's page](https://github.com/michaeltlombardi/getting-github)).

This URL won't change, so you should be able to just copy the following block into your terminal and run it:

```bash
git remote add upstream https://github.com/michaeltlombardi/getting-github
git remote -v
```

You should get back results like these (again, replacing `username` with your GitHub username):

```text
origin  https://github.com/username/getting-github.git (fetch)
origin  https://github.com/username/getting-github.git (push)
upstream        https://github.com/michaeltlombardi/getting-github (fetch)
upstream        https://github.com/michaeltlombardi/getting-github (push)
```

You've added the upstream project as a remote.
This is the last of the one-time setup steps - the rest of these steps will be followed from now on whenever you're working on the project.

### Sync with Upstream
If you're performing this exercise for the first time immediately after doing the setup, this step isn't _strictly_ necesssary, but it is helpful to go through the process and it certainly won't hurt any.

Before you make any changes to your project you want to ensure your local copy is up to date with the upstream project.
You do this by performing the following steps:

1. Make sure you've checked out the master branch locally
2. Fetch all updates from the upstream project
3. Update your local master branch with those changes, if any.
4. Push your updated copy of master to your fork.

For any project where `origin` is your fork on github and `upstream` is the project you're syncing to, the following code block will work:

```bash
git checkout master
git fetch upstream
git rebase upstream/master
git push -u origin master
```

**Note:** _You'll want to run these commands one at a time, making sure each one passes without errors before you go onto the next one._
_There are shortcuts and faster ways to do this, or to make it all work on oneline, but when you're practicing this kata it's important to do them one by one to become more familiar with the commands._

Let's go through those commands one by one.

First, you make sure you're on the master branch of your local copy by using the `git checkout master` command.
This way you won't accidentally sync a working branch to upstream when you don't mean to.

The output should look like this if you switch from a branch other than master:

```text
Switched to branch 'master'
Your branch is up to date with 'origin/master'.
```

Or like this if you were already on master (it won't hurt to call checkout if you're already on that branch):

```text
Already on 'master'
Your branch is up to date with 'origin/master'.
```

Then you'll use `git fetch upstream` to retrieve all of the changes and code updates that have been made to the project since the last time you ran this command.
You may or may not get output from this command, depending on if anything changed in the upstream project.

After you've retrieved the updates to upstream you call `git rebase upstream/master` to add all of those updates to your local copy.

The output should like this if it added changes from upstream:

```text

```

If your master branch is already up to date with the upstream project, you should see output like this:

```text
Current branch master is up to date.
```

Finally, you call `git push -u origin master` to push your changes to your fork on GitHub and make sure it's tracking your fork and not upstream.
This can be helpful if you've done something to change where your master branch points for pushing and doesn't hurt if you're pointed correctly already.

If you are pushing changes after syncing from upstream your output should look like this:

```text

```

If there was nothing to update, your output should look like this:

```text
Branch 'master' set up to track remote branch 'master' from 'origin'.
Everything up-to-date
```

Once you've run these commands (assuming no errors!) you have successfully synced your fork to master.
