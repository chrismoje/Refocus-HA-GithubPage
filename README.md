Learnings from Github Features:

Git Clone
The git clone command is used to create a copy of a specific repository or branch within a repository.

Git is a distributed version control system. Maximize the advantages of a full repository on your own machine by cloning.

What Does git clone Do?
git clone https://github.com/github/training-kit.git
When you clone a repository, you don't get one file, like you may in other centralized version control systems. By cloning with Git, you get the entire repository - all files, all branches, and all commits.

Cloning a repository is typically only done once, at the beginning of your interaction with a project. Once a repository already exists on a remote, like on GitHub, then you would clone that repository so you could interact with it locally. Once you have cloned a repository, you won't need to clone it again to do regular development.

The ability to work with the entire repository means that all developers can work more freely. Without being limited by which files you can work on, you can work on a feature branch to make changes safely. Then, you can:

later use git push to share your branch with the remote repository
open a pull request to compare the changes with your collaborators
test and deploy as needed from the branch
merge into the main branch.

How to Use git clone
Common usages and options for git clone
git clone [url]: Clone (download) a repository that already exists on GitHub, including all of the files, branches, and commits.
git clone --mirror: Clone a repository but without the ability to edit any of the files. This includes the refs, or branches. You may want to use this if you are trying to create a secondary copy of a repository on a separate remote and you want to match all of the branches. This may occur during configuration using a new remote for your Git hosting, or when using Git during automated testing.
git clone --single-branch: Clone only a single branch
git clone --sparse: Instead of populating the working directory with all of the files in the current commit recursively, only populate the files present in the root directory. This could help with performance when cloning large repositories with many directories and sub-directories.
`git clone --recurse-submodules[=<pathspec]: After the clone is created, initialize and clone submodules within based on the provided pathspec. This may be a good option if you are cloning a repository that you know to have submodules, and you will be working with those submodules as dependencies in your local development.
You can see all of the many options with git clone in git-scm's documentation.

Examples of git clone
git clone [url]
The most common usage of cloning is to simply clone a repository. This is only done once, when you begin working on a project, and would follow the syntax of git clone [url].

git clone A Branch
git clone --single-branch: By default, git clone will create remote tracking branches for all of the branches currently present in the remote which is being cloned. The only local branch that is created is the default branch.

But, maybe for some reason you would like to only get a remote tracking branch for one specific branch, or clone one branch which isn't the default branch. Both of these things happen when you use --single-branch with git clone.

This will create a clone that only has commits included in the current line of history. This means no other branches will be cloned. You can specify a certain branch to clone, but the default branch, usually main, will be selected by default.

To clone one specific branch, use:

git clone [url] --branch [branch] --single-branch

Cloning only one branch does not add any benefits unless the repository is very large and contains binary files that slow down the performance of the repository. The recommended solution is to optimize the performance of the repository before relying on single branch cloning strategies.

Git Pull
git pull updates your current local working branch, and all of the remote tracking branches. It's a good idea to run git pull regularly on the branches you are working on locally.

Without git pull, (or the effect of it,) your local branch wouldn't have any of the updates that are present on the remote.

What Does git pull Do?
git pull
git pull is one of the 4 remote operations within Git. Without running git pull, your local repository will never be updated with changes from the remote. git pull should be used every day you interact with a repository with a remote, at the minimum. That's why git pull is one of the most used Git commands.

git pull and git fetch
git pull, a combination of git fetch + git merge, updates some parts of your local repository with changes from the remote repository. To understand what is and isn't affected by git pull, you need to first understand the concept of remote tracking branches. When you clone a repository, you clone one working branch, main, and all of the remote tracking branches. git fetch updates the remote tracking branches. git merge will update your current branch with any new commits on the remote tracking branch.

git pull is the most common way to update your repository.

However, you may want to use git fetch instead. One reason to do this may be that you expect conflicts. Conflicts can occur in this way if you have new local commits, and new commits on the remote. Just like a merge conflict that would happen between two different branches, these two different lines of history could contain changes to the same parts of the same file. If you first operate git fetch, the merge won't be initiated, and you won't be prompted to solve the conflict. This gives you the flexibility to resolve the conflict later without the need of network connectivity.

Another reason you may want to run git fetch is to update to all remote tracking branches before losing network connectivity. If you run git fetch, and then later try to run git pull without any network connectivity, the git fetch portion of the git pull operation will fail.

If you do use git fetch instead of git pull, make sure you remember to git merge. Merging the remote tracking branch into your own branch ensures you will be working with any updates or changes.

How to Use git pull
Common usages and options for git pull
git pull: Update your local working branch with commits from the remote, and update all remote tracking branches.
git pull --rebase: Update your local working branch with commits from the remote, but rewrite history so any local commits occur after all new commits coming from the remote, avoiding a merge commit.
git pull --force: This option allows you to force a fetch of a specific remote tracking branch when using the <refspec> option that would otherwise not be fetched due to conflicts. To force Git to overwrite your current branch to match the remote tracking branch, read below about using git reset.
git pull --all: Fetch all remotes - this is handy if you are working on a fork or in another use case with multiple remotes.
You can see all of the many options with git pull in git-scm's documentation.

Examples of git pull
Working on a Branch
If you're already working on a branch, it is a good idea to run git pull before starting work and introducing new commits. Even if you take a small break from development, there's a chance that one of your collaborators has made changes to your branch. This change could even come from updating your branch with new changes from main.

It is always a good idea to run git status - especially before git pull. Changes that are not committed can be overwritten during a git pull. Or, they can block the git merge portion of the git pull from executing. If you have files that are changed, but not committed, and the changes on the remote also change those same parts of the same file, Git must make a choice. Since they are not committed changes, there is no possibility for a merge conflict. Git will either overwrite the changes in your working or staging directories, or the merge will not complete, and you will not be able to include any of the updates from the remote.

If this happens, use git status to identify what changes are causing the problem. Either delete or commit those changes, then git pull or git merge again.

Keep main up to date
Keeping the main branch up to date is generally a good idea.

For example, let's say you have cloned a repository. After you clone, someone merges a branch into main. Then, you'd like to create a new branch to do some work. If you create your branch off of main before operating git pull, your branch will not have the most recent changes. You could accidentally introduce a conflict, or duplicate changes. By running git pull before you create a brach, you can be sure that you will be working with the most recent information.

Undo A git pull
To effectively "undo" a git pull, you cannot undo the git fetch - but you can undo the git merge that changed your local working branch.

To do this, you will need to git reset to the commit you made before you merged. You can find this commit by searching the git reflog. The reflog is a log of every place that HEAD has pointed - every place that you have ever been checked out to. This reflog is only kept for 30 to 90 days, depending on the commit, and is only stored locally. (The reflog is a great reason not to delete a repository if you think you've made a mistake!)

Run git reflog and search for the commit that you would like to return to. Then, run git reset --hard <SHA> to reset HEAD and your current branch to the SHA of the commit from before the merge.

Force git pull to Overwrite Local Files
If you have made commits locally that you regret, you may want your local branch to match the remote branch without saving any of your work. This can be done using git reset. First, make sure you have the most recent copy of that remote tracking branch by fetching.

git fetch <remote> <branch>
ex: git fetch origin main

Then, use git reset --hard to move the HEAD pointer and the current branch pointer to the most recent commit as it exists on that remote tracking branch.

git reset --hard <remote>/<branch>
ex: git reset --hard origin/main

_Note: You can find the remotes with git remote -v, and see all available remote tracking branches with git branch --all.

Git Push
git push
git push uploads all local branch commits to the corresponding remote branch.

What Does git push Do?
git push updates the remote branch with local commits. It is one of the four commands in Git that prompts interaction with the remote repository. You can also think of git push as update or publish.

By default, git push only updates the corresponding branch on the remote. So, if you are checked out to the main branch when you execute git push, then only the main branch will be updated. It's always a good idea to use git status to see what branch you are on before pushing to the remote.

How to Use git push
After you make and commit changes locally, you can share them with the remote repository using git push. Pushing changes to the remote makes your commits accessible to others who you may be collaborating with. This will also update any open pull requests with the branch that you're working on.

As best practice, it's important to run the git pull command before you push any new changes to the remote branch. This will update your local branch with any new changes that may have been pushed to the remote from other contributors. Pulling before you push can reduce the amount of merge conflicts you create on GitHub - allowing you to resolve them locally before pushing your changes to the remote branch.

Common usages and options for git push
git push -f: Force a push that would otherwise be blocked, usually because it will delete or overwrite existing commits (Use with caution!)
git push -u origin [branch]: Useful when pushing a new branch, this creates an upstream tracking branch with a lasting relationship to your local branch
git push --all: Push all branches
git push --tags: Publish tags that aren't yet in the remote repository
You can see all of the options with git push in git-scm's documentation.

Why can't I push?
If you are trying to git push but are running into problems, there are a few common solutions.

Check your branch
Check what branch you are currently on with git status. If you are working on a protected branch, like main, you may be unable to push commits directly to the remote. If this happens to you, it's OK! You can fix this a few ways.

Work was not yet on any branch
Create and checkout to a new branch from your current commit: git checkout -b [branchname]
Then, push the new branch up to the remote: git push -u origin [branchname]
Accidentally committed to the wrong branch
Checkout to the branch that you intended to commit to: git checkout [branchname]
Merge the commits from the branch that you did accidentally commit to: git merge [main]
Push your changes to the remote: git push
Fix the other branch by checking out to that branch, finding what commit it should be pointed to, and using git reset --hard to correct the branch pointer