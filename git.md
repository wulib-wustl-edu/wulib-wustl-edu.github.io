---
layout: page
title: Git
permalink: /git/
---


### Git Basic Tutorial
---

Follow instructions: [here](https://help.github.com/articles/set-up-git/)


#### Install Git:
---

- Install on Mac: install XCode Command Line Tools
- Install on Windows: install [Git For Windows](https://gitforwindows.org/)


#### Configure Git on the terminal:
---

    $ git config --global user.name "Bill Bradsky"
    $ git config --global user.email "billbradsky@wustl.edu"
    $ git config --global core.autocrlf input FOR WINDOWS: core.autocrlf true

#### Create a project:
---

    $ mkdir workspace
    $ cd workspace/
    $ mkdir testproject/
    $ cd testproject/
    $ touch README.md
    $ echo "#### Hello World" >> README.md

#### Initialize Git repo in your project
---

    $ cd ~/workspace/testproject/
    $ git status
    fatal: Not a git repository (or any of the parent directories): .git
    $ git init

#### Git basic workflow of adding/committing work to Git
---

    $ cd ~/workspace/testproject/
    $ git status
    On branch master
    No commits yet
    Untracked files:
      (use "git add <file>..." to include in what will be committed)

    	README.md

    nothing added to commit but untracked files present (use "git add" to track)
    $ git add README.md

    $ git status
    On branch master
    No commits yet

    Changes to be committed:
      (use "git rm --cached <file>..." to unstage)

    	new file:   README.md

    $ git commit -m 'Add Readme file' # Make this meaningful and contextual about what you are commiting!
    master (root-commit) 5e2aa6a] Add Readme file
     1 file changed, 1 insertion(+)
     create mode 100644 README.md


#### But what about Github? Gitlab?
---

So your README file (and eventually a lot of other code) is being tracked by git now, but it is still only on your computer and is not shared with others.

Problems:

1. If computer is corrupted, damaged, taken by wolves, or the project folder was simply deleted; the remote repository is a great backup solution for your code
2. Not easy to share your code and collaborate with others; the remote repository and git allow us to easily share our code and collaborate


This is where [Github](https://github.com/), [Gitlab](https://gitlab.com/), [Bitbucket](https://bitbucket.org/), and other remote repository solutions out there.


#### We need to add a remote repository connection and push our code up
---

1. Create Github account if you do not have one
2. Create new repository on Github with name of your project on your computer
3. Follow instructions to add remote to your project on your computer


        $ git remote add origin https://github.com/downwriter/testproject.git/
        $ git remote -v (i.e. show your remotes)

4. Let's push our code to our remote repository (Github for now)

        $ git push -u origin master (this is a standard practice to name initial, and ultimately production, codebase as master)

5. Look at your repository page on Github now. The project from your computer should be pushed up to Github now.

   __NOTE__: The content of your README file should be presented on the repo homepage.


#### OK now what? Branching
---

Master should be a stable "branch" or version of the code. Any changes/edits/development should be performed in another branch and then merged into the master (after testing and/or approval...don't worry we'll get there)

1. Create a feature or development branch

   __NOTE__: By default, the new branch is based upon whatever branch you were on. So, if you are on master when you create a new branch, the branch will start from the master version of the code.


        $ git checkout -b <branch name here>
        Switched to a new branch 'alter_readme'

        $ git status
        On branch alter_readme
        nothing to commit, working tree clean


2. Make a change to your project code on command line OR in your text editor / IDE of choice

        $ echo 'This is additional or an edit to the code' >> README.md
        $ git status
        On branch alter_readme
        Changes not staged for commit:
          (use "git add <file>..." to update what will be committed)
          (use "git checkout -- <file>..." to discard changes in working directory)

                modified:   README.md

        no changes added to commit (use "git add" and/or "git commit -a")

        $ echo 'public shouldn't see this' >> keepithidden.txt (i.e. create another file)
        On branch alter_readme
        Changes not staged for commit:
          (use "git add <file>..." to update what will be committed)
          (use "git checkout -- <file>..." to discard changes in working directory)

                modified:   README.md

        Untracked files:
          (use "git add <file>..." to include in what will be committed)

                keepithidden.txt

        no changes added to commit (use "git add" and/or "git commit -a")

3. You do not have to commit everything all at once. In fact, additions/commits should be "atomic" or granular...focusing on a feature and/or a specific piece of code

   Let's add the README changes that git told us about above:

        $ git add README.md
        $ git status
        On branch alter_readme
        Changes to be committed:
          (use "git reset HEAD <file>..." to unstage)

                modified:   README.md

        Untracked files:
          (use "git add <file>..." to include in what will be committed)

                keepithidden.txt

        $ git commit -m 'added more content to Readme'
        $ git status

    __NOTE__: We added the README file and commited it without adding/tracking the other file

4. We don't want the keepithidden.txt file to be committed, tracked, and then pushed to the remote repo.

   We need to to tell Git to ignore it.

        $ touch .gitignore
        $ echo 'keepithidden.txt' >> .gitignore
        $ git status
        On branch alter_readme
        Untracked files:
          (use "git add <file>..." to include in what will be committed)

                .gitignore

        nothing added to commit but untracked files present (use "git add" to track)

        $ git add .gitignore
        $ git commit -m 'initial commit of gitignore file'
        $ git push origin alter_readme
        To https://github.com/downwriter/testproject.git
         * [new branch]      alter_readme -> alter_readme


5. Ok, we have done work in a branch (made some changes/edits/fixed code), now what?

   We need to merge those changes back into the master branch. Generally, there will be tests and/or reviews by teammates before this occurs.

   Here are the mechanics:

        $ git checkout master
        $ git merge alter_readme
        $ git push origin master
        $ git status
        $ git status
        On branch master
        Your branch is up to date with 'origin/master'.

        nothing to commit, working tree clean

6. In general, you do not want to have a branch sitting around after you make a change (there are exceptions and cases where you would have multiple branches)

   Need to clean up the branch we used to make a change:

        $ git push origin --delete alter_readme (delete remote branch on Github)
        $ git branch -D alter_readme
        error: Cannot delete branch 'alter_readme' checked out at '~/testproject'

        $ git checkout master
        $ git branch -D alter_readme (delete local branch)
        Deleted branch alter_readme (was 652611f).




------------
------------
#### Distributed Version Control

1. Vital component that team needs to adopt
2. Codebase is controlled by versions so that it can be moved back to a previous commit, release, etc.
3. Each team member can have a copy of the codebase to develop features and then merge those features into the master branch of code
4. Git aligns with Agile workflow: each issue or story can have its own branch in Git, it will be tested/reviewed, and then merged into master branch of code


#### Git Tutorials

1. [Git Source Code](https://git-scm.com/)
2. [Try Git](https://try.github.io/levels/1/challenges/1)
3. [Atlassian Guides on Git](https://www.atlassian.com/git/tutorials)
4. [Git Learning Resources](https://help.github.com/articles/git-and-github-learning-resources/)
5. [Codecademy Git](https://www.codecademy.com/learn/learn-git)
6. [Don't Be Afraid To Commit](http://dont-be-afraid-to-commit.readthedocs.io/en/latest/git/commandlinegit.html)


#### MarkDown

1. [MarkDown](https://guides.github.com/features/mastering-markdown/)
2. [Github Markdown](https://help.github.com/categories/writing-on-github/)
3. [Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)


#### Git Workflows

1. [Github Flow](https://guides.github.com/introduction/flow/)
2. [Atlassian Guides on Workflows](https://www.atlassian.com/git/tutorials/comparing-workflows)