---
author: chatgpt
datetime: 2023-10-20
title: Git commands
slug: "git"
featured: false
draft: false
tags:
  - git
ogImage: ""
description: Useful git commands.
---
# Git

<img src="https://acltc-blog-pics.s3.amazonaws.com/uploads/blog_pic/blog_pic/69/1color-darkbg_2x.png" />

## Table of Contents

## Basic Commands:-

1. `git init`: Initializes a new Git repository in the current directory.
2. `git clone <repository-url>`: Clones a remote Git repository to your local machine.
3. `git add <file>`: Stages changes for the next commit.
4. `git commit -m "Commit message"`: Commits the staged changes with a descriptive message.
5. `git status`: Shows the current status of your working directory.
6. `git log`: Displays a log of all commits in the repository.
7. `git diff`: Shows the differences between the working directory and the last commit.
8. `git branch`: Lists all branches in the repository.
9. `git checkout <branch-name>`: Switches to the specified branch.
10. `git checkout -b <new-branch-name>`: Creates and switches to a new branch.

## Branching and Merging:-

11. `git merge <branch-name>`: Merges changes from the specified branch into the current branch.
12. `git rebase <branch-name>`: Applies changes from the specified branch on top of the current branch.
13. `git branch -d <branch-name>`: Deletes a branch.
14. `git stash`: Temporarily saves your changes in a stash.
15. `git cherry-pick <commit>`: Applies a specific commit to the current branch.

## Working with Remotes:-

16. `git remote -v`: Lists the remote repositories associated with your local repository.
17. `git fetch`: Fetches changes from the remote repository but doesn't merge them.
18. `git pull`: Fetches and merges changes from the remote repository into the current branch.
19. `git push`: Pushes your local changes to the remote repository.
20. `git remote add <name> <url>`: Adds a new remote repository.

## Viewing and Comparing:-

21. `git show <commit>`: Shows details of a specific commit.
22. `git blame <file>`: Displays who last modified each line of a file.
23. `git log --graph --oneline --all`: Visualizes the commit history as a graph.

## Tagging and Releases:

24. `git tag`: Lists the tags in the repository.
25. `git tag -a <tag-name> -m "Tag message" <commit>`: Creates an annotated tag.
26. `git push --tags`: Pushes tags to the remote repository.

## Advanced Topics:-

27. `git reset --hard <commit>`: Resets the branch to a specific commit, discarding all changes after that commit.
28. `git reflog`: Lists a history of recent branch changes and other Git commands.
29. `git submodule`: Manages Git submodules within your repository.
30. `.gitignore`: A configuration file that specifies which files and directories to ignore in Git.

## Collaboration and Code Review:-

31. Pull Request: A mechanism for proposing and reviewing code changes in a collaborative environment (common in Git hosting platforms like GitHub and GitLab).
32. Code Review: The process of reviewing and providing feedback on code changes.
33. Fork: Creating a personal copy of a repository in your GitHub account for making changes independently.

## Stashing and Cleaning:-

34. `git clean -n`: Lists untracked files in your working directory.
35. `git clean -f`: Removes untracked files from your working directory.
36. `git stash pop`: Applies the most recent stash and removes it from the stash list.
37. `git stash list`: Lists all stashes.

## Reverting Changes:-

38. `git revert <commit>`: Creates a new commit that undoes changes from a specific commit.
39. `git reset --soft <commit>`: Resets the branch to a specific commit, keeping changes staged.
40. `git reset --mixed <commit>`: Resets the branch to a specific commit, unstaging changes.
41. `git reset --hard <commit>`: Resets the branch to a specific commit, discarding all changes after that commit.

## Ignoring and Excluding Files:-

42. `.gitignore`: A configuration file to specify which files and directories to ignore in Git.
43. `.git/info/exclude`: An alternative way to specify ignored files for a single repository.

## Submodules:-

44. `git submodule add <repository-url> <path>`: Adds a submodule to your repository.
45. `git submodule update --init --recursive`: Initializes and updates submodules.
46. `git submodule update --remote`: Updates submodules to the latest commit.

## Aliases:-

47. `git config --global alias.<alias-name> <git-command>`: Defines custom Git aliases for frequently used commands.
48. `git alias`: Lists all your defined aliases.

## Bisecting:-

49. `git bisect start`: Starts the binary search for a faulty commit.
50. `git bisect bad` and `git bisect good`: Marks commits as bad or good during the binary search.

## TimeStamp of commits
You can use the `git log` command to retrieve the timestamp of a specific commit or the entire commit history. Here are some common commands to get the time of a commit in Git:

1. To get the timestamp of the latest commit in the current branch:
```bash
git log -1 --format="%cd" --date=iso
```

This command uses the `git log` command with the `-1` option to limit the output to the latest commit, `--format="%cd"` to specify the format for the commit date, and `--date=iso` to display the date in ISO 8601 format.

2. To get the timestamp of a specific commit by its SHA-1 hash (replace `commit_hash` with the actual hash):
```bash
git log -1 --format="%cd" --date=iso commit_hash
```

3. To get the timestamps of all commits in the current branch:
```bash
git log --format="%cd" --date=iso
```

These commands will display the commit date and time in ISO 8601 format, which looks like "YYYY-MM-DD HH:MM:SS ±TTTT." You can adjust the date format to your liking by changing the `--format` parameter.

If you want to view additional information about the commit, such as the author, commit message, and more, you can omit the `--format` option. The default `git log` format includes this information. For example:

```bash
git log -1 --date=iso commit_hash
```

Replace `commit_hash` with the actual hash of the commit you're interested in, and the command will display details about that specific commit, including its timestamp.

To get the timestamp of a specific commit in a remote repository, you need to fetch the information from the remote repository first and then use the `git log` command. Here's how you can do it:

1. Fetch the latest changes from the remote repository:

```bash
git fetch origin
```

Replace `origin` with the name of the remote repository if it's different.

2. Retrieve the timestamp of a specific commit in the remote repository by its SHA-1 hash (replace `commit_hash` with the actual hash):

```bash
git log -1 --format="%cd" --date=iso origin/branch_name commit_hash
```

Here, `branch_name` is the name of the branch where the commit is located. Replace it with the correct branch name.

The `origin/branch_name` specifies the remote branch. This command fetches the commit information from the remote repository and displays the commit date and time in ISO 8601 format for the specific commit you're interested in.

If you want to retrieve the timestamp for a commit in a remote repository without fetching the latest changes, you can do so by specifying the remote repository and branch explicitly in the `git log` command. Here's an example:

```bash
git log -1 --format="%cd" --date=iso remote_url/branch_name commit_hash
```

Replace `remote_url` with the URL of the remote repository and `branch_name` with the name of the branch in the remote repository where the commit is located.

Remember that you need the necessary permissions and access to the remote repository to fetch and view its commit history.

<hr/>

## Bonus
### List git repo in remote
`git ls-remote --heads origin`

These commands and concepts cover a wide range of Git functionality, and mastering them will help you effectively manage your Git repositories and collaborate with others.

### Change most recent Git commit message
`git commit --amend`

### Merge abort
`git merge --abort`

Reverting Working Copy to Most Recent Commit

To revert to the previous commit, ignoring any changes:

`git reset --hard HEAD`

where HEAD is the last commit in your current branch
Reverting The Working Copy to an Older Commit

To revert to a commit that's older than the most recent commit:

### Resets index to former commit; replace '56e05fced' with your commit code
`git reset 56e05fced` 

### Moves pointer back to previous HEAD
`git reset --soft HEAD@{1}`

`git commit -m "Revert to 56e05fced"`

### Updates working copy to reflect the new commit
`git reset --hard`

### Push your changes to respective branch
`git push -f`

