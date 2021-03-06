
# Table of Contents

-   [What is git and why should we use it?](#org3d92788)
    -   [Motivation: Checkpoints](#orgc166f83)
-   [What is the difference between git and GitHub?](#org7681db1)
-   [Anatomy of a git repo](#org9b3ad4c)
-   [Basic Operations](#orgd953759)
    -   [Copying a repo - `git clone`](#orgb4156cc)
    -   [Creating a repo - `git init`](#org7e69bc0)
    -   [Staging - `git add`](#orgd66047e)
    -   [Commit - `git commit`](#orgb23206e)
    -   [Receive - `git pull`](#org47b9da5)
        -   [Conflicts](#org4f92a93)
    -   [Send - `git push`](#org9a4f127)
        -   [Conflicts](#orga3215a5)
-   [Branching and Merging](#orgb60869c)
    -   [Creating branches - `git branch`](#org94780c4)
    -   [A sample branching paradigm](#org48f9d18)
    -   [Conflicts!](#orgbcc438d)
    -   [Pull Requests](#org9365758)
-   [Undoing things](#orgabe0f1b)
    -   [Temporary - `git stash`](#org3938557)
    -   [Exploratory - `git checkout`](#org6682316)
    -   [UNDO! UNDO!! - `git revert`](#org3d872a9)
-   [Hints and FAQs](#orgb9f044e)
    -   [Some strategies to avoid panic.](#orgc1de766)
        -   [Commit messages](#orgcff1e5e)
        -   [Work on your own branch](#orga7c3968)
        -   [Test before you merge](#org80d7f1e)
        -   [Avoiding Conflicts](#org756dd05)
    -   [Help, I accidentally added or committed something I shouldn&rsquo;t have!](#orgbf76149)
    -   [Help, I&rsquo;ve made some changes but I want to move them out of the way!](#org981d37f)
    -   [Next Time](#orgaa56941)
    -   [Comics](#org6dca81a)



<a id="org3d92788"></a>

# What is git and why should we use it?

<div class="org-center">
<p>
<img class='invertible' src='https://imgs.xkcd.com/comics/git.png' title="If that doesn't fix it, git.txt contains the phone number of a friend of mine who understands git. Just wait through a few minutes of 'It's really pretty simple, just think of branches as...' and eventually you'll learn the commands that will fix everything." alt='Git'/>
</p>
</div>

When you&rsquo;re working on a file or a document, it is important to save frequently in order to make sure you don&rsquo;t lose your work. But code is complicated. The development of code is not linear - often edits are exploratory and end up being reverted. You find quickly that what you want to be able to save and explore is not the actual state of your files, but the sequence of edits you&rsquo;ve made. `git` saves your project as a sequence of edits to your code base rather than a set of file contents.


<a id="orgc166f83"></a>

## Motivation: Checkpoints

You have been writing code for hours. You&rsquo;ve got into a good flow. What started off as a few functions in a single file has expanded into dozens of functions and classes in half a dozen files each consisting of several hundred lines of code. At least. You save the latest line you wrote. After The Great Cherrry Coke Debacle in freshman year where you lost two pages of your final paper hours before the deadline, you&rsquo;re very diligent about saving. But it has started to occur to you - not all saves are created equal. Saving code might have one of several purposes:

-   To ensure that you don&rsquo;t lose what you&rsquo;ve done.
-   To compile and run your code as a sanity check.
-   To mark a checkpoint in your progress - a version of your code to backup and come back to if things break.

In a smaller, simpler project, you might have just copied the code into a different folder now and then. But what if you want to go back several checkpoints? or what if you forget? What if you want to copy something from the checkpoint you made three hours ago? What if you forgot to copy the checkpoint? Preparing for all of these eventualities leads to more and more complicated practices of saving and backing up your code. Eventually, you might find that you are annotating your checkpoints with little messages describing what exactly you&rsquo;ve done since the last one.


<a id="org7681db1"></a>

# What is the difference between git and GitHub?

While `git` is the underlying software that performs version control, [Github](https:github.com) is a website where users can host and collaborate on their repositories. Github provides many additional features such as *issues* for repositories where users can file bug reports or make feature requests, *code reviews* where users can provide feedback on each others code, and *project boards* which can help teams of developers to coordinate their work.  Github also provides a framework to give developers easier access to various tools such as *continuous integration servers* and app hosting.


<a id="org9b3ad4c"></a>

# Anatomy of a git repo

A git repository, or repo for short, is just a normal folder with a `.git` folder containing metadata. The `.git` folder will contain information about previous commits, branches, and where the remote repos are located. Most repos will also contain one or more files called `.gitignore`. A `.gitignore` file tells git which files to avoid adding to the repository to prevent mistakes. You might add things like `*.class` files in *Java* or `*.o` files for *C* projects to your `.gitignore` file. Repos hosted on Github might also have a `.github` folder to store configuration of Github specific features.

If you open a repo folder in a file browser, you might not see some of these files or folders because the prefix `.` on a filename is an instruction to many operating systems to keep a file hidden. Many developer tools use *&ldquo;dotfiles&rdquo;* to store user settings invisibly. Most file explorers have options to make all files, including dotfiles, visible.

Conceptually, there are three parts to the git repo. The *working directory* are the current state of the files in your repository. The *staging index* stores the the current *uncommitted* changes to the repo. Finally, the *commit history* stores all the incremental updates to the repository.

If your working on a team, each person will have their own copy of the repo. This ensures that everyone&rsquo;s repo has the same *commit history*. The commit you are currently viewing is known as the `HEAD`.


<a id="orgd953759"></a>

# Basic Operations

In this section, we will outline the basic operations for interacting with git repos


<a id="orgb4156cc"></a>

## Copying a repo - `git clone`

In order to use an existing repo, you must first *clone* it to your local machine. The `git clone` command takes the location of a repo and copies it to your local directory along with all the repo&rsquo;s history. The location provided to the *clone* command can either be an url for a remote repository or a file path, if you want to make a copy of a repository already on your computer. The cloned repo will automatically store the *source* repo as the *origin*, so you can send back your changes.


<a id="org7e69bc0"></a>

## Creating a repo - `git init`

To create a repo, you would perform a `git init`. This creates a `.git` folder inside the base directory of your repo with an empty history. At this stage, the repo considers itself to be empty. Even if there are files already in your directory, they are not yet being *tracked*. A repo created like this will have no remote target<sup><a id="fnr.1" class="footref" href="#fn.1">1</a></sup>.


<a id="orgd66047e"></a>

## Staging - `git add`

Once you&rsquo;ve created or cloned your repo and edited some files, you will want to add your changes to the repo&rsquo;s history. This is a two step process. The first step is to *stage* your changes. You can choose which files<sup><a id="fnr.2" class="footref" href="#fn.2">2</a></sup> you want to add to your repo. During this process, you can stage or unstage changes as you wish - nothing gets saved permanently until the next step.


<a id="orgb23206e"></a>

## Commit - `git commit`

To make your changes (semi) permanent, you make a commit. As well as staged changes, a commit requires a commit message describing its contents. It is important to leave descriptive commit messages so you and your collaborators can more easily go back and identify what happened if something goes wrong. Since commits are a kind of &rsquo;unit&rsquo; in git repo, you may want to split a single session of coding into multiple commits. Just as with saving, it is good practice to commit often<sup><a id="fnr.3" class="footref" href="#fn.3">3</a></sup>. The commit gets assigned a unique reference which can be used in other commands.


<a id="org47b9da5"></a>

## Receive - `git pull`

In order to get updates that have been sent by others (or by you from other machines), you would perform a `git pull`. This gathers all the commits that have been sent to the remote repository since your last pull and sends them to you.


<a id="org4f92a93"></a>

### Conflicts

In some cases, the commits on the remote server may overlap with your own changes. If this is the case, you&rsquo;ll have to perform a `merge`. Merges can be stresfull and we&rsquo;ll talk about them in more detail a little later. We will also discuss some strategies to avoid unnecessary merging.


<a id="org9a4f127"></a>

## Send - `git push`

Once you&rsquo;ve made some commits, you&rsquo;ll want to synchronize them with your remote repo. In order to do this, you&rsquo;ll need to perform a `git push`. Pushing sends your revisions to the remote repository and makes them available for other users to add.


<a id="orga3215a5"></a>

### Conflicts

Sometimes as you try to push, your remote repository will notify you that it has received a push from someone else since you last pulled from the repo. In situations like this, the remote repository will ask you to perform a `git pull` first. You might see an error like this:

     ! [rejected]        master -> master (fetch first)
    error: failed to push some refs to 'https://github.com/mrmechko/repo.git'
    hint: Updates were rejected because the remote contains work that you do
    hint: not have locally. This is usually caused by another repository pushing
    hint: to the same ref. You may want to first integrate the remote changes
    hint: (e.g., 'git pull ...') before pushing again.
    hint: See the 'Note about fast-forwards' in 'git push --help' for details.

This is a safety measure. This way, if there are any issues surrounding integrating your changes with your collaborator&rsquo;s changes, you can resolve them yourself. We will discuss how to handle conflicts below.


<a id="orgb60869c"></a>

# Branching and Merging

<div class="org-center">
<p>
<img class='invertible' src='https://imgs.xkcd.com/comics/algorithms.png' title="There was a schism in 2007, when a sect advocating OpenOffice created a fork of Sunday.xlsx and maintained it independently for several months. The efforts to reconcile the conflicting schedules led to the reinvention, within the cells of the spreadsheet, of modern version control." alt='Algorithms'/>
</p>
</div>

A key features of git repos is the ability to create a branch of your code. Branches allow you switch between and develop multiple versions of your code simultaneously. If we decide that we want to combine two branches, we can perform a `git merge`. Merging adds all the commits from one branch to another and then a special merge commit to indicate that the two branches have been combined. This special merge commit can be used to resolve any conflicts that might have arisen. We will talk about merge conflicts a little later.

The most common use for branches would be developing a new feature for piece of software with an existing stable version. You don&rsquo;t want to add unstable code to the version that other people might be using, so you can make a branch to work on your feature until it is ready. As mentioned previously, a git repo stores the history as a sequence of *changes* to repo, not as files. That means in many cases changes made to code by different people can be automatically combined by git. This does *NOT* mean that the combined files will work!


<a id="org94780c4"></a>

## Creating branches - `git branch`

`git branch` allows you to create a new branch from the current commit. This does not create any changes in the repository&rsquo;s history. Instead, you just create a new `HEAD` pointer at the existing commit. Once you&rsquo;ve created the new branch, you will need to perform a `git checkout -b` to make sure any future commits get added to your newly created branch.


<a id="org48f9d18"></a>

## A sample branching paradigm

Most repositories have a single branch to start with, called `main`<sup><a id="fnr.4" class="footref" href="#fn.4">4</a></sup>. This branch should contain the most recent &rsquo;released&rsquo; version of your software. In early the early stages of development, of course, it might not be a finished product. Next, you might create a branch called `develop` to track less stable improvements to the software. Individual features will spawn their own branches off of `develop` and be merged back into `develop` as they are completed. When the team is sufficiently sure that the code on `develop` is ready for release, it can be pushed to `main`.

In order to make the most of this paradigm for branching, it is a good idea to set *milestones* which lists a set of features and fixes that are required before the next release. Features that do not contribute to the next milestone are left un-merged

> When you perform a `git merge`, you are taking commits from another branch and adding them to the branch you are currently on. For example, if you want the branch `main` to include changes made in `my-cool-feature-branch`, you would have to first checkout `main` and then *merge* `my-cool-feature-branch` into `main`.


<a id="orgbcc438d"></a>

## Conflicts!

<div class="org-center">
<p>
<img class='invertible' src='https://imgs.xkcd.com/comics/computer_problems.png' title="This is how I explain computer problems to my cat. My cat usually seems happier than me." alt='Computer Problems'/>
</p>
</div>

When pulling or merging changes from other branches, from time to time you will come across conflicts. These occur when two commits change the same line in a manner that git is unable to automatically compute. Merge conflicts are one of the most panic-inducing events associated with git. It is important to remember that all your work is safe as long as you&rsquo;ve been committing regularly.

In the event that a segment of code has a conflict during a merge, that segment will be replaced with both versions of the code with in-text annotations to indicate the conflict. Below is an example of a conflict when merging `other-branch` into `main`. Note the line of `=`&rsquo;s separating the two alternatives.

    This line does not have a conflict.
    <<<<<<< main
    this line has a conflict.
    =======
    This line is a conflict.
    >>>>>>> other-branch;

If we were to look at the state of `main` before the merge began, we would see this:

    This line does not have a conflict.
    this line has a conflict.

Similarly, if we were to look at the state of `other-branch` before the merge began, we would see this:

    This line does not have a conflict.
    This line is a conflict.

To resolve the conflict, all you have to do is edit the file(s) in question to remove the lines containing `<<<<<<< main`, `=======`, and `>>>>>>> other-branch`. Naturally, you&rsquo;ll want to also make sure that the state of the file is correct. Often this means deleting one of the alternatives as well. In some cases you might want to keep some or all of both alternatives.

Once you&rsquo;re done with the edits, commit the changes, and you&rsquo;re all done! Remember, there might be more than one conflict in a merge. You can usually find all the conflicts by searching for `<<<<<` in your code base - it is a fairly uncommon string in code!


<a id="org9365758"></a>

## Pull Requests

Pull requests are a special feature provided by some git-hosting services (eg [github](https://github.com), [gitlab](https://gitlab.com), etc) which help with the latent potential chaos surrounding `git merge`. A pull request allows the team to discuss commits that are scheduled to be merged into a branch, test the resulting merged code, and to add additional commits to fix any issues, all before the merge occurs. Teams will often write-protect shared branches like `main` or `development` because introducing errors to those branches can be disasterous to the whole team. In such situations, a pull-request might require the approval of multiple team members or of product managers.


<a id="orgabe0f1b"></a>

# Undoing things

A very important part of git is the ability to undo commits that introduce errors or fall down rabbit-holes.


<a id="org3938557"></a>

## Temporary - `git stash`

`git stash` is a very useful command which takes any *uncommitted* changes you&rsquo;ve made to the working directory and stores them safely. You can later `git stash pop` to return the changes themselves to the working directory. Note that if you move some changes to your `stash` and then make conflicting changes, then you will have to resolve the conflicts. You can perform any number of other actions between `git stash` and `git stash pop`.


<a id="org6682316"></a>

## Exploratory - `git checkout`

A `git checkout` takes a commit reference and sets your *working directory* state to that commit<sup><a id="fnr.5" class="footref" href="#fn.5">5</a></sup>. `git checkout -b` can be used to switch to a different branch.


<a id="org3d872a9"></a>

## UNDO! UNDO!! - `git revert`

Since each commit in git is a &rsquo;change&rsquo;, the natural way to undo things is to make the opposite change. `git revert` will construct the inverse of a specific revision and commit it. This is great for two reasons. First, you haven&rsquo;t altered the *commit history* - just added to it. Second, you can target a specific commit from several days ago without undoing all the work you&rsquo;ve done since!


<a id="orgb9f044e"></a>

# Hints and FAQs


<a id="orgc1de766"></a>

## Some strategies to avoid panic.


<a id="orgcff1e5e"></a>

### Commit messages

<div class="org-center">
<p>
<img class='invertible' src='https://imgs.xkcd.com/comics/git_commit.png' title="Merge branch 'asdfasjkfdlas/alkdjf' into sdkjfls-final" alt='Git Commit'/>
</p>
</div>

Take the extra few minutes to write a complete commit message. Your teammates and future self will thank you.


<a id="orga7c3968"></a>

### Work on your own branch

Merging your code with someone else&rsquo;s code is one of the more stressful processes when it comes to working in a team. While it is important to make sure your changes remain compatible with your teammates&rsquo; changes, you might want to avoid integrating partially completed features. By working on separate branches, you avoid issues surrounding merging code until actually necessary. Many teams will perform integration tests on a regular basis to ensure that teammates don&rsquo;t diverge too far. You will still have to perform the merges, but at least you can avoid unexpected merge conflicts and bugs that manifest from unexpected combinations of code.


<a id="org80d7f1e"></a>

### Test before you merge

Having a number of commits between working versions of code is not usually a problem, however, it is a good idea to test your code thoroughly before pushing or merging on a shared branch.


<a id="org756dd05"></a>

### Avoiding Conflicts

The best way to deal with conflicts is to have as few of them as possible. Working on separate branches reduces the frequency with which you&rsquo;ll have to deal with conflicts, but that might open the door to much larger conflicts cropping up down the line. Conflicts will usually only occur when two people are editing the same segment of code. If this is happening, it probably means that two people are working on very closely related features! The best way to avoid conflicts in this case is to communicate with your team before hand and let everyone know what you&rsquo;re working on and collaborate proactively instead of reactively.


<a id="orgbf76149"></a>

## Help, I accidentally added or committed something I shouldn&rsquo;t have!

`git rm` will untrack a file that you have committed to the repository by accident. If you don&rsquo;t want to delete the file altogether, you can use the `--cached` option


<a id="org981d37f"></a>

## Help, I&rsquo;ve made some changes but I want to move them out of the way!

`git stash`!


<a id="orgaa56941"></a>

## Next Time

In the next article, we will discuss some of the additional features GitHub brings to git. Pull requests, issues, project boards, and continuous integration are great tools that allow teams to collaborate more efficiently and painlessly.


<a id="org6dca81a"></a>

## Comics

All comics were taken from the fabulous [XKCD](https://xkcd.com).


# Footnotes

<sup><a id="fn.1" href="#fnr.1">1</a></sup> You can manually set a remote target using `git remote add [name] [url]`

<sup><a id="fn.2" href="#fnr.2">2</a></sup> You can even specify *hunks* or specific lines of files you want to add.

<sup><a id="fn.3" href="#fnr.3">3</a></sup> Sometimes it might feel like committing too often will make the history look messy. There are advanced commands such as `git rebase -i` which allow you to rewrite the history. This is a dangerous operation and can break things for other people. There are very few ways to completely destroy a git repo and misuse of history rewriting is basically it.

<sup><a id="fn.4" href="#fnr.4">4</a></sup> The default branch on many repos you see will be called `master`. In a more recent attempt to avoid slave/master metaphors in computer science, Github and many others have encouraged people to use the name `main` instead.

<sup><a id="fn.5" href="#fnr.5">5</a></sup> If you checkout a specific commit instead of a branch, you might get a scary looking message informing you that you are in a `detached HEAD state`. [This](https://www.cloudbees.com/blog/git-detached-head) is a good guide explaining how to handle this.
