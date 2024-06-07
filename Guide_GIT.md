# Git

# References

[Github workshop - 29/04/2022](https://www.notion.so/Github-workshop-29-04-2022-21185e34ea0d4368a14ba1abf69432e3?pvs=21)

[Git](https://git-scm.com/)

[Afraid of Git? Let's break it.](https://towardsdatascience.com/afraid-of-git-lets-break-it-eaab427c73c0?source=collection_tagged---------1-------------------------------&gi=fed6fe703c4f)

[Git Tutorials and Training | Atlassian Git Tutorial](https://www.atlassian.com/git/tutorials)

# References repository

[git-essentials.pdf](Git%20678ea8bb3cb24a9fbdf64f18fe53011f/git-essentials.pdf)

[git-essentials-cheatsheet.pdf](Git%20678ea8bb3cb24a9fbdf64f18fe53011f/git-essentials-cheatsheet.pdf)

[progit.pdf](Git%20678ea8bb3cb24a9fbdf64f18fe53011f/progit.pdf)

# Git Basics

**Disclaimer**: this tutorial is mainly local machine and command-line oriented. No GitHub desktop, limited use of online platform GitHub.

You can use standard bash syntax when working on git.

## 0. Installing git and linking to GitHub

### For Linux

### Installing Git on Linux

1. Open the terminal.
2. Use the package manager to install Git:
    - For Debian-based distributions (like Ubuntu):
        
        ```
        sudo apt-get update
        sudo apt-get install git
        ```
        
    - For RPM-based distributions (like Fedora or CentOS):
        
        ```
        sudo dnf install git
        ```
        
    - For Arch Linux:
        
        ```
        sudo pacman -S git
        ```
        
    - For openSUSE:
        
        ```
        sudo zypper install git
        ```
        
3. Verify the installation by typing `git --version` in the terminal.

### Connecting to GitHub

Note: this procedure works if connecting with SSH, on your local machine, or a server. This means that the URL which you will use from now on for cloning and also for setting the remote origin is found in the SSH option of <Code> in GitHub, not the HTTPS.

1. Create a GitHub account if you don't have one.
2. Generate an SSH key pair (or create a personal access token for authentication, which you can own indipendently of your SSH key).
	(see below: **Setting up SSH connection...** 
3. Add the SSH key to your GitHub account settings.
4. Configure Git with your username and email:

	For local configuration (multiple accounts, RECOMMENDED)
    ```
    git config user.name "Your Name"
    git config User.email "your_email@example.com"
    ```
    
	For global configuration (1 account only for all repositories, NOT RECOMMENDED)
    ```
    git config --global user.name "Your Name"
    git config --global user.email "your_email@example.com"
    ```
    
    For checking all available configurations:
    ```
    git config --list
    ```

5. (If you don't have local repositories yet...) Create a new repository on GitHub, empty: no README, no .gitignore, no license.
6. Initialize a local Git repository using `git init`. You can also personalize the name of the branch by `git init -b <branch-name>`, e.g. `git init -b server_branch`. Default is `git init -b main`.
7. Add your files with `git add .` and commit them `git commit -m "first commit"`.
8. Add the remote GitHub repository: go to the GitHub repository and copy the https url, then in your local git folder:
    ```
    git remote add origin <remote-url>
    ```
9. Push your changes to GitHub:
    ```
    git push origin <branch-name>
    ```

**Setting up SSH connection and multiple accounts**
To set up multiple GitHub accounts on your local laptop and access repositories via the command line using Git, you need to follow these steps:

1. **Generate SSH Keys for Each Account**:
   - Use `ssh-keygen` to create an SSH key for each GitHub account. For example:
     ```
     ssh-keygen -t rsa -b 4096 -C "your-email@example.com"
     ```
   - Save each key with a unique name in the `~/.ssh/` directory, such as `id_rsa_account1`, `id_rsa_account2`, etc...
   - Eventually add to the forward agent:
   	 ```
   	 eval "$(ssh-agent -s)" #check that it works
   	 ssh-add ~/.ssh/id_rsa_account1
   	 ```

2. **Add SSH Keys to GitHub**:
   - Log into each GitHub account and navigate to "Settings" > "SSH and GPG keys".
   - Click on "New SSH key", paste the public key content from the corresponding `.pub` file, and save it.

3. **Create or Modify the SSH Configuration File**:
   - Create or edit the `~/.ssh/config` file to define host aliases for each GitHub account. For example:
     ```
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
   - Eventually replace `account1` and `account2` with identifiers for your GitHub accounts.
   - PAY ATTENTION: the name of the user "git" should stay like that and not be substituted with the user name of your account.

4. **Clone Repositories and Set Up Remote Origin**:
   - Use the `git clone` command with the appropriate host alias to clone repositories for each account. For example:
     ```
     git clone git@github.com-account1:user1/repo-name.git
     ```
   - To update the remote origin URL for an existing repository, use:
     ```
     git remote set-url origin git@github.com-account1:user1/repo-name.git
     ```
   - Verify the remote origin URL with `git remote -v`.

5. **Configure User Attributes Locally**:
   - Navigate to the repository directory and configure the local Git user name and email for commits to match the respective GitHub account. For example:
     ```
     git config user.name "Your Name"
     git config user.email "your-email@example.com"
     ```
   - This step ensures that commits are attributed to the correct user for each repository.

6. **Test SSH Connection**:
   - Test your SSH connection to GitHub to ensure that authentication works correctly. For example:
     ```
     ssh -T git@github.com-account1
     ```
   - You should receive a message indicating successful authentication: "Hi <user>! You've successfully authenticated, but GitHub does not provide shell access."

By following these steps, you can manage multiple GitHub accounts on the same laptop and perform Git operations such as clone, push, and pull with the correct account credentials. Remember to replace placeholders like `your-email@example.com`, `account1`, `user1`, and `repo-name` with your actual email addresses, account identifiers, usernames, and repository names.

7. **Make sure you are connecting via SSH and not HTTPS**:
	Sometimes, you clone the repository via HTTPS and then it fails to connect you when trying to link it with an SSH key, and asks for authentication nevertheless. If you already followed all the steps above, go on, or do the previous steps after checking for the tipe of connetion that you have.
	- Check which connection do you have: HTTPS protocol starts with https://github.com, whilst SSH is git@github.com: (server)
	```
    git remote -v
    ```
    - Set the SSH origin
    ```
    git remote set-url origin git@github.com:<user>/<repository>.git
    ```
    

### For Windows

**Installing Git on Windows**

1. Download the latest Git for Windows installer.
2. Run the installer and follow the setup wizard, choosing the default options or customizing as needed.
3. Verify the installation by opening the command prompt and typing `git --version`.

**Connecting to GitHub**

1. Follow the same steps as for Linux to create a GitHub account, generate an SSH key or personal access token, and add it to your GitHub account.
2. Configure Git with your username and email as shown above.
3. Create a new repository on GitHub.
4. Initialize a local Git repository, add the remote GitHub repository, and push your changes as described in the Linux section.


### Additional Tips

- If you have multiple GitHub accounts and want to use different credentials, you can use SSH aliases in your SSH config file or specify the username in the repository URL.

[SSH aliases w/ git - guide](https://www.notion.so/SSH-aliases-w-git-guide-952559c45e6e4942a2ae20048246918a?pvs=21)

- For the latest features of Git, you can compile it from source.
- If you encounter any issues, check the official Git documentation or seek help from the community.

## 1. Create and remove a repository locally

- `git init <REPO-NAME>`: creates a new, local repository
- `sudo rm -r .git`: removes the local git repository (CHECK IF REMOVES IT FROM EVERYWHERE)

## 2. Clone an already-existing repo from GitHub

- `git clone <REPO-URL>`: run, in your local target folder

## 3. Create a file and keep track of it

Just create a file as you would normally do.

- `git add <FILE-NAME>`: stage a file: add a file to the git tracking system. You can use bash's regular expressions as well.
- `git commit`: accept changes that have been staged. This will trigger git to launch a text editor to write a multi-line command.
    - `m "MESSAGE"` flag to write in-line message
    - `am "MESSAGE"` flag to ADD and COMMIT together
- `git show`: show the changes just committed.

### Commits and tags

- `git log`: access the list of commits
    - `p`: shows difference introduced in each commit
    - `<n>`: where <n> should be substituted by an integer number: show only last n commits
    - `-pretty=<option>`: changes the log output format to something *prettier* than default. <options> are `oneline`,`short`, `full`, `fuller`.
- you can create a `tag` on a commit to mark a given version of your source code

### Workflow for already existing repo already set to remote:

```
> git fetch  # check changes in main remote repository
> git status # check status of repository - should tell you if something is conflicting 
> git pull   # eventually pull changes from remote to local 
> git add .  # add all changes done to files
> git commit -m "your commit message"
> git status # check all's ok
> git show   # to check if all ok
> git push   # push to remote
```

## 4. Branching

Branching is what you do when you work locally on your files before pushing them to the remote repository. You can branch any repo, work on it, without never pushing your codes, e.g. if you want to try different things without affecting the main branch or you have to work on something for a long period of time.

`<remote>` below is the url of the remote repo.

- `git branch`: print which branch you're in
    - `-d <BRANCH>`: deletes BRANCH locally; to push this edit, run `git push <remote> --delete <BRANCH>`
- `git status`: see differences between local copy of the main branch and the remote one
- `git checkout <OTHER-BRANCH-NAME>`: switches to other branch
    - `-b <NEW BRANCH NAME>`: creates new branch; to push this edit, run `git push <remote> <NEW BRANCH NAME>`

## 4.1 Merging

`git merge <OTHER-BRANCH-NAME>`: merges your current branch and the other

## 5. Push and pull (upload, download)

- `git push`: pushes (=uploads) your local work (of your branch) into the main branch
- `git pull`: pulls (=downloads) all the work on the remote main branch on your local machine
- `git fetch`: pulls the changes without trying to merge them with what you have

[How to push a local repo to remote](https://jdhao.github.io/2018/05/16/git-push-local-to-remote/)
To push a local repo to remote on GitHub, the simplest way is:

- be sure that the local repo is empty or at most contains just a REAMDE
- create a new EMPTY repo on GitHub, with the same name of your local folder
- `git remote add upstream <your_repo_url>`
- `git push upstream main` with eventual flag -f for push if you've created a README in the local repo.

If your new branch was created locally and doesn't have a remote equivalent, use `git push --set-upstream origin <branch>` where you could also substitute the --set... flag with -u

## 6. `.gitignore`

[Git - gitignore Documentation](https://git-scm.com/docs/gitignore)

[GitHub gitignore templates](https://github.com/github/gitignore)

A gitignore file specifies intentionally untracked files that Git should ignore. It should be called `.gitignore`, as it is. Recommended way to create it is to use `touch` or `nano` directly in a linux shell in your repo's local folder.

- files already tracked by Git are not affected: you should remove them from the cache before adding their paths to the .gitignore file. Use `git rm --cached <FILE NAME>` to untrack your files. ADD AND COMMIT ANY EDIT BEFORE DOING THIS. Then move your files, add them in .gitignore, then add everything again. You may want to add the .gitignore file before everything else.
- each line in a gitignore file specifies a pattern. You may use wildcards as well: all regular expressions supported in the linux environment are supported by git as well.

## Note: .gitignore files are written in ASCII or UTF-8: be aware that any other encoding will be ignored by git.

---

# Github

## Forking

Ref: [https://www.atlassian.com/git/tutorials/comparing-workflows/forking-workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/forking-workflow)

> The Forking Workflow is fundamentally different than other popular Git workflows. Instead of using a single server-side repository to act as the “central” codebase, it gives every developer their own server-side repository. This means that each contributor has not one, but two Git repositories: a private local one and a public server-side one. The Forking Workflow is most often seen in public open source projects.
The main advantage of the Forking Workflow is that contributions can be integrated without the need for everybody to push to a single central repository. Developers push to their own server-side repositories, and only the project maintainer can push to the official repository. This allows the maintainer to accept commits from any developer without giving them write access to the official codebase.
> 

## Issues

You can open issues on your code to remind yourselves of things to do.

Issues can be closed even automatically by committing “Closed #number” where #number is the reference to the issue that you have opened in Github. 

```
git commit -m "Closed #1"
```

## Pull requests

When you want to merge a branch on a main, you can open a pull request. Mainly used by many-user projects.

---

## Debugging

### can't add files to repo due to permissions' restrictions

example:

```
error: open("HelloWorld/.vs/HelloWorld/v16/Browse.VC.opendb"): Permission denied
error: unable to index file 'HelloWorld/.vs/HelloWorld/v16/Browse.VC.opendb'
fatal: adding files failed

```

**Solution**: close Visual Studio or any other IDE working on the files

# Cheatsheet

[https://ndpsoftware.com/git-cheatsheet.html#loc=index](https://ndpsoftware.com/git-cheatsheet.html#loc=index)
