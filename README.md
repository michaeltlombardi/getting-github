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

```text
remote: Counting objects: 1, done.
remote: Total 1 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (1/1), done.
From https://github.com/michaeltlombardi/getting-github
 * [new branch]      add-sync-step -> upstream/add-sync-step
   538632f..5e19386  master        -> upstream/master
```

**Note:** _This example was for retrieving changes from this project._
_Projects will have different URLs, branches, and hashes._

```

After you've retrieved the updates to upstream you call `git rebase upstream/master` to add all of those updates to your local copy.

The output should like this if it added changes from upstream:

```text
First, rewinding head to replay your work on top of it...
Fast-forwarded master to upstream/master.
```

If your master branch is already up to date with the upstream project, you should see output like this:

```text
Current branch master is up to date.
```

Finally, you call `git push -u origin master` to push your changes to your fork on GitHub and make sure it's tracking your fork and not upstream.
This can be helpful if you've done something to change where your master branch points for pushing and doesn't hurt if you're pointed correctly already.

If you are pushing changes after syncing from upstream your output should look like this:

```text
Counting objects: 1, done.
Writing objects: 100% (1/1), 652 bytes | 652.00 KiB/s, done.
Total 1 (delta 0), reused 0 (delta 0)
To https://github.com/michaeltlombardi/getting-github.git
   538632f..5e19386  master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```

**Note:** _As before, the details for the push will vary from project to project and depending on the changes being synced.

If there was nothing to update, your output should look like this:

```text
Branch 'master' set up to track remote branch 'master' from 'origin'.
Everything up-to-date
```

Once you've run these commands (assuming no errors!) you have successfully synced your fork to master.

### Checkout New Working Branch
You should always work on a branch _other than_ master for [lots](http://waterstreetgm.org/git-why-you-should-never-commit-directly-to-master/) [of](https://softwareengineering.stackexchange.com/questions/335654/git-what-issues-arise-from-working-directly-on-master) [reasons](http://www.yegor256.com/2014/07/21/read-only-master-branch.html) which all add up to making working with other people on projects easier and safer.

If you've [synced your local copy and fork with upstream](#sync-with-upstream) you should already have the master branch checked out locally.
If not, go ahead and sync before you continue.

Once you have master checked out locally you'll want to create a new branch based off of it.

```bash
git checkout -b add_daily_entry
```

The example above would give the following output:

```text
Switched to a new branch 'add_daily_entry'
```

The command created a new branch, `add_daily_entry`, which matches the updated master branch.
This way all of the changes you make will start from a known good state.

**Note:** _The name of the branch can be anything you want._
_It's best to be descriptive of the work you intend to do._

### Make Changes to Working Branch
Now that you have a working branch to use it is time to make some changes.
For the purposes of this kata you'll need to add a subfolder to the `entries` folder.
You should name this folder the same as your github account name.

In my case, the folder is `michaeltlombardi`.

You can do this however you like, but here's the code to create the folder from the terminal you're doing the rest of this kata in:

```bash
mkdir ./entries/username
```

**Note:** _Make sure you replace `username` with your actual username._
_This will work whether you're on Linux, MacOs, or Windows._

Once you've created the folder you'll need to add your journal entry into that folder.
The name of the file should be `YYYY-MM-DD.md`, using today's date.
For example, when setting up this project, I added the journal entry `2018-01-22.md`.

Everyday you repeat this kata you'll be adding a new file to your folder.
You can put whatever you like in the file, or nothing at all (I put silly messages in mine).
Both options are perfectly fine.

### Commit Your Changes
Just saving your work on your computer while on your working branch isn't enough to get that work into version control.
We can verify that by checking the status:

```bash
git status
```

You should see output like this, but with your username instead of `username`:

```text
On branch add_daily_entry
Untracked files:
  (use "git add <file>..." to include in what will be committed)

        entries/username/

nothing added to commit but untracked files present (use "git add" to track)
```

Git errs on the side of caution and won't automatically include the changes you make unless you tell it to.
You can make sure that git tracks your changes by using the following command:

```bash
git add entries/username
```

This won't produce any output but if we run `git status` again we'll get something like this:

```text
On branch add_daily_entry
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   entries/username/YYYY-MM-DD.md
```

**Note:** _The output should show your username and the current date if you created the folder and journal entry files as instructed above._

Now that the changes are being tracked you can commit them - that is, save them in git - after which you can retrieve your work even if the files are deleted or changed in the meantime, so long as the git repository itself isn't lost.
Once you push these changes they'll survive even if you accidentally delete your entire local copy of the project.

It's important to [write good commit messages](https://chris.beams.io/posts/git-commit/) for everyone who is working on the project and for those users who may have to go investigating the project to discover bugs and history.

**Note:** _Commit messages are **not** a replacement for changelog entries - they're for maintainers first and users second, whereas changelogs' audience priorities are reversed._

For this kata, it's okay to just include a simple commit message.
The link above includes lots of good tips for writing excellent commit messages but that's slightly outside the scope of this kata.

For this kata, the following is fine:

```bash
git commit -m 'Add daily journal for username'
```

You should get output like this:

```text
[add_daily_entry ab44aaf] Add daily journal for username
 1 file changed, 24 insertions(+)
```

**Note:** _The hash after `add_daily_entry` and the insertions count are probably different - that's normal and expected._

### Push Changes to Your Fork
Now that you have committed your changes locally you can push them to your fork of the project.
This makes your changes visible and available to other maintainers and allows you to submit a Pull Request to get your changes into the upstream project.

But first, you need to actually push your changes.

```bash
git push -u origin add_daily_entry
```

**Note:** _You may notice this is the same command we used to push the synced updates to our fork's master branch - the difference is that we're on our working branch and pushing that branch up to our fork._

You should get output that looks like this (the details may vary slightly):

```text
Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 1.58 KiB | 1.58 MiB/s, done.
Total 3 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/username/getting-github.git
 * [new branch]      add_daily_entry -> add_daily_entry
Branch 'add_daily_entry' set up to track remote branch 'add_daily_entry' from 'origin'.
```

### Submit Pull Request to Upstream
For this step you'll return to GitHub in the browser.
You'll want to navigate to the upstream project's Pull Requests - in this case [Getting-GitHub](https://github.com/michaeltlombardi/getting-github/pulls).

Once you have that page open you can click the green "New pull request" button to start the process.
Underneath the "Compare changes" title is a few sentences and a link to compare across forks.
Click this link to compare your fork to the upstream project (your fork should be the one labeled "head fork", the "base fork" should be "michaeltlombardi/getting-github" for this example).

For this kata you want to compare your working branch (`add_daily_entry` if you used the branch name from our examples) to master.
Once you've selected the appropriate choices you should be able to click the green "Create pull request" button.

You don't need to change anything else.

Now that your Pull Request is submitted, one of the maintainers will be along within a day or so (usually!) to review your Pull Request.
Good practice is to never merge a pull request without a review by a second person and to never merge your own changes.

The maintainers will try to review and merge your code quickly, but please be patient.

### Clean Local Repository
Once your PR is merged into the upstream project you can delete your working branch.
Normally, this isn't strictly necessary but it helps to declutter your local copy of the repository.
However, in the case of this kata, you'll always be recreating the `add_daily_entry` branch each day if you follow the instructions exactly.
You will _definitely_ get errors if you do not clean your fork between each repetition of the kata.

First, we checkout master again (if you want to, now is a great time to [sync your fork](#sync-with-upstream) since it puts you back on master anyway):

```bash
git checkout master
```

**REMINDER:** DO NOT PERFORM THESE STEPS UNITL **AFTER** YOUR PR IS MERGED!
THIS WILL DELETE YOUR COPY OF THE WORKING BRANCH BOTH LOCALLY AND FROM GITHUB!

First, delete the branch on your fork in GitHub:

```bash
git push -d origin add_daily_entry
```

You should get output like this:

```text
To https://github.com/username/getting-github.git
 - [deleted]         add_daily_entry
```

After that, it's time to delete your local branch:

```bash
git branch -d add_daily_entry
```

You should then get output like this:

```text
Deleted branch add_daily_entry (was ab44aaf).
```

Once the branches are deleted you need to run a command to prune the stale references from git:

```bash
git remote prune origin
```
You should get output like this:

```text
Pruning origin
URL: https://github.com/username/getting-github.git
 * [pruned] origin/add_daily_entry
```

You can verify that the branches were deleted by checking your branches:

```bash
git fetch origin
git branch -a
```

This should give you output like the following:

```text
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
  remotes/upstream/master
```

If there is no `add_daily_entry` branch listed in your origin then you have successfully cleaned up your working branch and finished an iteration of this kata.