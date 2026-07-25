[[Git]] 
Git is a distributed version used to track changes overtime. 
## Git Architecture  
Git keeps tracks of the state of the project file system by keeping checksum of files, every time there's a change, the complete changed file is compressed and made to a new git object, however the file with no changes are not objectified since the hash is same. These objects are structured in a tree representing the file system. `Blob objects` → store the actual contents of a file (not the filename, just the content) `Tree objects` → store directory structure (filenames + permissions → blob or tree hashes). `Commit objects` → store metadata (author, message, timestamp, parent commit(s), and a pointer to the root tree hash). 
## Git Workflow 
The git workflow starts with initiating git, this adds git's file system within the projects working directory this .git directory stores all the objects and configuration related to git stages of the project. 
##### Related Commands 
```bash
# Initilise a directory with git repository 
git init
```
### Stages
A git stage can be defined as the current state of the project in respect to tracking with git within the project development lifecycle. Like file changes in terms of structure , data addition , deletion , manipulation , file operations in respect to other files etc. 
##### Related Commands 
```BASH
git
  status # flag get the current stage of the file and repo
    -s | —-short # flag to get the status in simpler form
    -b | -—branch # flag to get the branch information
```
#### Untracked
When a new file is added its changes are not tracked by git unless told specifically, at this stages the file remains in untracked state. Matter fact every changes made to the file are untracked unless notified to git. 
#### Modified / Untracked Changes 
_after file gets tracked_
File is set to be in modified state when there are changes which are not tracked by git (unless explicitly told). Untracked changes after committed stage moves project to modified stage.
##### Related Commands 
```bash 
git
 reset FILE # option to remove tracked chnages 
    HEAD~N # To reset the current state with nth commit, provided after file           name as a argument, else last commit will be taken as argument 
    -—hard COMMIT-HASH # tracked changes are lost completely.
    —-mixed COMMIT-HASH # changes gets untracked but remains.
    —-soft COMMIT-HASH # changes remains in the staging area.
```
#### Tracked / Staged
The file is set to be tracked when git is aware of the changes, at this stage the files can be committed to snapshot the current tracked stage and make them git object. 
##### Related Commands 
```bash 
git
  add FILEPATH # option to track a file / change 
  mv FILES # option rename a file with tracked changes
  rm —cached FILES # option to remove a file with tracked chnages
```

```bash
git 
  stash # Extract all changes and move it to stack
    —-all | -a # flag Stash all the files
    -u | —untracked # flag to Stash untracked files too
    list # flag to list all the stashed changes in the stack
    pop # flag to apply the latest added stash
    apply stash@{N} # option to Apply specific stash at nth index in the stack
    drop stash@{N} # option to remove the nth applied stash
```
#### Committed / Unmodified
When tracked changes are committed the objects are created which gets stored in the git dir follow-ups the object structuring and other follows. Commits over time whenever made gets chained in branch, which is linked list of commits. *more in branching*
##### Related Commands 
```bash
git
  commit # Snapshotting aka creating git object 
    -a FILES # option too for staging and commiting at the same time.
    —-amend | —-no-edit # flag to overwrite last commit 
    -m “DESIRED COMMIT MESSAGE” # option to set the commit messege
    --allow-empty # option for empty commits
```
## Git Branching & Plumbing
A branch is a chain of commits which represent the project changes over time, when the first commit is created a branch is created automatically as same as default name `main` `master` The top of the branch `HEAD` represent the latest commit. From the branch history the commits can be restated at any given point of time.  Commits which are not chained with branch are lone git objects which might be collected by git garbage collector.  
##### Related Commands
```bash
# Git brach commands 
git branch 
  -v # flag to list all the branches present on the system 
  -a # flag to list all the branches available at remote (user specific)
  -D # flag to delete a branch (current must be different) 
  -m $OLD_NAME $NEW_NAME # option to rename a branch 
  
```

```bash 
# branch lifecycle commands 
git checkout $BRANCH_NAME # will switch to provided branch 
  -b # flag to create from current branch and checkout at the same time to new       branch will share the same commit hostory 

```
### Plumbing
There can be multiple branches from or within a single project, a branch forked from a branch share the same history prior, the changes in commit difference can occur either in original branch or forked branch in these cases branches need to be updated from one another, or the new branch for specific work need to be blend back in. 
#### Merge
Merge is the operation when the forked branch with commits merge in the original branch but all the new commits are converged to a single commit.  
##### Related Commands 
```bash 
git 
  merge BRANCH-TO-BE-APPLY-ON BRANCH-TO-BE-APPLIED # Apply the changes of second     argument on the first, it will create a single commit
    --squash # flag to merge commits into a single commit
    -—no-commit # Avoid commit but apply the changes
    -—abort # flag to abort the merging during conflicts
    -—continue # flag to continue applying commits
    -—ff # flag to move the pointer of the branchto-be-merged-upon to the latest       commit of the merging branch
    -m “DESIRED-COMMIT-MESSAGE” # option to set the desired commit message
```
#### Rebase
Rebase is the operation in which forked branch's commits gets rebased as it is onto the original branch i.e the commits of the forked branch gets append on the original branch.  
##### Related Commands
```bash
git 
  rebase BRANCH-TO-BE-APPLY-ON BRANCH-TO-BE-APPLIED # Apply the changes of           second argument on the first, it will add all the commits of the second onto     first creating series of commit.
    --onto BRANCH COMMIT-1 COMMIT-N # option to apply series on changes/commits        onto a branch
    -—abort # flag to abort the rebasing during conflicts
    -—continue # flag to continue applying changes
    --interactive BRANCH BRANCH2 # option to apply chnages intractive manner
	  # pick: Use the commit as-is
	  # reword: Use commit but edit the commit message
	  # edit: Use commit, but stop for amending
	  # squash: Meld current commit into previous one
	  # fixup: Like squash but keeps only the previous commit's log message
	  # exec: Run a command using the shell
	  # drop: Remove the commit
```
#### Cherry pick 
Cherry picking is the method by one one can select commits and add them onto the head of the branch, the commits can be from any branch. 
##### Related Commands 
```bash 
git 
  cherry-pick BRANCH-TO-BE-APPLY-ON COMMIT-1 COMMIT-N # Apply the patches by       picking commits
    -—no-commit # flag to apply the chnages but do not commit
    —-abort # flag to abort the cherry-picking during conflicts
    —-continue # flag to continue applying commits
    -m “DESIRED-COMMIT-MESSAGE” # option to set the desired commit message
```
## Git User Aspects
Since the git is used by the user every commit have a a user identity signature which include username, user email.  
### `User configuration`
##### Related Commands 
```bash 
git config 
  --global # for setting up system wide config / multiple users 
	user.name $NAME
	user.email $EMAIL 
	core.editor $EDITOR
	init.defaultBranch $BRANCH_NAME
  --system # for setting up config for system 
```
##### Related Commands
```bash 
# Creating ssh key for auth @ pushing 
ssk-keygen -t rsa -b 4096 -C "mail"
```
## Git Distributed Aspects 
Git is a distributed system, every system on which git resides can be part of node. Init changes can be `push` to send the local changes to other nodes and `pull` to get changes form one node to local. These push and pull is just a sent of git objects from one to another sent via ssh or https protocol. Thus requiring the ssh keys and target url. Usually one node is considered to be pushed and to pull from making it a single source of truth. 
### Getting repo
The public repo can be cloned from remote to local , in case of private repo public ssh key of the user must be present on the hosting server. 
##### Related Commands 
```bash 
# Simple cloning to target directory 
git clone $REMOTE_REPO_URL $DIRNAME 
  -—branch $BRANCH-NAME # option to clone with specific branch
  -—no-checkout # flag to avoid checkout of head after cloning
  -—depth NUMBER-OF-COMMITS # option to get the n number of commits from top
  
# repo urls in case 
$USERNAME@HOST:PORT:PATH/REPO.git # clone via ssh
https://HOST/USERNAME/REPO_PATH.git # clone via https 
```
##### URL formats with auth 
| Platform      | URL Format.                                                             |
| ------------- | ----------------------------------------------------------------------- |
| **Bitbucket** | `https://x-token-auth:$ACCESS_TOKEN@bitbucket.org/$WORKSPACE/$REPO.git` |
| **GitLab**    | `https://oauth2:$ACCESS_TOKEN@gitlab.com/$USERNAME/$REPO.git`           |
| **GitHub**    | `https://$USERNAME:$ACCESS_TOKEN@github.com/$USERNAME/$REPO.git`        |
### Remotes
##### Related Commands 
```bash 
# To handle remotes 
git remote 
  add $REMOTE_NAME $USERNAME@HOST:PORT:PATH/REPO.git # option to add new remote
  add $REMOTE_NAME https://HOST/USERNAME/REPO_PATH # option to add remote @ http
  set-url $REMOTE $URL # option to edit the remote url 
  show $REMOTE # option to get details about the remote  
```
#### Pulling 
##### Related Commands 
```bash 
git pull $REMOTE $BRANCH
  --rebase # flag to perform rebase operation after fetching comments 
  --merge # flag to perform merge operation after fetching commits (default) 
```
#### Pushing 
##### Related Commands 
```bash
git push $REMOTE $BRANCH 
  -u # flag for pushing the new non existing branch (upstream)
  -f # flag for force pushing matching the remote with local  
```

[[Git]]