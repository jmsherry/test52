# Git Exercises

## Making a git repository from scratch
1. Do `git status` on your terminal (with the pointer in this directory). You should get 'Fatal: Not a git repository'
2. Do `git init`
3. Now do `git status` again
4. Delete the `.git` folder (`rm -rf .git`) and then repeat steps 1-3

You now have an empty git repository

## Adding files
1. Create a file called `index.html` and add some HTML inside it.
2. Do `git status`, you'll see that it lists `index.html` in 'untracked files'
3. Do `git add index.html`, then `git status`. The file is now 'staged for commit'
4. Do `git log`. This gets the log of what's in the repository. `index.html` is not in there. That's because you've *staged* it for commit, but you've not actually committed it. 

To put it another way, you've told git that this file is part of a group of files in your working directory that you're getting ready to add to the repository - you're just 'lining them up first' before you do so.

## Committing files to the local repository

1. Do `git commit` and hit return. You will be thrown into vim, because git needs a 'commit message' to add them with. Press `i` to go into 'insert mode' and type `initial html file`, then press `esc`, then type `:wq` to save and quit vim
2. Now do `git log`. You can see that the index.html is in there.
3. Make a small change to the html file and do `git status` again. You should see `index.html` in 'modified files'
4. This time we'll add it, then commit the shorter way `git commit -m "added something"` (**N.B. Your commit messages MUST be meaningful IRL. Your colleagues would KILL YOU for a message like that!**)
5. Do `git log` to see that both changes are now in the log for the repo

## Seeing the difference between what's in your repo and what's in your working directory (i.e what's in your current files vs what you've got saved)

1. Make some changes to the `index.html` and save them.
2. Do `git diff`. What you see is a 'diff report', showing the differences and the line numbers on which they occur. You can get git to do this on all files
3. Create a directory called `styles` and do `git diff`, `git log`, and `git status`. The directory does not show because **git tracks files, not directories**
4. put an empty file inside it called `.gitkeep`
5. Now do `git status` again, and the directory should be visible
6. Add a `styles.css` file with some random CSS in.
7. Do `git add styles` (this will add the contents of the directory)
8. Do `git status` again and then commit the styles directory.
9. Make another change to the `styles.css` and then do `git diff`; you'll see that it will diff both the HTML and the CSS file

## Using a remote repository

1. `git remote` lists the remote repos that this repo uses. ATM, it should return nothing...
2. On github, create a new repository. (look at the second set of instructions for an existing repo)
3. `git remote add origin git@github.com:jmsherry/test52.git` (or `git remote add <your made-up name for that remote repo> <git protocol address of that remote repo>`)
4. Do `git remote` again, to see that it has taken
5. Now to push your changes to that repo. `git push -u origin master` (`-u` sets that repo as 'upstream' and tracks the differences between your local repo and it)
6. Look on GitHub to see the files are there
7. Do `git status`. At the top it now tells you the differences between your local and remote. (These don't auto-update. See `git fetch` later...)


# END OF BASIC GIT CYCLE
---
## Undoing our mistakes

### Staging mistakes
#### Single file
1. Make some changes to the `index.html` file
2. Do `git add`
3. Do `git status`
4. Now we decide we don't want to add the file to this commit. Do `git reset index.html`
5. Do `git status` again

#### Directory
1. Create a directory called `scripts`
2. Add a `main.js` to it
3. Do `git add scripts`
4. Do `git reset scripts` *test this*

### Commit Mistakes
#### Wrong last message
1. Add the `scripts` directory again
2. Do `git commit -m "rubbish message"`
3. Do `git commit --amend -m "better message"`

#### Removing something accidentally committed
1. Create a directory called `node_modules` with a `.gitkeep` inside it.
2. Often with node_modules people accidentally add them to version control and they're not supposed to be there.
3. Do `git add node_modules` and then `git commit -m "node modules oops"`
4. You could delete the directory and then commit the deletion (which requires moving files), but it's easier to do `git rm -r --cached node_modules`
5. Check the status with `git status`
6. To avoid this happening again, add a `.gitignore` file to your working directory with the text `node_modules/` in it
7. Now do `git status again`. (You'll still have to commit the deletion)

#### Undoing a commit wholesale
1. Make some changes to your files
2. Do `git add .`
3. Do `git commit -m "to be reverted`
4. Now we need to undo this commit, do `git log` to find out the SHA of the commit
5. Do `git revert <SHA>`
6. Do `git log` to see the logs and `git status` to see the current status

#### Undoing parts of a commit
1. TODO

#### Moving up and down the history
1. If you do `git log` you can see the history of the project.
2. Each commit has an SHA number (like a 'commit id').
3. You can move up and down these with `git checkout <SHA>`
4. You may get a message saying that you are in a 'detached head state'. Don't worry. You're in a little side commit. You can either 'branch and save' (see next section) here; or make changes and commit directly, or; just go back to where you were with `git checkout master` or `git checkout HEAD`.

---
## Branching
1. `git status` will tell you that you are on the `master branch`
2. `git branch` will list all the branches, with an asterisk next to the one you are on
3. `git branch myBranch` will create a new branch called `myBranch`
4. If you `git branch` you'll see you're still on master but `myBranch` is listed
5. If you do `git checkout myBranch` and then `git branch`, you'll see that you are now on that branch
6. If you do `git checkout master` and then `git branch` you'll see that you are back on the master branch
7. If you do `git checkout -b myOtherBranch` then `git branch`, you can see that that can be done in one move
8. If you `git checkout master` you'll move back to master.
9. Create a text file `test.txt` but don't add it and do `git status`. You'll see it is untracked on `master`.
10. Do `git checkout myOtherBranch` (no `-b` because you're not creating it again) and do `git status` there. You'll see that the text file is visible there too.
11. Let's clean up a bit and delete those 2 branches and the text file: `git branch -d myBranch myOtherBranch && rm test.txt`
12. `git branch myBranch2` creates a new branch from master
13. Let's add a text file `touch text2.txt && git add . && git commit -m "text file"`
14. Switch to `myBranch2`. Is the text file there?
15. Do `git merge master`. Is it there now?
16. Change the text file and commit it
17. Checkout master and make a different change and commit it
18. Do `git merge myBranch2`. Does the auto-merge fail?
19. Manually select the correct change and commit with the message 'merge'
20. Use `git log --graph` to see how your branches have interplayed
21. On github create a new branch and add a new file to it (make sure you commit the file)
22. Do `git branch` to see local branches; `git branch -r` to see remote branches, and; `git branch -a` to see all
23. To retrieve the remote branch do `git checkout origin/<branchname>`
24. Do `git branch` to ensure you have it

---
## Push, Pull (& Fetch)

1. `git status` will show you that you are a few commits ahead of the remote
2. On github itself, add a file called `README.md`
3. Do `git status` (new file on remote is not acknowledged)
4. Do `git fetch` then `git status` again (now it is)
5. Try to push. You can't. You have to pull first and merge if not possible automatically
6. Do `git pull`
7. Do `git status`. 
8. If you are OK locally then `git push`
