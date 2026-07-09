# Git

## References

[Github workshop - 29/04/2022](https://www.notion.so/Github-workshop-29-04-2022-21185e34ea0d4368a14ba1abf69432e3?pvs=21)  
[Git](https://git-scm.com/)  
[Afraid of Git? Let's break it.](https://towardsdatascience.com/afraid-of-git-lets-break-it-eaab427c73c0?source=collection_tagged---------1-------------------------------&gi=fed6fe703c4f)  
[Git Tutorials and Training | Atlassian Git Tutorial](https://www.atlassian.com/git/tutorials)

## References repository

[git-essentials.pdf](Git%20678ea8bb3cb24a9fbdf64f18fe53011f/git-essentials.pdf)  
[git-essentials-cheatsheet.pdf](Git%20678ea8bb3cb24a9fbdf64f18fe53011f/git-essentials-cheatsheet.pdf)  
[progit.pdf](Git%20678ea8bb3cb24a9fbdf64f18fe53011f/progit.pdf)

---

# Git Basics

**Disclaimer**: this tutorial is mainly local machine and command-line oriented. No GitHub Desktop, limited use of the online platform GitHub.

You can use standard bash syntax when working with git.

## 0. Installing git and linking to GitHub

### For Linux

#### Installing Git on Linux

1. Open the terminal.
2. Use the package manager to install Git:
   - For Debian-based distributions (like Ubuntu):

     ```bash
     sudo apt-get update
     sudo apt-get install git
     ```

   - For RPM-based distributions (like Fedora or CentOS):

     ```bash
     sudo dnf install git
     ```

   - For Arch Linux:

     ```bash
     sudo pacman -S git
     ```

   - For openSUSE:

     ```bash
     sudo zypper install git
     ```

3. Verify the installation:

   ```bash
   git --version
   ```

### Connecting to GitHub

Note: this procedure works when connecting with SSH, on your local machine or a server. This means that the URL you use for cloning and for setting the remote origin is the SSH URL from the **Code** button in GitHub, not the HTTPS URL.

1. Create a GitHub account if you don't have one.
2. Generate an SSH key pair (or create a personal access token for authentication, which you can own independently of your SSH key). See below: **Setting up SSH connection and multiple accounts**.
3. Add the SSH key to your GitHub account settings.
4. Configure Git with your username and email.

   Local configuration (multiple accounts, **recommended**):

   ```bash
   git config user.name "Your Name"
   git config user.email "your_email@example.com"
   ```

   Global configuration (one account for all repositories, **not recommended**):

   ```bash
   git config --global user.name "Your Name"
   git config --global user.email "your_email@example.com"
   ```

   Check all available configurations:

   ```bash
   git config --list
   ```

5. (If you don't have local repositories yet) create a new repository on GitHub, empty: no README, no `.gitignore`, no license.
6. Initialize a local Git repository:

   ```bash
   git init
   ```

   You can personalize the branch name:

   ```bash
   git init -b <branch-name>  # e.g. git init -b server_branch
   ```

   Default is `main`.

7. Add your files and commit:

   ```bash
   git add .
   git commit -m "first commit"
   ```

8. Add the remote GitHub repository: go to the GitHub repository and copy the SSH URL, then in your local git folder:

   ```bash
   git remote add origin <remote-url>
   ```

9. Push your changes to GitHub:

   ```bash
   git push origin <branch-name>
   ```

### Setting up SSH connection and multiple accounts

To set up multiple GitHub accounts on your local laptop and access repositories via the command line using Git, follow these steps.

1. Generate SSH keys for each account:

   ```bash
   ssh-keygen -t rsa -b 4096 -C "your-email@example.com"
   ```

   Save each key with a unique name in `~/.ssh/`, such as `id_rsa_account1`, `id_rsa_account2`, etc.

   Add the keys to the SSH agent:

   ```bash
   eval "$(ssh-agent -s)"
   ssh-add ~/.ssh/id_rsa_account1
   ```

2. Add SSH keys to GitHub:

   - Log into each GitHub account.
   - Go to **Settings → SSH and GPG keys**.
   - Click **New SSH key**, paste the public key from the corresponding `.pub` file, and save.

3. Create or modify the SSH configuration file `~/.ssh/config`:

   ```text
   # Account 1
   Host github.com-account1
   HostName github.com
   User git
   IdentityFile ~/.ssh/id_rsa_account1

   # Account 2
   Host github.com-account2
   HostName github.com
   User git
   IdentityFile ~/.ssh/id_rsa_account2
   ```

   Replace `account1` and `account2` with identifiers for your GitHub accounts. The user name **must** stay `git`.

4. Clone repositories and set up remote origin using the host alias:

   ```bash
   git clone git@github.com-account1:user1/repo-name.git
   ```

   Update the remote origin URL for an existing repository:

   ```bash
   git remote set-url origin git@github.com-account1:user1/repo-name.git
   ```

   Verify:

   ```bash
   git remote -v
   ```

5. Configure user attributes locally (per repository):

   ```bash
   git config user.name "Your Name"
   git config user.email "your-email@example.com"
   ```

6. Test SSH connection:

   ```bash
   ssh -T git@github.com-account1
   ```

   You should see: `Hi <user>! You've successfully authenticated, but GitHub does not provide shell access.`

7. Make sure you are connecting via SSH and not HTTPS:

   Sometimes you clone via HTTPS and Git keeps asking for authentication even after setting SSH keys. Check:

   ```bash
   git remote -v
   ```

   HTTPS URLs start with `https://github.com/...`, SSH with `git@github.com:...`.

   Set the SSH origin:

   ```bash
   git remote set-url origin git@github.com:<user>/<repository>.git
   ```

### For Windows

#### Installing Git on Windows

1. Download the latest Git for Windows installer.
2. Run the installer and follow the setup wizard (default options are fine).
3. Verify:

   ```bash
   git --version
   ```

#### Connecting to GitHub

1. As for Linux: create a GitHub account, generate an SSH key or personal access token, and add it to your GitHub account.
2. Configure Git with your username and email:

   ```bash
   git config user.name "Your Name"
   git config user.email "your-email@example.com"
   ```

3. Create a new repository on GitHub.
4. Initialize a local Git repository, add the remote GitHub repository, and push your changes as described in the Linux section.

---

## 1. Create and remove a repository locally

- `git init <REPO-NAME>` creates a new local repository.
- `sudo rm -r .git` removes the local git repository (only the `.git` folder; verify before using).

## 2. Clone an already-existing repo from GitHub

Run in your local target folder:

```bash
git clone <REPO-URL>
```

## 3. Create a file and keep track of it

Create files as you normally do.

- `git add <FILE-NAME>` stages a file. Bash globs/regex are supported.
- `git commit` accepts staged changes. This opens a text editor for a multiline message.
  - `git commit -m "MESSAGE"` writes an inline message.
  - `git commit -am "MESSAGE"` adds and commits tracked files in one step.
- `git show` shows the changes just committed.

### Commits and tags

- `git log` shows the list of commits.
  - `git log -p` shows the diff introduced in each commit.
  - `git log -<n>` shows only the last `n` commits.
  - `git log --pretty=<option>` changes the output format. Options: `oneline`, `short`, `full`, `fuller`.
- You can create a **tag** on a commit to mark a given version of your source code.

### Workflow for a repo with remote already set

```bash
git fetch      # check changes in main remote repository
git status     # check status, see if anything conflicts
git pull       # pull changes from remote to local
git add .      # add all changes
git commit -m "your commit message"
git status     # verify everything is clean
git show       # inspect the last commit if needed
git push       # push to remote
```

## 4. Branching

Branching lets you work locally on your files before pushing to the remote repository. You can branch any repo and work on it without ever pushing, e.g. to try different things or for long-running work separated from `main`.

`<remote>` below is the name or URL of the remote repo.

- `git branch` prints existing branches and highlights the current one.
  - `git branch -d <BRANCH>` deletes `BRANCH` locally. To delete it on the remote:

    ```bash
    git push <remote> --delete <BRANCH>
    ```

- `git status` shows differences between the local copy of the current branch and its remote.
- `git checkout <OTHER-BRANCH-NAME>` switches to another branch.
  - `git checkout -b <NEW-BRANCH-NAME>` creates a new branch and checks it out. To push:

    ```bash
    git push <remote> <NEW-BRANCH-NAME>
    ```

## 4.1 Merging

Merge another branch into the current one:

```bash
git merge <OTHER-BRANCH-NAME>
```

## 5. Push and pull (upload, download)

- `git push` uploads your local work (on the current branch) to the remote.
- `git pull` downloads changes from the remote branch and merges them.
- `git fetch` downloads changes without merging.

[How to push a local repo to remote](https://jdhao.github.io/2018/05/16/git-push-local-to-remote/)

Simplest workflow to push a local repo to a new remote on GitHub:

- Ensure the GitHub repo is empty or only has a README.
- Create a new **empty** repo on GitHub, with the same name as your local folder.
- Add a remote:

  ```bash
  git remote add upstream <your_repo_url>
  ```

- Push:

  ```bash
  git push upstream main
  ```

  Add `-f` if you need to force-push (e.g. different README history).

If your new branch was created locally and has no remote equivalent:

```bash
git push --set-upstream origin <branch>
# or
git push -u origin <branch>
```

## 6. `.gitignore`

[Git - gitignore Documentation](https://git-scm.com/docs/gitignore)  
[GitHub gitignore templates](https://github.com/github/gitignore)

A `.gitignore` file specifies intentionally untracked files that Git should ignore. Recommended: create it directly in the repo folder using `touch .gitignore` or `nano .gitignore`.

- Files already tracked by Git are not affected; you must untrack them before adding them to `.gitignore`:

  ```bash
  git rm --cached <FILE-NAME>
  ```

  Add and commit any edits before doing this. Then move files if needed, update `.gitignore`, and add/commit again. Often it is useful to add `.gitignore` early.

- Each line in `.gitignore` specifies a pattern. Wildcards/regex supported in the Linux environment are supported by git as well.

**Note**: `.gitignore` files should be written in ASCII or UTF-8; other encodings may be ignored by git.

---

# GitHub

## Forking

Reference: [Forking Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/forking-workflow)

> The Forking Workflow is fundamentally different than other popular Git workflows. Instead of using a single server-side repository to act as the “central” codebase, it gives every developer their own server-side repository. This means that each contributor has not one, but two Git repositories: a private local one and a public server-side one. The Forking Workflow is most often seen in public open source projects.

The main advantage is that contributions can be integrated without giving every contributor write access to a single central repository. Developers push to their own server-side repositories, and only the project maintainer can push to the official repository. This lets the maintainer accept commits from any developer without granting direct write access.

## Issues

You can open issues on your code to remind yourself of things to do.

Issues can be closed automatically by committing a message like:

```bash
git commit -m "Closed #1"
```

where `#1` is the issue number.

## Pull requests

When you want to merge a branch into `main`, you can open a pull request. This is mainly used in multi-user projects.

---

## Debugging

### Cannot add files to repo due to permissions

Example error:

```text
error: open("HelloWorld/.vs/HelloWorld/v16/Browse.VC.opendb"): Permission denied
error: unable to index file 'HelloWorld/.vs/HelloWorld/v16/Browse.VC.opendb'
fatal: adding files failed
```

**Solution**: close Visual Studio or any other IDE that is locking the files.

---

# Cheatsheet

<https://ndpsoftware.com/git-cheatsheet.html#loc=index>
