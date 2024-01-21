---
layout: post
title:  "<img src='{{site.baseurl}}/assets/img/git-and-github.assets.d/git.svg' alt='Git Icon'> Git & GitHub: An Invitation to Version Control"
date:   2024-01-12 18:00:00 +0530
categories: jekyll update
author: 'Jyotirmaya Shivottam'
---

<style>
    html, body {
      /* background-color: #0d1117; */ /* dark mode */
      background-color: #fdfdfd; /* light mode */
    }

    * {
      /* color: #c9d1d9; */ /* dark mode color */
      font-family: 'JuliaMono';
      font-size: 95%;
      letter-spacing: 0.015em;
    }

    img.teaser {
      position: relative;
      margin: 2em auto;
    }

    span.marked {
      /* color: #92fc8f; */ /* dark mode color */
      color: #2ea043; /* light mode color */
      font-weight: 600;
    }

    .tip {
      color: #e2ac0f;
      font-weight: 900;
    }

    span.tip-text {
      /* color: #f2fc0f; */ /* dark mode color */
      color: #29cd00; /* light mode color */
      font-weight: 600;
      font-style: italic;
    }
</style>

<div style="text-align: center;">

<img class="teaser" src="{{site.baseurl}}/assets/img/git-and-github.assets.d/git.svg" width=72 height=72>

</div>

This blogpost is a slightly expanded conversion of a talk, I gave to the students of CS460/CS660 at NISER, Bhubaneswar, on January 12, 2024. It is a tutorial-style introduction to Git & GitHub, and how to use them for version control. The sections are delineated to be easily navigable. This blogpost / talk is meant for beginners, and assumes no prior knowledge of Git or GitHub. It is also meant to be interactive, so that the readers can follow along and try out the `git` commands themselves.

#### Notes
1. The original slides are available in the associated [GitHub repo]({{site.baseurl}}/assets/docs/git-and-github.assets.d/git-and-github-talk.pdf) for the Coding Club blogs.
2. When you encounter a superscripted <span class="tip" title="Hover here">##</span> below, hover over it to see the associated text.

#### Checklist before we begin:
* Ensure GitHub Desktop is installed and you are signed in. For installing it on Windows and macOS, go [here](https://desktop.github.com/). For some flavors of Linux, check [here](https://github.com/shiftkey/desktop).
* Verify that [Visual Studio Code (VS Code)](https://code.visualstudio.com/download) is installed and you are signed in using GitHub. Don't forget to sign in to the extensions as well (you can check this by looking for any pending notifications on the profile icon).
* Install the GitLens extension in VS Code. You can do this by searching for "GitLens" in the Extensions tab, or by going [here](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens).


### Table of Contents
- [Some Preliminaries](#some-preliminaries)
  - [Version Control Systems and why we need them?](#version-control-systems-and-why-we-need-them)
  - [What are Git \& GitHub?](#what-are-git--github)
  - [Git Primer - Some Basic Concepts](#git-primer---some-basic-concepts)
  - [Git Primer - Some Keywords](#git-primer---some-keywords)
  - [Branching \& Merging Illustrated](#branching--merging-illustrated)
- [Managing the `24cs460(clone)` repo](#managing-the-24cs460clone-repo)
  - [`git init` and `git clone`](#git-init-and-git-clone)
  - [`git remote -v`](#git-remote--v)
  - [`git branch`, `git checkout` and `git switch`](#git-branch-git-checkout-and-git-switch)
  - [`git status`, `git add`, `git rm` \& `git restore`,](#git-status-git-add-git-rm--git-restore)
  - [`git commit` \& `git commit --amend`](#git-commit--git-commit---amend)
  - [`git push`, `git log` \& `git show`](#git-push-git-log--git-show)
  - [`git pull` \& `git fetch`](#git-pull--git-fetch)
  - [Creating a Pull Request (PR) \& Merging PRs](#creating-a-pull-request-pr--merging-prs)
  - [Syncing your Local Repo with the Upstream Repo](#syncing-your-local-repo-with-the-upstream-repo)
  - [Handling Merge Conflicts (\& `git diff`)](#handling-merge-conflicts--git-diff)
  - [`git reset` - The (_almost-_)Nuclear Option](#git-reset---the-almost-nuclear-option)
  - [`git revert` \& `git stash` - The Safer Alternatives](#git-revert--git-stash---the-safer-alternatives)
  - [`.gitignore`](#gitignore)
- [Additional Information](#additional-information)
  - [Where to get help with Git \& GitHub (\& in general)?](#where-to-get-help-with-git--github--in-general)
  - [Some notes on **GitHub** usage](#some-notes-on-github-usage)
  - [GitHub Pro for Students](#github-pro-for-students)
  - [`git config`](#git-config)
  - [`git submodule`](#git-submodule)
  - [`git <subcommand> --dry-run`](#git-subcommand---dry-run)
- [References](#references)


## Some Preliminaries
### Version Control Systems and why we need them?
* In "Version Control Systems" (VCS), "Version" refers to a specific state of the codebase at a given point in time, and "Control" refers to the ability to manage changes to the codebase's state over time. VCS are also called **Source Control Systems**.
* VCS store <span class="marked">snapshots</span> of your codebase ($\equiv$ codebse's state) at different points in time. Through these snapshots, they let you <span class="marked">compare changes</span> to files.
* They also allow you to <span class="marked">revert files / entire projects back to any state</span>, thereby preventing you from losing your work, e.g., if you make a mistake in your code or accidentally delete something.
* For CS460, since you'll be working in groups, VCS will help you <span class="marked">collaborate</span> with your group-mates.


### What are Git & GitHub?
* Git is a free and open-source distributed VCS, that works via the command line interface, i.e., a CLI tool. You can download and install it from [https://git-scm.com/downloads](https://git-scm.com/downloads).
* GitHub is a web-based hosting service for version control using Git. There are other similar services, e.g., GitLab, BitBucket, etc. There are also other VCS, e.g., Mercurial, Subversion, etc.
* <span class="tip">Tip</span>: <span class="tip-text">Git is a tool, GitHub is a service. You can use Git without GitHub, but not the other way around</span>.
* <span class="tip">Tip</span>: <span class="tip-text">Git / GitHub are not automated<sup title="You can automate backups, e.g., using local scripts & GitHub Actions, but that's not the primary purpose of GitHub.">##</sup> backup services. They are not a replacement for Dropbox / Google Drive / OneDrive etc</span>.
* <span class="tip">Sidenote</span>: There exists a related set of software called Knowledge Management Systems, e.g., Obsidian, Notion, Confluence, etc.


### Git Primer - Some Basic Concepts
* Git is a <span class="marked">distributed</span> VCS. This means that every collaborator has a complete <span class="marked">working copy<sup class="tip" title="The local copy of the codebase on which you are currently working.">##</sup></span> of the entire codebase, including its full history.
* Git stores the codebase in a <span class="marked">repository</span>, or <span class="marked">repo</span> for short. A repo is a directory that contains all the files and folders of your project, along with a special hidden folder called `.git`.
* The `.git` folder contains all the information about the history of your project, including all the snapshots of your codebase, and the changes made to it.
* <span class="tip">Tip</span>: <span class="tip-text">If you cannot see the `.git` folder in VS Code, it's because VS Code hides it by default. You can change this setting in VS Code Preferences (Search for ".git"), but it's recommended to keep it hidden.</span>
* <span class="marked">Commits</span> are snapshots of your codebase at a given point in time. Each commit has a unique identifier called a <span class="marked">hash</span>, which is a long string of characters.


### Git Primer - Some Keywords
* Some labels:
  * <span class="marked">Local</span> - The computer / server you are working on.
  * <span class="marked">Remote</span> - A GitHub repo that stores the codebase.
    * <span class="marked">Origin</span> - A GitHub repo, where you have pushed your codebase to / cloned your codebase from.
    * <span class="marked">Upstream</span> - A repo on GitHub that you have forked to your GitHub account.
* <span class="marked">Branch</span> - A parallel version of your codebase. You can have multiple branches, e.g., `main`, `dev`, `feature-1`, etc.
* <span class="marked">Forking</span> - Creating a copy of a repo on GitHub, under your GitHub account.
* <span class="marked">Pull / Merge Requests</span> - A request to merge a branch into another branch. This is how you collaborate with others on GitHub.


### Branching & Merging Illustrated
<div style="text-align: center;">

<img src="{{site.baseurl}}/assets/img/git-and-github.assets.d/brm.png" alt="Branching, Forking & Pull Requests">
<br>
Image source: [https://www.atlassian.com/git/tutorials/using-branches/git-merge](https://www.atlassian.com/git/tutorials/using-branches/git-merge)

</div>


## Managing the `24cs460(clone)` repo
1. We will work with a duplicate of the original repo. Go to https://github.com/JeS24/24cs460clone.
2. <span class="marked">Fork</span> the repo to your GitHub account. Click on the <span class="marked">Fork</span> button on the top-right corner of the page. This creates a copy of the repo under your GitHub account.
3. <span class="marked">Clone</span> the repo to your computer by clicking on the <span class="marked">Code</span> button, and then copying the URL. Check next section for further steps.
4. Make some changes to the repo (*check next sections*).
5. <span class="marked">Commit</span> the changes to your local repo (*check next sections*).
6. <span class="marked">Push</span> the changes to your GitHub repo (*check next sections*).
7. Create a <span class="marked">pull request</span> to merge your changes into the main repo (*check next sections*).
8. Watch the pull request get <span class="marked">merged</span> (*check next sections*).
9.  <span class="marked">Sync</span> your local repo with the upstream repo (*check next sections*).
10. Handle <span class="marked">merge conflicts</span> (*check next sections*).


### `git init` and `git clone`
* After copying the link, to clone an <span class="marked">existing repo</span>, you can use the `git clone <remote>` command. This creates a new repo in the current directory, and downloads the codebase from the remote repo.
  ```sh
  git clone https://github.com/<GH_USER_NAME>/24cs460clone.git
  ```
* <span class="tip">OPTIONAL</span>: To create a <span class="marked">new repo</span>, you can use the `git init` command. This creates a new repo in the current directory.
* <span class="tip">Tip</span>: <span class="tip-text">To push an existing codebase to GitHub, see: [https://www.digitalocean.com/community/tutorials/how-to-push-an-existing-project-to-github](https://www.digitalocean.com/community/tutorials/how-to-push-an-existing-project-to-github).</span>
* <span class="tip">Tip</span>: <span class="tip-text">You can also create a new repo on GitHub, and then clone it to your computer. See the instructions given by GitHub after creating a new empty repo.</span>

### `git remote -v`
* `git remote -v` shows you the <span class="marked">remote URLs</span> of your repo.
* `git remote show <name>` shows you information about the remote with the given name.
* <span class="tip">OPTIONAL</span>:
  * `git remote add / rm <name> <url>` adds / removes a new remote to your repo, with the given name and URL.
  * `git remote set-url <name> <url>` changes the URL of the remote with the given name.
  * `git remote prune <name>` removes all the remote-tracking branches that no longer exist on the remote with the given name.


### `git branch`, `git checkout` and `git switch`
* `git branch` shows you the <span class="marked">branches</span> in your repo.
* `git branch <name>` creates a new branch with the given name.
* `git checkout <name>` / `git switch <name>` switches to the branch with the given name.
* `git checkout -b <name>` / `git switch -c <name>` creates a new branch with the given name, and switches to it.
* `git checkout <commit-hash>` / `git switch <commit-hash>` switches to the commit with the given hash.
* <span class="tip">Tip</span>: <span class="tip-text">It is recommened to use `git switch` over `git checkout` for switching / creating branches.</span>
* <span class="tip">Tip</span>: <span class="tip-text">Branch names should be kept short, e.g., `main`, `dev`, `feature-1`, etc.</span>


### `git status`, `git add`, `git rm` & `git restore`,
* <span class="tip-text">Make a change to the repo, e.g., add a new file, modify an existing file, etc</span>.
* `git status` shows you the status of your repo, including the files in the <span class="marked">staging area</span><sup class="tip" title="The staging area is a special area in your repo, where you can add files before committing them.">##</sup>, and the files in the <span class="marked">working directory</span><sup class="tip" title="The working directory is the directory on your computer, where your repo is stored.">##</sup>.
* `git add -A` $\equiv$ `git add *` adds all modified files in the repo to the staging area.
* `git add <file>` adds the given file to the staging area.
* `git rm --cached <file>` removes the given file from the staging area, but keeps it in the working directory.
* `git rm <file>` / `git rm -r <folder>` removes the given file / folder from the working directory (if there are no modifications).
* `git restore <path>` undoes changes to files in the working directory.


### `git commit` & `git commit --amend`
* `git commit -m "<message>"` <span class="marked">commits<sup class="tip" title="A commit is a snapshot of your project at a particular point in time.">##</sup></span> the changes in the staging area to the <span class="tip-text">local repo</span>, with the given message. It does not affect the remote repo (till we <span class="marked">push</span>).
* `git commit -a -m "<message>"` stages ("adds") & commits all the changes to the <span class="tip-text">local repo</span>, with the given message.
* <span class="tip">Tip</span>: <span class="tip-text">Commit messages should be short, yet precise e.g., "Add README.md", "Fix typo in README.md", etc.</span>
* <span class="tip">OPTIONAL</span>:
  * `git commit --amend` lets you <span class="marked">amend</span> the last commit, i.e., change the commit message, add / remove files from the commit, etc.
  * `git commit --amend --no-edit` lets you amend the last commit, without changing the commit message.
* <span class="tip">Tip</span>: <span class="tip-text">Always try to split changes into smaller updates / commits. More snapshots is almost always a good idea. There are ways to rewrite the history of the repo, if the need arises.</span>
* <span class="tip">Tip</span>: <span class="tip-text">If, for some reason, you cannot avoid a large commit, you should write a descriptive message.</span>


### `git push`, `git log` & `git show`
* `git push <remote> <branch>` <span class="marked">pushes<sup class="tip" title="Pushing means updating the remote with any commits made locally that do not yet exist on the remote.">##</sup></span> the changes in the local repo to the remote repo, to the given branch (here, `origin` & `main`).
* `git push -u <remote> <branch>` sets the given remote repo and branch as the <span class="marked">upstream</span> for the current branch. This lets you use `git push` (and `git pull`) without specifying the remote and branch each time.
* <span class="tip">OPTIONAL</span>:
  * `git push --force` <span class="marked">forces</span> the push to the remote repo. Usually, this should be avoided.
  * `git push --delete <remote> <branch>` deletes the given branch from the remote repo.
* `git log` shows you the <span class="marked">commit history</span> of your repo. It shows each commit, along with its <span class="marked">hash</span>, author, date, and message.
  * `git log --oneline` shows you the commit history in a <span class="marked">compact</span> format.
  * `git log --graph` shows you the commit history in a <span class="marked">graphical</span> format.
* `git show <commit-hash>` shows you the changes in the given commit.


### `git pull` & `git fetch`
* `git pull <remote> <branch>` <span class="marked">pulls<sup class="tip" title="Pulling entails fetching the changes from remote and then automatically merging them into the current branch of local.">##</sup></span> the changes from the remote repo to the local repo, from the given branch (here, `origin` & `main`).
* `git fetch <remote> <branch>` <span class="marked">fetches</span> the changes from the remote repo to the local repo, but <span class="tip-text">does not merge the changes into local</span>.
* To <span class="marked">update</span> your local repo, you should <span class="marked">pull</span> the changes from the remote repo, and then <span class="marked">merge</span> them into your local repo.
* If there are no <span class="marked">merge conflicts<sup class="tip" title="Merge conflicts occur when changes are made to the same part of the same file on two different branches and Git cannot automatically determine which version to use.">##</sup></span>, `git pull` will <span class="marked">merge</span> the changes into your local repo, and <span class="marked">fast-forward</span> your branch to the latest commit on the remote repo.
* Try `git log` after pulling to see the updated commit history.
* <span class="tip">Tip</span>: <span class="tip-text">You can also see a graphical view of the commit history in VS Code, using the GitLens extension.</span>
* <span class="tip">Tip</span>: If you don't see remote's changes in local, try `git fetch` first, and then `git pull`.


### Creating a Pull Request (PR) & Merging PRs
* <span class="marked">Pull requests</span> are a way to collaborate with others on GitHub. Once you have made some changes on a branch on your repo, you can create a PR to merge your changes with another branch, typically the <span class="marked">main</span> branch of your repo, or the `main` branch of another repo, e.g., <span class="marked">upstream</span>.
* Go to your GitHub repo, and click on the <span class="marked">Pull Requests</span> tab.
* Find the branch you want to merge, and click on <span class="marked">New pull request</span>.
* Select the <span class="marked">base</span> branch, i.e., the branch you want to merge into.
* Select the <span class="marked">compare</span> branch, i.e., the branch you want to merge from.
* Click on <span class="marked">Create pull request</span>.
* Add a <span class="marked">title</span> and a <span class="marked">description</span> for the PR.
* Click on <span class="marked">Create pull request</span>.
* Wait for the PR to be <span class="marked">merged</span>.
* After the PR is merged, check the commit history of your repo, and see the changes (`git log` or GitLens).


### Syncing your Local Repo with the Upstream Repo
* <span class="tip">Tip</span>: <span class="tip-text">Always <span class="marked">pull</span> before you <span class="marked">push</span>, to avoid merge conflicts on the remote repo.</span>
* To sync:
  * `git fetch upstream` <span class="marked">fetches</span> the changes from the remote repo to the local repo, but <span class="tip-text">does not merge the changes into local</span>.
  * `git merge upstream/<branch>` <span class="marked">merges</span> the changes from the remote repo into the local repo, from the given branch (here, **`upstream`** & `main`).
  * `git push origin <branch>` <span class="marked">pushes</span> the changes from the local repo to the remote repo, to the given branch (here, `origin` & `main`).


### Handling Merge Conflicts (& `git diff`)
* <span class="marked">Merge conflicts</span> occur when changes are made to the same part of the same file on two different branches and Git cannot automatically determine which version to use.
* To see this, let us make a conflicting change to `README.md`.
* Run `git fetch`.
* `git merge upstream/main`  to start a merge. It should result in a <span class="marked">merge conflict</span>.
* `git status` shows you the files with merge conflicts.
* `git diff` shows you the conflicting changes. The conflicting changes are marked with `<<<<<<<` and `>>>>>>>`, like so:
  ```diff
  <<<<<<< HEAD (main)
  this is conflicted text from main
  =======
  this is conflicted text from feature branch
  >>>>>>> feature branch
  ```
* Check the <span class="marked">staging area</span> in VS Code to see the conflicting changes. Or, you can use `git diff --staged` to see the changes in the staging area.
* Resolve the merge conflict in VS Code by choosing the changes you want to keep, and deleting the rest. VS Code also provides some buttons to help you resolve the conflict, like `Accept Incoming Change`, `Accept Current Change`, etc.
* Once resolved, the file will be <span class="marked">staged</span> and you can <span class="marked">commit</span> and <span class="marked">push</span> it.
* <span class="tip">Tip:</span> <span class="tip-text">`git merge --abort` aborts the merge (before committing), and <span class="marked">resets</span> your repo to the state before the merge. This might be useful if you want to start over, e.g., if you made a mistake while resolving the merge conflict.</span>


### `git reset` - The (_almost-_)Nuclear Option
* Sometimes, you might want to undo a (few) commit(s) or to <span class="marked">reset</span> your repo to a previous state, and start over, e.g., if you made a mistake, or if a merge conflict is too complicated to resolve. `git reset` lets you <span class="marked">reset</span> your repo to a previous state.
* `git reset --hard <commit-hash>` resets your repo to the given commit, and <span class="marked" style="color: crimson;">deletes (!!)</span> all the commits after it.
* `git reset --soft <commit-hash>` resets your repo to the given commit, but <span class="marked">keeps (!!)</span> all the commits after it.
* `git reset ~<n>` resets your repo to the commit <span class="marked">$n$ commits before</span> the current commit.
* `git reset HEAD~<n>` resets your repo to the commit <span class="marked">$n$ commits before</span> the current commit, and <span class="marked">unstages</span> all the commits after it.
* <span class="tip">Tip</span>: <span class="tip-text">Use `git reset --hard` with caution. It is a <span class="marked" style="color: crimson;">destructive</span> command, and can result in <span class="marked" style="color: crimson;">data loss</span>.</span>


### `git revert` & `git stash` - The Safer Alternatives
* `git revert` lets you <span class="marked">undo</span> a commit, without deleting it.
* `git revert <commit-hash>` creates a new commit that <span class="marked">undoes</span> the changes in the given commit.
* <span class="tip">Tip</span>: <span class="tip-text">Use `git revert` instead of `git reset --hard` to undo commits.</span>
* `git stash` lets you <span class="marked">stash</span> your changes, i.e., save them for later. You can also name your stashes by appending `-m "<name>"` to the command.
* This is useful, when you want to <span class="marked">switch branches</span> or <span class="marked">pull remote</span>, but you have <span class="marked">uncommitted changes</span> in your current branch.
* `git stash pop` <span class="marked">pops</span> the last stash, i.e., it <span class="marked">restores</span> the last stash, and <span class="marked">deletes</span> it from the stash list.
* When you <span class="marked">stash</span> your changes, you can give it a <span class="marked">name</span>, e.g., `git stash push -m "<name>"`.
* `git stash list` shows you the list of stashes.
* `git stash show <stash-name>` shows you the changes in the given stash.


### `.gitignore`
* `.gitignore` is a file that lets you <span class="marked">ignore</span> files and folders in your repo. The files and folders in `.gitignore` are <span class="marked">not tracked</span> by git.
* You can use <span class="marked">wildcards</span> in `.gitignore`, e.g., `*.pyc`, `*.log`, `__pycache__`, etc.
* You can also use <span class="marked">negation</span> in `.gitignore`, e.g., `!*.py`, `!*.md`, etc.
* <span class="tip">Tip</span>: <span class="tip-text">You should always add `.gitignore` to the root of your repo, and commit it.</span>
* <span class="tip">Tip</span>: <span class="tip-text">You can use <span class="marked">templates</span> for `.gitignore`, e.g., ones provided by GitHub for several languages</span>.
* An example of a `.gitignore` for a typical <span class="marked">Machine Learning / Deep Learning</span> project in Python:
  ```gitignore
  # Folders
  *.pyc

  # Logs
  *.log

  # Virtual environments
  venv/
  env/
  ENV/

  # Jupyter Notebooks
  .ipynb_checkpoints/

  # Data
  data/
  *.csv
  *.tsv
  *.json
  *.zip
  *.tar.gz

  # Models
  models/
  *.pth
  *.pt
  *.ckpt
  *.h5

  # IDEs
  .vscode/

  ```


## Additional Information
The following sections contain some additional information, that you might find useful for your future projects.

### Where to get help with Git & GitHub (& in general)?
* On a terminal:
    `git <subcommand> --help` $\equiv$ `git help <subcommand>`, e.g., `git commit --help`.
* On the web:
  * Git Documentation: [https://git-scm.com/docs](https://git-scm.com/docs)
  * GitHub Documentation: [https://docs.github.com/en](https://docs.github.com/en)
  * StackOverflow: [https://stackoverflow.com/questions/tagged/git](https://stackoverflow.com/questions/tagged/git)
  * ChatGPT: [https://chat.openai.com/](https://chat.openai.com/)
ChatGPT is listed at the bottom, because it is not a reliable source of information. Always double-check any information you get from ChatGPT, especially when it comes to running commands on your computer. That said, it does usually point you in the right direction.
* Some repos to practice git:
  * [https://github.com/firstcontributions/first-contributions](https://github.com/firstcontributions/first-contributions)
  * <span class="tip">Tip:</span> <span class="tip-text">Create a repo on your GitHub account and play around with it.</span>


### Some notes on **GitHub** usage
* GitHub has a <span class="marked">file size limit</span> of 100 MiB per file, and of 1 GiB with [Git Large File Storage](https://docs.github.com/en/repositories/working-with-files/managing-large-files/about-git-large-file-storage#about-git-large-file-storage).
* It has a <span class="marked">file count limit</span> of 300 updated files per commit, at 100 MiB per file for free accounts.
* To check other GitHub repo limitations, see [this](https://docs.github.com/en/repositories/creating-and-managing-repositories/repository-limits).
* <span class="tip">Tip</span>: <span class="tip-text">Don't store large monolithic files, like dataset archives, in your repo.</span>
* <span class="tip">Tip</span>: <span class="tip-text">Don't store too many files, like images from extracted archives, or coding artifacts, like compiled binaries, in your repo.</span>
* <span class="tip">Tip</span>: <span class="tip-text">If you have to store very large files, up to 2 GB, you can use [Git Large File Storage](https://docs.github.com/en/repositories/working-with-files/managing-large-files/about-git-large-file-storage#about-git-large-file-storage). It requires some additional configuration.</span>
* <span class="tip">Tip</span>: <span class="tip-text">GitHub also provides access to [github.dev](github.dev) or [vscode.dev](vscode.dev), to view and edit your repos in the browser itself.</span> Try pressing the <span class="marked">.</span> key while viewing your repo in the browser.


### GitHub Pro for Students
* GitHub offers a <span class="marked">free Pro account</span> to students, as part of the GitHub Student Developer Pack. This gives you access to a bunch of useful features, like <span class="marked">GitHub CoPilot</span> and <span class="marked">GitHub Codespaces</span>.
* You can apply for the pack at https://education.github.com/pack. They ask for some <span class="marked">proof of enrollment</span>, e.g., a student ID card etc., that you submit each year. I strongly recommend applying for it. It barely takes 5 minutes.
* <span class="marked">GitHub CoPilot</span> is an <span class="marked">AI pair programmer</span> that helps you write code. Think of it like ChatGPT but with access to your codebase. So, you can ask it to comment your code, write test cases or to explain an error or a piece of code to you, among other things. You can read more about GitHub CoPilot [here](https://copilot.github.com/).
* <span class="marked">GitHub Codespaces</span> lets you run your code <span class="marked">in the browser</span>, without having to install anything else on your computer. It is similar to Google Colab, but for GitHub repos. You can also use it to <span class="marked">collaborate</span> with others on your codebase. You can read more about GitHub Codespaces [here](https://github.com/features/codespaces).


### `git config`
* `git config` lets you <span class="marked">configure</span> your git installation.
* `git config --global` lets you configure your git installation <span class="marked">globally</span>, i.e., for all repos on your computer. Without the `--global` flag, the configuration is <span class="marked">local</span>, i.e., for the current repo only.
* `git config user.name "<name>"` sets your <span class="marked">name</span> for git commits (<span class="tip-text">local repo</span>).
* `git config user.email "<email>"` sets your <span class="marked">email</span> for git commits (<span class="tip-text">local repo</span>).
* `git config core.editor "<editor>"` sets your <span class="marked">editor</span> for git commits (<span class="tip-text">local repo</span>).
* `git config --global credential.store "<helper>"` sets your <span class="marked">password / credential store</span> for git.
* `git config --global --list` shows you the <span class="marked">global</span> git configuration.
* `git config --list` shows you the <span class="marked">local</span> git configuration.
* `git config --get <item>` shows you the value of the given <span class="marked">item</span> in the local git configuration, e.g., `git config --get remote.origin.url`.


### `git submodule`
* `git submodule` lets you <span class="marked">include</span> another repo as a <span class="marked">submodule</span> in your repo.
* This is useful when you want to <span class="marked">reuse</span> code from another repo, e.g., a library, a framework, etc.
* `git submodule add <url>` adds the repo at the given URL as a submodule to your repo.
* `git submodule update --init --recursive` updates the submodules in your repo.
* `git submodule foreach git pull origin main` pulls the latest changes from the remote repos to the submodule.


### `git <subcommand> --dry-run`
* `git <subcommand> --dry-run` is a <span class="marked">dry run</span> of a git command. It shows you what the command would do, without actually doing it.
* For example, `git add --dry-run` shows you all the files that would be added to the staging area, without actually adding them.
* Note that `git --dry-run` is not a git subcommand. It is an option that you can append to any git subcommand, e.g., `git add --dry-run`.
* Also, it is <span class="marked">not a universal option</span>. It is only available for some git subcommands, e.g., `git add`, `git rm`, `git commit`, etc.
* Other subcommands have their own dry run analogues, e.g., `git merge --no-commit --no-ff`, etc.


## References
* Thorough Git tutorial: https://www.atlassian.com/git/tutorials/setting-up-a-repository
* GitHub walkthrough: https://docs.github.com/en/get-started/quickstart/hello-world
