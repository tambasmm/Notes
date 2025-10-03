# Clone_Push_Merge

## Clone-push
1. **Clone the Repository** (if you haven't already):
   ```bash
   git clone <repository-url>
   cd <repository-directory>
   ```

2. **Create a New Branch**:
   ```bash
   git checkout -b <new-branch-name>
   ```

3. **Make Your Changes**:
   - Edit your files as needed.

4. **Stage Your Changes**:
   ```bash
   git add .
   ```

5. **Commit Your Changes**:
   ```bash
   git commit -m "Your commit message"
   ```

6. **Push the Branch to the Remote Repository**:
   ```bash
   git push origin <new-branch-name>
   ```

Here's a quick example:

```bash
git clone https://github.com/your-username/your-repo.git
cd your-repo
git checkout -b feature-branch
# Make your changes
git add .
git commit -m "Add new feature"
git push origin feature-branch
```


## Force push and merge
If you need to force merge your changes into the main branch, you can use the `--force` option. However, be very careful with this, as it can overwrite changes in the main branch and potentially cause data loss. Here are the steps:

1. **Switch to the Main Branch**:
   ```bash
   git checkout main
   ```

2. **Fetch the Latest Changes**:
   ```bash
   git fetch origin
   ```

3. **Reset the Main Branch to the Latest State**:
   ```bash
   git reset --hard origin/main
   ```

4. **Merge Your Branch into Main**:
   ```bash
   git merge <your-branch-name>
   ```

5. **Force Push the Updated Main Branch to the Remote Repository**:
   ```bash
   git push --force origin main
   ```

Here's an example:

```bash
git checkout main
git fetch origin
git reset --hard origin/main
git merge feature-branch
git push --force origin main
```

**Important Note**: Force pushing should be used with caution, especially if you are working in a team, as it can overwrite changes made by others. Always communicate with your team before performing a force push.

------------------------------------------------------------------------------

## How to undo last push commit

```bash
git reset --hard HEAD~1
```
get head back to previous commit
```bash
git push --force origin <branch_name>
```
change commit message when rule violated
```bash
git commit --amend -m "<commit message>"
```

-----------------------------------------------------------------------------
## List all commit:
You can use the following Git command to see all the commit messages that you have done so far:

```sh
git log --author="Your Name" --pretty=format:"%h - %s"
```

Replace `"Your Name"` with your actual name or email address that you use for your commits. This command will list all the commit messages authored by you in a concise format.

If you want to see all commit messages regardless of the author, you can simply use:

```sh
git log --pretty=format:"%h - %s"
```

-------------------------------------------------------------------------------

## To go to a previous commit in Git
you can use the `git checkout` command followed by the commit hash. Here's how you can do it:

1. First, find the commit hash of the commit you want to go to. You can use the `git log` command to list all commits and their hashes.
   
   ```sh
   git log
   ```

2. Once you have the commit hash, use the `git checkout` command to switch to that commit.

   ```sh
   git checkout <commit-hash>
   ```

   Replace `<commit-hash>` with the actual hash of the commit you want to go to.

For example, if the commit hash is `abc123`, you would use:

```sh
git checkout abc123
```

Keep in mind that this will put your repository in a "detached HEAD" state, meaning you are not on any branch. If you want to create a new branch from this commit, you can do so with:

```sh
git checkout -b new-branch-name
```
_____________________________________________________________________________

## Deleting a Local Branch
1. **List all branches** to ensure you have the correct branch name:
   ```bash
   git branch
   ```
2. **checkout to another branch**
   ```
   git checkout <another_branch>
   ```
4. **Delete the local branch** using the following command:
   ```bash
   git branch -d <branch_name>
   ```
   If the branch hasn't been merged, you might need to use the `-D` flag to force delete:
   ```bash
   git branch -D <branch_name>
   ```

### Deleting a Remote Branch
1. **Ensure you are on a different branch** than the one you want to delete.
2. **Delete the remote branch** using the following command:
   ```bash
   git push origin --delete <branch_name>
   ```
   Alternatively, you can use:
   ```bash
   git push origin :<branch_name>
   ```

### Example
If you want to delete a branch named `feature-branch`, you would use:
```bash
git branch -d feature-branch  # Local deletion
git push origin --delete feature-branch  # Remote deletion
```
--------------------------------------------------------------------------------
## UNDO changes

1. **Discard Unstaged Changes**: If you have changes that are not yet staged, you can discard them using:
   ```sh
   git checkout -- .
   ```
   This will revert all unstaged changes in your working directory.

2. **Discard Staged Changes**: If you have staged changes that you want to discard, you can use:
   ```sh
   git reset HEAD .
   ```
   This will unstage the changes, but keep them in your working directory. You can then discard them using the previous command.

3. **Reset to Last Commit**: If you want to reset your working directory to the state of the last commit, you can use:
   ```sh
   git reset --hard HEAD
   ```
   This will discard all changes made after the last commit and reset your working directory to match the last commit.

-----------------------------------------------------------------------------
## This will ensure that your working directory is exactly as it was at the last commit you pushed.

To reset your Git repository to the **latest commit** and ensure that:

- ✅ All **new files** (untracked) are **deleted**  
- ✅ All **deleted files** are **restored**  
- ✅ All **modified files** are **reverted** to their last committed state  

You can use the following commands:


### 🧹 Step-by-Step Git Reset to Latest Commit

```bash
# 1. Remove all untracked files and directories (new files)
git clean -fd

# 2. Discard changes in tracked files (modified files)
git checkout -- .

# 3. Reset the index (staging area) to match the latest commit
git reset --hard
```



### ⚠️ Important Notes:
- These commands will **permanently delete** any uncommitted changes and untracked files.
- Make sure to back up anything important before running them.












