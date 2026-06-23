---
title: "Ignoring Things"
slug: git-novice-ignoring-things
teaching: 5
exercises: 0
---

::::::::::::: questions

- How can I tell Git to ignore files I don't want to track?

:::::::::::::::::::::::

::::::::::::: objectives

- "Use a `.gitignore` file to ignore specific files and explain why this is useful."

:::::::::::::::::::::::



What if we have files that we **do not** want Git to track for us, like **backup files** or **intermediate** files created during data analysis?

Let's switch to our `dev` branch and create a few dummy files to demonstrate.
Make sure you're on the `dev` branch in GitHub Desktop, then create:

- A new folder `results/` in your repository folder:
- A text file inside the `results/` folder called `example.txt`

Switch to GitHub Desktop and look at the Changes tab.
It will show both the new `example.txt` file and the `results/` folder as untracked files:

![](fig/08-ignore/untracked-files.png){alt="Untracked files in Changes tab"}

We may not want to share our results data on GitHub.  Plus, having all our results files listed could **distract** us from changes that actually matter.
Let's tell Git to **ignore** them by adding them to a `.gitignore` file.

### Creating a `.gitignore` File

Click **Repository** then **Repository settings**.

Select **Ignored files**.


Add the following:

```
results/
```

These patterns tell Git to **ignore** everything in the `results/` directory.

Save the file. Now switch back to GitHub Desktop and look at the Changes tab.

The `results/` folder has disappeared from the list!
They still exist on your disk — Git just won't track them.

The only thing GitHub Desktop now shows is the newly-created `.gitignore` file:

![](fig/08-ignore/gitignore-added.png){alt="Only .gitignore shown in Changes tab"}}

### Committing `.gitignore`

You might think we wouldn't want to track `.gitignore` itself, but everyone we're **sharing** our repository with will probably **want to ignore the same** things that we're ignoring.

So let's add and commit it. Make sure `.gitignore` is **checked** in the Changes tab, then enter a commit message:

```
Add gitignore file
```

Click **Commit to [branch]**:

![](fig/08-ignore/commit-gitignore.png){alt="Committing .gitignore"}}

The Changes tab will now be empty showing that there are no more untracked or modified files to commit.

The files are still there and you can view them in your file explorer, but they will now be ignored by Git.

### Using `.gitkeep` Files

One interesting edge case: you can't add empty directories to a repository — directories need to have files in them.

But what if your code expects a `results/` directory to exist for writing output to?
Users would run your code and it would error because the directory doesn't exist.

You can solve this by creating an empty `.gitkeep` file inside the directory.
Although `.gitkeep` is a hidden file (starts with a dot), it will ensure the directory structure is preserved in your repository.

To do this:

1. Create the `results/` directory if it doesn't exist
2. Create an empty file called `.gitkeep` inside it
3. In GitHub Desktop, this file will appear — commit it with the message "Keep results directory"

Now when someone clones your repository, the `results/` directory will exist, even though you're ignoring the actual output files.

### Common `.gitignore` Patterns

Here are some common patterns you might want in a `.gitignore`:

```
# Python
*.pyc
__pycache__/
.venv/
venv/

# Data files (keep code, not data)
*.csv
*.xlsx
data/
results/

# IDE and editor files
.vscode/
.idea/
*.swp
*~

# OS files
.DS_Store
Thumbs.db
```

Most platforms have pre-made `.gitignore` files for common languages and tools.
When you create a new repository on GitHub, it offers to generate one for you!

:::::::: callout

## `.gitignore` Templates

GitHub has a collection of `.gitignore` templates for different programming languages and frameworks.
You can find them at [github.com/github/gitignore](https://github.com/github/gitignore).

If you're starting a Python project, for example, you might copy the [Python.gitignore](https://github.com/github/gitignore/blob/main/Python.gitignore) template and use it as your starting point.

::::::::::::

:::::::: keypoints

- The `.gitignore` file tells Git which files and folders to ignore.
- GitHub Desktop automatically respects `.gitignore` and won't show ignored files in the Changes tab.
- You should commit `.gitignore` to your repository so collaborators ignore the same files.
- Use `.gitkeep` files to preserve directory structure for empty folders that your code needs.

::::::::::::::::::
