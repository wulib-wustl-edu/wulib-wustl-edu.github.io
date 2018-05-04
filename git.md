---
layout: page
title: Git
permalink: /git/
---


### Git Basic Tutorial

Follow instructions: [here]((https://help.github.com/articles/set-up-git/))


#### Install Git:

1. Download and Install [Git](https://git-scm.com/downloads)
2. Install on Mac: install XCode Command Line Tools
3. Install on Windows: install [Git For Windows](https://gitforwindows.org/)

#### Configure username and email for Git on the terminal:

    $ git config --global user.name "Bill Bradsky"
    $ git config --global user.email "billbradsky@wustl.edu"

#### Create a project:

    $ mkdir workspace
    $ cd workspace/
    $ mkdir project/
    $ cd project/
    $ touch README.md
    $ echo "Hello World" >> README.md

#### Initialize Git repo in your project

    $ cd ~/workspace/project/
    $ git init


-----

#### Distributed Version Control

- Vital component that team needs to adopt
- Codebase is controlled by versions so that it can be moved back to a previous commit, release, etc.
- Each team member can have a copy of the codebase to develop features and then merge those features into the master branch of code
- Git aligns with Agile workflow: each issue or story can have its own branch in Git, it will be tested/reviewed, and then merged into master branch of code

Software Possibilities: GitHub, Gitlab, BitBucket

__NOTE__: important to remember that all/many of the software components in dev-ops can be integrated for seamless workflow.


#### Git Tutorials

1. [Git Source Code](https://git-scm.com/)
2. [Try Git](https://try.github.io/levels/1/challenges/1)
3. [Atlassian Guides on Git](https://www.atlassian.com/git/tutorials)
4. [Git Learning Resources](https://help.github.com/articles/git-and-github-learning-resources/)
4. [Codecademy Git](https://www.codecademy.com/learn/learn-git)


#### MarkDown

1. [MarkDown](https://guides.github.com/features/mastering-markdown/)
2. [Github Markdown](https://help.github.com/categories/writing-on-github/)
3. [Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)


#### Git Workflows

1. [Github Flow](https://guides.github.com/introduction/flow/)
2. [Atlassian Guides on Workflows](https://www.atlassian.com/git/tutorials/comparing-workflows)