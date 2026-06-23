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

:::::::: callout

## Optional Episode

If you don't want to do this section, [just head straight to the survey!](./99-survey.md)

::::::::::::::::

What if we have files that we **do not** want Git to track for us, like **backup files** created by our editor, or **intermediate** files created during data analysis?

Let's switch to our `dev` branch and create a few dummy files to demonstrate.
Make sure you're on the `dev` branch in GitHub Desktop, then create two new files in your repository folder:

- `example.csv` (an empty file)
- A text file inside a new `results/` folder called `example.txt`

Switch to GitHub Desktop and look at the Changes tab.
It will show both the new `example.csv` file and the `results/` folder as untracked files:

![](fig/08-ignore/untracked-files.png){alt="Untracked files in Changes tab"}

Putting these files under version control would be a **waste of disk space**.
What's worse, having them all listed could **distract** us from changes that actually matter.
Let's tell Git to **ignore** them by creating a `.gitignore` file.

### Creating a `.gitignore` File

Open your text editor and create a new file called `.gitignore` in the root of your repository.
(Note the dot at the beginning — this makes it a hidden file.)

Add the following lines:

```
*.csv
results/
```

These patterns tell Git to **ignore** any file whose name ends in `.csv` and everything in the `results/` directory.

Save the file. Now switch back to GitHub Desktop and look at the Changes tab.

The `example.csv` file and the `results/` folder have disappeared from the list!
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

The Changes tab will now be empty — there are no more untracked or modified files to commit.

### Benefits of `.gitignore`

Using `.gitignore` helps us **avoid accidentally adding files** to the repository that we don't want.

Let's see this in action. Try to add `example.csv` by checking it in the Changes tab.
You'll notice something interesting:

When you try to check a file that's ignored, GitHub Desktop will show a warning (or simply not let you check it):

![](fig/08-ignore/ignored-warning.png){alt="Warning about ignored file"}}

This protects you from accidentally committing files you didn't mean to track.

### Viewing Ignored Files

If you want to see what files are currently being ignored, you can switch to the **View** menu in GitHub Desktop and enable **Show Hidden Files**.
Then, in your file explorer, you can see the ignored files are still there — Git just won't track them:

:::::::: tab

### Windows

In File Explorer, go to **View > Show > Hidden items**.

### Mac

In Finder, press <kbd>Cmd</kbd>+<kbd>Shift</kbd>+<kbd>.</kbd> to toggle hidden files.

::::::::::::

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

Now when someone clones your repository, the `results/` directory will exist, even though you're ignoring the actual output files (`*.csv`).

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
