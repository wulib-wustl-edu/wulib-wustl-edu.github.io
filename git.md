---
layout: page
title: Git
permalink: /git/
---


## Git Basic Tutorial
---

## Setup:

Please follow these instructions [here](https://help.github.com/articles/set-up-git/)

---

## Resources

### Distributed Version Control: [What is Git?](https://www.atlassian.com/git/tutorials/what-is-git)

1. Each person has a copy of the repository and its history instead of the repostiory and its version history *__ONLY__*   being located centrally like in CVS or SVN.
2. Each team member can have a copy of the codebase to develop features and then merge those features into the master branch of code
3. Git aligns with Agile workflow: each issue or story can have its own branch in Git, it will be tested/reviewed, and then merged into master branch of code
4. Not just for code: it can be used for a variety of other documents in the organization; many use git to manage their documentation


### Git Tutorials

1. [Git Source Code](https://git-scm.com/)
2. [Try Git](https://try.github.io/levels/1/challenges/1)
3. [Atlassian Guides on Git](https://www.atlassian.com/git/tutorials)
4. [Git Learning Resources](https://help.github.com/articles/git-and-github-learning-resources/)
5. [Codecademy Git](https://www.codecademy.com/learn/learn-git)
6. [Don't Be Afraid To Commit](http://dont-be-afraid-to-commit.readthedocs.io/en/latest/git/commandlinegit.html)
7. [Github Tutorial](https://guides.github.com/activities/forking/)
8. [Gitlab Merge Requests](https://docs.gitlab.com/ee/user/project/merge_requests/)


### MarkDown

1. [MarkDown](https://guides.github.com/features/mastering-markdown/)
2. [Github Markdown](https://help.github.com/categories/writing-on-github/)
3. [Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)


### Git Workflows

1. [Github Flow](https://guides.github.com/introduction/flow/)
2. [Atlassian Guides on Workflows](https://www.atlassian.com/git/tutorials/comparing-workflows)


### Install Git:
---

- Install on Mac: install XCode Command Line Tools
- Install on Windows: install [Git For Windows](https://gitforwindows.org/)


### Configure Git on the terminal:
---

    $ git config --global user.name "Bill Brasky"
    $ git config --global user.email "billbrasky@wustl.edu"
    $ git config --global core.autocrlf input FOR WINDOWS: core.autocrlf true

### Create a project:
---

    $ mkdir workspace
    $ cd workspace/
    $ mkdir yourname_testproject/
    $ cd yourname_testproject/
    $ touch README.md
    $ echo "#### Hello World" >> README.md

### Initialize Git repo in your project
---

    $ cd ~/workspace/yourname_testproject/
    $ git status
    fatal: Not a git repository (or any of the parent directories): .git
    $ git init

### Git basic workflow of adding/committing work to Git
---

    $ cd ~/workspace/yourname_testproject/
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


### But what about Github? Gitlab?
---

So your README file (and eventually a lot of other code) is being tracked by git now, but it is still only on your computer and is not shared with others.

Problems:

1. If computer is corrupted, damaged, taken by wolves, or the project folder was simply deleted, your content would be lost; the remote repository is a great backup solution for your code
2. Not easy to share your code and collaborate with others; the remote repository and git allow us to easily share our code and collaborate


This is where [Github](https://github.com/), [Gitlab](https://gitlab.com/), [Bitbucket](https://bitbucket.org/), and other remote repository solutions out there.


### We need to add a remote repository connection and push our code up
---

1. Create Github account if you do not have one
2. Create new repository on Github with name of your project on your computer
3. Follow instructions to add remote to your project on your computer


        $ git remote add origin https://github.com/username/yourname_testproject.git/
        $ git remote -v (i.e. show your remotes)

4. Let's push our code to our remote repository (Github for now)

        $ git push -u origin master (this is a standard practice/convention to name initial, and ultimately production, codebase as master)

5. Look at your repository page on Github now. The project from your computer should be pushed up to Github now.

   __NOTE__: The content of your README file should be presented on the repo homepage.


### OK now what? Branching
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
        To https://github.com/username/yourname_testproject.git
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
        error: Cannot delete branch 'alter_readme' checked out at '~/yourname_testproject'

        $ git checkout master
        $ git branch -D alter_readme (delete local branch)
        Deleted branch alter_readme (was 652611f).


### Cloning

You can clone a repository from Github, Gitlab, or some other repository server out there to your local computer:

1. Find the repository on Github or Gitlab that you want to clone.
2. Look for the Clone or Download button on Github.
3. It will offer you a url in HTTPS (or SSH) for you to copy (CTRL + C or CMD + C on MAC)

        $ cd ~/workspace
        $ git clone <url of the git repo>
        <May Show Prompts for Github login if using HTTPS>


---
---

### SSH Keys

---

__Check to make sure you do not have existing SSH Keys__

---

   1. Open a terminal window and type:

            $ ls -al ~/.ssh

   2. You should see files such as id_rsa.pub, id_rsa, etc.

        1. If you do not see id_rsa.pub, id_rsa, etc., you will need to generate SSH keys. Continue to "Add SSH Keys to Github Account."

---

__Generate SSH Keys__

---

   1. Open terminal window:

            $ ssh-keygen -t rsa -b 4096 -C "youremail@wustl.edu"
            Generating public/private rsa key pair

            Enter a file in which to save the key (/c/Users/you/.ssh/id_rsa):[Press enter]
            Enter passphrase (empty for no passphrase): [Press Enter]
            Enter same passphrase again: [Enter]

---

Add SSH Keys to Github Account

---

1. Copy SSH Key to your clipboard

        $ pbcopy < ~/.ssh/id_rsa.pub

        OR

        $ vi ~/.ssh/id_rsa.pub

        THEN copy the id_rsa.pub manually with your mouse/keyboard

2. Login to your Github Account

3. Click Settings in upper right corner (where your profile is).

4. Click SSH and GPG Keys

4. Click "New SSH Key"

5. Paste your previously copied SSH Key into the appropriate box & provide label


---
---

### Forking Projects

---


![Forking](https://github-images.s3.amazonaws.com/help/bootcamp/Bootcamp-Fork.png "Forking")

#### Why Forking?

Forking allows you to copy a repository. You can experiment, test, customize the code or work without having an impact on the original repository.

The main reason for forking repositories is make changes to the code and propose those changes back to the original repository maintainer. Also, you might

use the forked project as the foundation for your own project (assuming they are cool with it. *__See Licensing__*).

---

As noted, collaboration is a key feature of Git (and Github, Gitlab, Bitbucket, etc.). Forking allows teams to work on the same project.

__Short Version:__ a team member would fork the main project repository, clone the fork to their local computer, make changes, push the changes back up to their fork, and then make a pull request.



#### Getting Started
---

1. Login to your Github Account

2. Find the project of a team member that was created using the steps above in the Git Basic Workflow section

3. Find the "Fork" button on their project page

   __NOTE__: see the example image above. It might take a minute for the fork to complete (depending on size of project)

4. Once the project is forked, you should see it listed on your Github repo page.

   > *__YourGithubAccountName/NameOfProjectYouForked__*tp

   > Forked from *__GithubAccountOfOriginalMaintainer/NameOfProject__*


5. Clone the forked repository to your local machine

        $ cd workspace
        $ git clone git@github.com:yourusernamehere/repoyouforked.git

#### Staying in Sync with the original repository
---

1. Move to the project that you cloned

        $ cd workspace/projectdirectory/

2. Check your remote repository connections

        $ git remote -v
        origin  git://github.com/yourusername/yourforkedproject.git (fetch)
        origin  git://github.com/yourusername/yourforkedproject.git (push)


3. We need to add an "upstream" remote so that we can get the changes from the original project

        $ git remote add upstream git://github.com/originalownerusername/originalrepository.git
        origin  git://github.com/yourusername/yourforkedproject.git (fetch)
        origin  git://github.com/yourusername/yourforkedproject.git (push)
        upstream        git@github.com:originalownerusername/originalrepository.git (fetch)
        upstream        git@github.com:originalownerusername/originalrepository.git (push)


4. Make sure your local repo is in a clean state (nothing to add or commit)

        $ git status


5. Now, download any updates from the upstream repository (if any)

        $ git fetch upstream

6. Checkout your fork's local (i.e. what's on your computer) master branch

        $ git checkout master

7. Merge the changes you fetched using the git fetch command above

        $ git merge upstream/master

   __NOTE__: This syncs your fork with the original repository (i.e. the upstream) AND you do not lose any local changes you made

   __ALSO__: This only updates your working project on your local machine. You need to push your changes to your fork.

---
---

#### Exercise: Making a Pull Request With Your Fork
---

1. Make a change or changes to your project on your computer

2. Use the Basic Git Workflow (see above) to add, commit, and push those changes to your *__remote fork__* on Github

3. Navigate to the original repo that you forked from on Github

4. Click the "New Pull Request" Button

![New Pull Request](https://help.github.com/assets/images/help/pull_requests/pull-request-start-review-button.png)


5. On left side of the compare, choose the branch you want your changes merged into (probably master in this case)

6. On the right side of the compare, choose the branch from your fork that you want merged into the original repo (probably master in this case)

7. Add title and description information to your pull request (something meaningful related to the work you did in your fork)


---
---

#### Git Fetch vs. Git Pull

---

> Both download or get data from the remote repo. It is important that you do this daily (i.e. stay in sync with the remote repo by downloading any changes)

> git fetch: this only downloads the data but it does not integrate the changes into your working project. Don't be afraid to to do a git fetch regularly.

       $ git fetch <remote repo>

> git pull: this downloads the data but it also integrates (or merges) the remote repo data into your working project.

        $ git pull <remote repo> <remote branch>

Example:

        $ git pull origin master

  __NOTE__: Make sure your local copy is clean. A "git status" command should say: "nothing to commit, working tree clean." Make sure you have nothing on your local repl that needs to be added/committed


#### Merge Conflicts

---

__IT'S TIME TO FREAK OUT THE WORLD IS ENDING.__ __KIDDING.__

Remember, the nice thing about Git is that you can't break things (you might break your pride from time to time).

Any merge conflicts that occurs can be resolved. Also note, any merge conflict that occurs is only happening to you/your local git repo.

The original project or branch is not impacted.

How do they occur?

A merge is an integration of another branch into your current branch.

A merge conflict occurs when two people make changes to the same line(s) in the same file(s). Or, when one person deletes a file from the codebase
and another person only modified it. Git does not know how to handle these situations. Normally, with a merge, Git knows where to put the code changes
that are made as part of an integration (i.e. via a git merge command).


#### Handling Merge Conflicts...

---

Merge Conflict Example:


        $ mkdir merge_conflict_example
        $ cd merge_conflict_example/

        merge_conflict_example$ git init
        Initialized empty Git repository in /Users/philsuda/workspace/merge_conflict_example/.git/

        merge_conflict_example$ touch my_code.sh

        merge_conflict_example$ git add my_code.sh

        merge_conflict_example$ echo "echo Hello" > my_code.sh

        merge_conflict_example$ git commit -am 'initial commit'
        [master (root-commit) b731ab0] initial commit
         1 file changed, 1 insertion(+)
         create mode 100644 my_code.sh

        merge_conflict_example$ git checkout -b new_branch
        Switched to a new branch 'new_branch'
        merge_conflict_example$ echo "echo \"Hello World\"" > my_code.sh 
        merge_conflict_example$ git commit -am 'first commit on new branch'
        [new_branch a539557] first commit on new branch
         1 file changed, 1 insertion(+), 1 deletion(-)
        merge_conflict_example$ git checkout master
        Switched to branch 'master'

        merge_conflict_example$ echo 'echo \"Hello World\"' > my_code.sh

        merge_conflict_example$ git commit -am 'second commit on master'
        [master 8f55cfe] second commit on master
         1 file changed, 1 insertion(+), 1 deletion(-)

        merge_conflict_example$ git merge new_branch
        Auto-merging my_code.sh
        CONFLICT (content): Merge conflict in my_code.sh
        Automatic merge failed; fix conflicts and then commit the result.


 1. Determine the situation

        merge_conflict_example$ git status
            On branch master
            You have unmerged paths.
              (fix conflicts and run "git commit")
              (use "git merge --abort" to abort the merge)
            
            Unmerged paths:
              (use "git add <file>..." to mark resolution)

              both modified:   my_code.sh

              no changes added to commit (use "git add" and/or "git commit -a")

2. Using your favorite text editor look at the conflicting file or files

    Contents of file: 

            <<<<<<< HEAD
            echo \"Hello World\"
            =======
            echo "Hello World"
            >>>>>>> new_branch

          __NOTE__: Everything above the "=======" is your local/working project code; everything below is the content of the branch you are trying to merge into your code


3. Make a decision and edit the file in your text editor the way you want it to be moving forward

      Contents of file now: 

            echo "Hello World"

4. Save the file

5. Perform a git add 

6. Perform a git commit    


__NOTE__: If all else fails, use this command: 

    $ git merge --abort 

    OR if you already completed the merge and realize you made a mistake

    $ git reset --hard (this will take you back to previous commit so you can start over)

---
---

### Next Steps & Discussion:
---


1. Best Practices & Standards for the WashU Libraries

    Recommendation/Proposal:

    Every project should have:


    - README.md
    - Contributing.md
    - License.md (see [choosealicense.com](https://choosealicense.com))
    - Vagrantfile
    - Tests


    - Every README.md should have:

    - Project Title

    - Basic Description

    - Prerequisites: describe dependencies or other software that needs to be installed before running the application/code

        > Example: Samvera/Hyrax requires ffmpeg, imagemagick, libreoffice, solr, fedora, etc. before it will even run

    - Installation: provide step by step procedure for how to get the application running in a development environment. Also, show how to prove installation is successful

    - Tests: show how to run (automated) tests against the system (see Test-Driven Development)

    - Deployment: document how to deploy the application to a live system

    - Built with: document what the application was built with (i.e. Python, Flask, SQLAlchemy, etc.) as well as any APIs used

    - Contributing: document in the Contributing.md file how people can contribute, process/requirements for pull requests, code of conduct, etc.

    - License: document the License used with a link to the License.md

    - More Info: links to resources, wiki pages, etc.

    - Support: contact info if required/necessitated


    Every Gitlab Project:

    - Connected with Slack Notification Webhook
    - Project should document issues/to-dos/completed on project issue board
    - Gitlab Runner (CI/CD)



2. Workflows & Collaboration


    * Centralized Workflow
    * Feature Branch Workflow
    * GitFlow
    * Forking Workflow


------------
------------