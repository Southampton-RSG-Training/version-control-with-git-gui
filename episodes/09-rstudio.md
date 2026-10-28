---
title: "Using Git in RStudio"
slug: git-novice-rstudio
teaching: 30
exercises: 10
---

::::::::::::: questions

- How do I use Git from inside RStudio?
- How does RStudio's Git interface compare to GitHub Desktop?

:::::::::::::::::::::::

::::::::::::: objectives

- Clone a GitHub repository as an RStudio Project.
- Stage, commit, and review changes using the RStudio **Git** tab.
- Push and pull changes between RStudio and GitHub.
- Recognise which Git tasks RStudio can't do through its buttons.

:::::::::::::::::::::::

:::::::: callout


This episode is **optional**.
It covers the same ideas as the rest of the course, but using the Git tools built into RStudio rather than GitHub Desktop.
If you mostly write R code and spend your day in RStudio, you may find it more convenient to commit and push without leaving your editor.

You'll need to have worked through the earlier episodes first, as we'll refer back to concepts like **staging**, **commits**, and **remote repositories** without re-explaining them.

::::::::::::::::

## Git in RStudio

RStudio has a built-in Git interface that lets you do most of the day-to-day tasks we've seen in GitHub Desktop: viewing changes, staging files, committing, browsing history, and pushing and pulling.

The interface works slightly differently, though:

- It only works inside an **RStudio Project**. A Project is a folder that RStudio keeps track of, marked by a `.Rproj` file.
- It doesn't handle signing in to GitHub for you. We have to set that up ourselves.
- It has no buttons for a few things, like reverting a commit or merging branches. We'll point these out as we go.

![](fig/08-rstudio/rstudio-git-overview.png){alt="The RStudio window with the Git tab highlighted"}

## Setting Up

### Checking RStudio Can Find Git

Open RStudio and go to **Tools > Global Options...**, then select **Git/SVN** on the left.

Check that:

- **Enable version control interface for RStudio projects** is ticked.
- The **Git executable** box contains a path to Git. If it's empty, click **Browse...** and look for `git.exe` (Windows) or `git` (Mac).

TODO: ![](fig/08-rstudio/global-options-git.png){alt="Global Options window with Git/SVN section selected"}

If RStudio can't find Git, make sure Git is installed (GitHub Desktop includes its own copy of Git, but RStudio can't use that one), then restart RStudio.

### Signing in to GitHub

GitHub Desktop signed in to GitHub for us, but RStudio can't borrow that sign-in.
To push to GitHub from RStudio, we need to give it a **Personal Access Token (PAT)**. This is a long, randomly-generated password that lets RStudio act on your behalf, and which you can delete at any time from GitHub.

The easiest way to create and store one is with two R packages, `usethis` and `gitcreds`.
If you don't have them yet, install them by typing this in the RStudio **Console** (the pane where you type R code) and pressing <kbd>Enter</kbd>:

```r
install.packages(c("usethis", "gitcreds"))
```

Then create a token:

```r
usethis::create_github_token()
```

This opens GitHub in your web browser with a form already filled in. Scroll to the bottom and click **Generate token**, then **copy the token** that GitHub shows you. You'll only see it once!

Back in RStudio, run:

```r
gitcreds::gitcreds_set()
```

When prompted, paste in your token and press <kbd>Enter</kbd>.
RStudio will now remember it in your computer's credential store, so you shouldn't need to enter it again.

:::::::: callout

## Treat Your Token Like a Password

Anyone with your token can push to your repositories.
Never put it in a file in a repository, share it in an email, or paste it into a chat message.
If you think it's been exposed, go to **Settings > Developer settings > Personal access tokens** on GitHub and delete it.

::::::::::::::::

### Telling Git Who You Are

Git attaches your name and email to every commit.
If you've used GitHub Desktop, this will probably already be set up, but you can check (and set it if not) with:

```r
usethis::use_git_config(user.name = "Your Name", user.email = "your.email@soton.ac.uk")
```

Use the same email address as your GitHub account, so your commits are linked to you.

## Cloning the Repository as a Project

We'll work with the same `climate-analysis` repository we created earlier.

:::::::: callout

## Use a Different Folder

If you've already cloned `climate-analysis` with GitHub Desktop, **clone it again into a different folder** for this episode, or you'll end up with two copies in the same place.
Both copies are linked to the same remote repository on GitHub, so you can use push and pull to keep them in sync.

Alternatively, if you'd rather carry on using your existing copy, choose **File > New Project... > Existing Directory** and select your `climate-analysis` folder.
RStudio will notice that it's already a Git repository and turn on the Git tab for you.

::::::::::::::::

1. Go to your `climate-analysis` repository on GitHub. Click the green **Code** button, make sure the **HTTPS** tab is selected, and **copy the URL**.

![](fig/08-rstudio/github-copy-url.png){alt="GitHub Code button with the HTTPS URL highlighted"}

2. In RStudio, click **File > New Project...**, then choose **Version Control**, then **Git**.

![](fig/08-rstudio/new-project-version-control.png){alt="New Project wizard with Version Control selected"}

3. Paste the URL into the **Repository URL** box. RStudio will fill in the **Project directory name** (`climate-analysis`) automatically.

4. Next to **Create project as subdirectory of**, click **Browse...** and choose where on your computer you'd like the project to live.

![](fig/08-rstudio/clone-details.png){alt="Clone Git Repository dialog filled in"}

5. Click **Create Project**.

RStudio will clone the repository, then re-open itself inside the new project.

## Exploring the Git Tab

Once the project has opened, look at the top-right pane of RStudio.
Alongside the **Environment** and **History** tabs, there should now be a **Git** tab.
(Don't confuse RStudio's **History** tab, which lists the R commands you've typed, with Git's history!)

TODO: ![](fig/08-rstudio/git-tab.png){alt="The Git tab in RStudio"}

The Git tab is the equivalent of GitHub Desktop's **Changes** tab.
Along the top are the buttons we'll be using:

| Button                          | What it does                                     |
| ------------------------------- | ------------------------------------------------ |
| **Diff**                        | Opens the review window to see your changes      |
| **Commit**                      | Also opens the review window, ready to commit    |
| **Pull** (down arrow)           | Downloads commits from GitHub                    |
| **Push** (up arrow)             | Uploads your commits to GitHub                   |
| **History** (clock)             | Shows previous commits                           |
| **More** (cog)                  | Extra actions like **Revert** and **Ignore**     |
| **Branch** (purple icon)        | Shows the current branch and lets you switch     |

The Git tab will probably already show some files, even though we haven't edited anything.
That's because RStudio creates a few extra files when it turns a folder into a Project:

- `climate-analysis.Rproj` is the Project file itself.
- `.gitignore` is a special file telling Git to **ignore** certain files, so they never show up as changes. RStudio adds entries to it for temporary files it creates, like `.Rproj.user/` and `.Rhistory`.

TODO: ![](fig/08-rstudio/git-tab-new-files.png){alt="Git tab showing .gitignore and .Rproj files"}

Each file has a **status** icon next to it:

| Icon     | Meaning                                  |
| -------- | ---------------------------------------- |
| `?`      | Untracked: a new file Git hasn't seen    |
| `M`      | Modified: a tracked file has been edited |
| `D`      | Deleted: a tracked file has been removed |
| `A`      | Added: a new file that has been staged   |

## Staging and Committing

In GitHub Desktop, a single tick-box did two jobs: it marked a file as included in the next commit, and the **Commit** button then saved everything that was ticked.
RStudio is closer to the way Git itself works, with **staging** being a separate, explicit step.

Let's commit the new project files.

1. In the Git tab, tick the **Staged** checkbox next to `climate-analysis.Rproj` and `.gitignore`. (This is the staging area we met in the *Tracking Changes* episode.)

TODO: ![](fig/08-rstudio/staging-files.png){alt="Git tab with Staged checkboxes ticked"}

2. Click **Commit**. A new **Review Changes** window will open.

TODO: ![](fig/08-rstudio/review-changes-window.png){alt="The Review Changes window with staged files and a commit message box"}

The window has three parts:

- **Top left:** the list of changed files, with a **Staged** checkbox for each.
- **Bottom:** a **diff** for whichever file is selected, with additions in green and deletions in red, just like GitHub Desktop.
- **Top right:** a box to type your **commit message**.

3. Type a commit message:

```
Add RStudio project files
```

4. Click **Commit**.

A window will pop up showing the output of the commit, which you can close. The Git tab will now be empty again, meaning there are no changes to record.

:::::::: callout

## Staging Individual Lines

The Review Changes window lets you stage just *part* of a file.
Select a file, then in the diff at the bottom, highlight some lines and click **Stage selection** (or use **Stage chunk** to stage a whole block of changes).

This is the same **partial staging** that GitHub Desktop offers, and is useful when you've made two unrelated changes to the same file and want them in separate commits.

::::::::::::::::

### Modifying a File

Now let's make a change to an existing file.

In RStudio's **Files** pane (bottom right), click `climate_analysis.py` to open it in the editor.
Add a **docstring** at the very top of the file:

```python
""" Climate Analysis Tools """
```

Save the file using <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows) or <kbd>Cmd</kbd>+<kbd>S</kbd> (Mac).

The Git tab will now list `climate_analysis.py` with an `M` next to it.
Tick **Staged**, click **Commit**, and **review the diff** to check the docstring is the only change.
Then add the commit message:

```
Add docstring
```

and click **Commit**.

:::::::: challenge

## One More Change

Add the following line to the end of `climate_analysis.py`:

```python
# TODO: Add rainfall processing code
```

Review the change in the Review Changes window, then commit it with the message "Add rainfall processing placeholder".

When you're done, the Git tab should be empty.

::::: solution

## Solution

Open `climate_analysis.py`, add the TODO line at the end, and save.

In the Git tab, tick **Staged** next to `climate_analysis.py`, then click **Commit**.
Check that the new line appears in green at the bottom of the diff.

Enter the message `Add rainfall processing placeholder` and click **Commit**.

::::::::::::::

::::::::::::::::::

## Exploring History

To see previous commits, click the **History** button (the clock icon) in the Git tab.
This opens the Review Changes window on its **History** view:

TODO: ![](fig/08-rstudio/history-view.png){alt="History view in the Review Changes window"}

Each row shows the **commit message**, **author**, **date**, and the short **commit identifier**.
Click on a commit and the bottom half of the window shows the **diff** of exactly what changed in that snapshot.

Unlike GitHub Desktop, RStudio can't select two commits and compare them side by side.
For that, the easiest option is to view the commits on GitHub itself.

## Undoing Changes

### Discarding Uncommitted Changes

If you've edited a file but **haven't committed** the change, you can throw the edits away.

Try it: add `print("oops")` somewhere in `climate_analysis.py` and save, but don't commit.
In the Git tab, tick the file's checkbox (without clicking Commit), then click the **More** (cog) button and select **Revert...**.

TODO: ![](fig/08-rstudio/revert-file.png){alt="The More menu with Revert selected"}

RStudio will warn you that this will lose your changes. Click **Yes**, and the file is restored to how it was in the last commit.

:::::::: callout

## Revert Is Permanent

In RStudio, **Revert** means *"throw away my uncommitted edits"*, which is what GitHub Desktop calls **Discard Changes**.
It's not the same as GitHub Desktop's **Revert changes in commit**!
Once discarded, uncommitted edits can't be recovered, so double-check before you click Yes.

::::::::::::::::

### Recovering a Deleted File

The same **Revert** button recovers files you've deleted by accident.
If you delete `climate_analysis.py` from the **Files** pane, the Git tab will show it with a `D`.
Tick it, then choose **More > Revert...** and the file will return.

### Reverting a Commit

If you've already **committed** a change you want to undo, RStudio doesn't have a button for it.
We can still do it by using the **Terminal** tab, which is next to the Console in the bottom-left pane.
To undo the most recent commit by creating a new commit that reverses it, type:

```
git revert HEAD
```

Press <kbd>Enter</kbd>, and a text editor may appear asking you to confirm the commit message.
If the editor looks unfamiliar, you can usually accept the default message by closing it. If you get stuck, you can switch to GitHub Desktop, which handles this with a few clicks (see the *Exploring History* episode).

:::::::: callout

## Mixing Tools

GitHub Desktop and RStudio are both just different front-ends for the same Git repository.
You can use both on the same repository: commit in RStudio, then look at the history in GitHub Desktop, or the other way round.
Use whichever tool suits the task best.

::::::::::::::::

## Pushing and Pulling

### Pushing Your Work

To send your commits to GitHub, click the green **Push** (up arrow) button in the Git tab.

TODO: ![](fig/08-rstudio/push-button.png){alt="Push and Pull buttons in the Git tab"}

A window will show the progress of the push.
The first time you do this, RStudio may ask for your GitHub username and a password: **paste in your Personal Access Token** here, not your usual GitHub password.

When the window says it has finished, check your repository on GitHub, and your commits should be there.

The Git tab also shows how many commits you're ahead of or behind GitHub, with a message like *"Your branch is ahead of 'origin/main' by 2 commits"*.

### Pulling Changes

To download changes that someone else (or you, in another tool) has pushed to GitHub, click the blue **Pull** (down arrow) button.

As we learned earlier, it's a good habit to **pull before you start work**, to make sure you're building on the latest version.

### Resolving Conflicts

Conflicts happen exactly as they did in the *Remote Repositories* episode: if you and a collaborator edit the same part of a file, and you try to pull their changes, Git can't automatically combine them.

RStudio doesn't have a special conflict-resolution window.
Instead, a message will tell you the pull failed because of a conflict, and the conflicted file will appear in the Git tab with a `U` status.

TODO: ![](fig/08-rstudio/conflict-status.png){alt="Git tab showing a conflicted file marked with U"}

To fix it:

1. Click the file in the **Files** pane to open it. You'll see the same conflict markers we saw before (`<<<<<<<`, `=======`, `>>>>>>>`).
2. Edit the file, deleting the markers and keeping the text you want, then save.
3. In the Git tab, tick **Staged** for the fixed file.
4. Click **Commit** and write a message like `Merge changes from GitHub`.
5. Click **Push**.

:::::::: callout

## Practise Making a Conflict

If you'd like to practise, repeat the exercise from the *Remote Repositories* episode: commit a change to `README` in RStudio, edit the same line on GitHub, then try to pull.

::::::::::::::::

## Branches

The branch icon at the top right of the Git tab shows which branch you're on (probably `main`).
Click it to see a list of branches and **switch** between them.

TODO: ![](fig/08-rstudio/branch-dropdown.png){alt="Branch dropdown in the Git tab"}

To create a new branch, click the **New Branch** icon (purple, next to the branch name) and enter a name such as `dev`.
Leave **Sync branch with remote** ticked so the branch is published to GitHub, then click **Create**.

TODO: ![](fig/08-rstudio/new-branch.png){alt="New Branch dialog in RStudio"}

You can then commit on that branch and push as usual.

RStudio can't **merge** branches.
You can do this using GitHub's **Pull Requests** (see the *Branching* episode), by switching to GitHub Desktop and using **Branch > Choose a branch to merge into...**, or by typing `git merge dev` in the **Terminal** tab while on `main`.

:::::::: callout

## When to Use Which Tool

RStudio's Git tab is great for the everyday loop of **edit, stage, commit, push**, especially if you're already working in R.
GitHub Desktop is better for exploring history, comparing commits, reverting and merging.

| Task                           | RStudio    | GitHub Desktop |
| ------------------------------ | ---------- | -------------- |
| Stage and commit               | Yes        | Yes            |
| Partial (line) staging         | Yes        | Yes            |
| Push and pull                  | Yes        | Yes            |
| Browse history                 | Yes        | Yes            |
| Compare two arbitrary commits  | No         | Yes            |
| Revert a commit                | Terminal   | Yes            |
| Create and switch branches     | Yes        | Yes            |
| Merge branches                 | Terminal   | Yes            |
| Sign in to GitHub              | Manual     | Automatic      |

::::::::::::::::

:::::::: checklist

Before finishing, make sure you've:

- Created a Personal Access Token and saved it with `gitcreds::gitcreds_set()`.
- Cloned `climate-analysis` as an RStudio Project.
- Staged and committed a change using the Git tab.
- Pushed your commits to GitHub.

::::::::::::::::::

:::::::: keypoints

- RStudio's Git tab only appears in **RStudio Projects** that are Git repositories.
- RStudio can't use GitHub Desktop's sign-in, so you need a **Personal Access Token** to push to GitHub.
- In RStudio, **staging** and **committing** are separate steps: tick **Staged** in the Git tab, then click **Commit**.
- The **Review Changes** window shows diffs, a commit message box, and the commit history.
- **More > Revert...** discards *uncommitted* changes. It doesn't undo a commit.
- Use the **Push** and **Pull** buttons to synchronise with GitHub.
- Some tasks, like reverting commits and merging branches, need the **Terminal** tab or GitHub Desktop.

::::::::::::::::::
