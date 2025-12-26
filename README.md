# Git

much simpler than some people make it out to be.

## the bottom line
there's really only a few commands you REALLY need to know. but first you need to know somewhat how git works.  

at its simplest, git is version control. a "commit" is essentially when you make a change to your project, and you want to "finalize" that change (note: commits are NOT undoable). a "push" sends that commit from wherever you're working locally to github, so you can see the new version on the website. a "pull" simply grabs the latest version of your project from github and overwrites whatever you have locally.  

## the essentials
like i said, there are really only a tiny few commands you NEED to know:
- `git clone [repository]`. this is when a project exists on github and you want to copy it to your local machine. there are two main ways to clone:
    - **over https**: `git clone https://github.com/[OWNER]/[REPOSITORY].git`
    - **over ssh (recommended)**: `git clone git@github.com:[OWNER]/[REPOSITORY].git`
i recommend ssh because it's more secure and also slightly faster, but you will also have to set up ssh keys for this. ultimately, https vs ssh really doesn't have much of a difference.  
- `git status`: this command tells you what stuff has been changed. it's super useful to help you decide what you want to commit. new files that you created will be labeled "Unmarked", while existing files that you changed will be marked as "Modified". 
- `git add [. / -A]`: this stages changes for a commit. in other words, when you make changes to a file, calling `git add` will add those changes to your next commit. `git add .` stages all changes in the *current working directory and all subdirectories*, while `git add -A` stages *all changes in the repository*. for simplicity, i recommend using `git add .` and making sure you know what your current working directory is at all times. tip: use `pwd` to print your working directory.
- `git commit -m "message"`: this commits all your staged changes with a *nonoptional* commit message. you must specify your commit message in quotes after `-m`. 
- `git push [origin branch-name]`: this pushes your recent commit(s) to the github repository. it'll push any commits that have not already been pushed. the first time you push from a specific branch, you will need to specify the branch you want to push to, which is where you will specify the branch name. example: if i want to push to a branch called "feature2", the command will be `git push origin feature2`. more on branches later!
- `git pull [origin branch-name]`: the opposite of push. this command pulls the latest version of the project from github and updates your local files to reflect that. like with pushes, you may need to specify branch name. however, unlike pushes, this is only if you want to pull from a different branch. if you just want to pull from the same branch, `git pull` will suffice.
- `git checkout [-b] branch-name`: this is the go-to command for handling branches. a branch in git is basically like its own train of thought. you can think of a project like a tree, where multiple collaborators can "branch" off in their own directions (hence the name branch). working on branches is important so that different collaborators can isolate their work from others, so that there are no conflicts. when you create a branch, you start where you currently are and create a new path off of the main branch, where you can work on what you want. to create a new branch called feature3, use `git checkout -b feature3`. if feature3 already exists, you can switch to it using `git checkout feature3`. the `-b` flag indicates that you're creating a new branch.

that's it! everything else with git is a nice to have. so now let's see how to actually do these in practice. 

## testing it out on this repository
1. we first need to clone the repository. for simplicity let's assume you don't have ssh keys set up, so we can use https. to clone this repository: `git clone https://github.com/adarshm11/git-tutorial.git`
2. once we have cloned, we can open up the repo in our vs code and open `main.js`. there we'll see a simple program that loops five times and prints hello + the iteration number five times. we want to make a change to this, but it's probably best to do this on our own branch. to create a new branch called "fix-loop": `git checkout -b fix-loop`. 
3. now that we're on our own branch, we can make our change. let's modify the loop to iterate 10 times instead of 5 by changing the while loop condition to be `while (i < 10)`. 
4. now that we made a change, we need to commit it. to make sure that our change was saved, let's run `git status`. we should see that `main.js` shows up as a changed file, since we just made the change to the while loop condition. if it doesn't show up, make sure you saved `main.js` after making the change.
5. now we need to stage `main.js` for a commit. run `git add .` to stage it.
6. after staging the change, let's run `git status` again to make sure our change was successfully staged. `main.js` should now show up under the label "Changes to be committed". 
7. now we're ready to commit! let's make our commit message insightful: `git commit -m "modify loop to run 10 times"`. 
8. everything's ready to go, so we can push now. since this is a new branch, running `git push` won't work; we'll have to specify that we're creating a new branch on github. this is simple, we can just use the following command: `git push -u fix-loop`. the `-u` flag specifies that we're creating a new upstream branch (upstream meaning on github). 
9. the final thing we can try is pulling our changes to a different branch. let's go back to the main branch: `git checkout main`. now we can pull our changes from the other branch: `git pull origin fix-loop`. we should now see that our code shows the updated loop with 10 iterations, even though we're on the main branch. 

and that's all!

## nice to haves
- `git stash`: stash is a way to hold onto changes that you don't want to commit just yet. it's useful when you're working on a branch but want to put your work on hold and go to a different branch to check something else, as switching branches required that there are no changes currently. to list all your stashes, you can use `git stash list`. to retrieve a stash, use `git stash pop [stash number]`. 
- `git rebase [base branch name]`: this is huge for dealing with PRs. when you want to merge a branch into another branch, you need to make sure that your current branch has the target branch as a "base". in other words, you need to make sure that your branch only has the changes you want to make and not any extraneous stuff. rebasing allows you to update your current branch with any changes the base branch might have had, so that you don't risk some kind of version mismatching between the two branches.
- `git fetch [--all]`: fetching simply retrieves all new changes that are on the github repo that you might not have locally. it makes sure that your local code is up to date with whatever's on github.
- `git init`: this initializes a git repository locally. sometimes you might want to start a project from your local machine (instead of creating a repository on github then cloning). in that case, you can run `git init` to initialize a repository locally first, which you can later push to github. 