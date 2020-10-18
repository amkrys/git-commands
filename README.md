# Git

1. Discard all unstaged files from git
    ```bash
    git clean -df
    git checkout -- .
    ```
1. Log all the commits
    ```bash
    git log --pretty=format:"%h %s" --graph
    ```
1. Git status
    ```bash
    git status
    ```
1. Add all changed files to staging area
    ```bash
    git add .
    ```
1. Commit 
    ```bash
    git commit -m "commit msg here"
    ```
1. Amend you last commit
    ```bash
    // Stage your new changes first
    git add .

    // Now commit with a new message that will replace the last commit
    git commit --amend -m "new commit message"
    ```
1. List all remotes
    ```bash
    git remote -v
    ```
1. List all existing branches
    ```bash
    git branch -av
    ```
1. Add remote repo to a git
    ```bash
    git remote add <shortname_of_remote> <remote_url>
    ```
1. Git Push
    ```bash
    git push <shortname_of_remote> <branch>
    ```
1. Git Pull
    ```bash
    git pull <shortname_of_remote> <branch>
    ```
1. Delete a local branch
    ```bash
    git branch -d the_local_branch
    ```
1. Delete a remote branch
    ```bash
    git push origin :the_remote_branch
    ```
1. Duplicate a branch
    ```bash
    git checkout -b new_branch old_branch
    ```
1. Associate Atom Text Editor with your git
    ```bash
    git config --global core.editor "atom --wait"
    ```
1. GUI Git Tool
    ```bash
    gitk --all &
    ```
1. Rewriting History
    ```bash
    git rebase -i <branch/commit_id>
    ```
1. Reset merge changes
    ```bash
    git reset --merge ORIG_HEAD
    ```
1. Stop tracking a file [[More Info](http://stackoverflow.com/a/3320183/2745762)]
    ```bash
    git update-index --assume-unchanged [<file> ...]
    ```
1. Start tracking a file [[More Info](http://stackoverflow.com/a/3320183/2745762)]
    ```bash
   git update-index --no-assume-unchanged [<file> ...]
    ```
1. Know which branch a particular commit is on [[More Info](http://stackoverflow.com/a/2707110)]
    ```bash
   git branch --contains <commit_id>
    ```
1. View the change history of a file using Git versioning [[More Info](http://stackoverflow.com/a/278242)]
    ```bash
    git log -p <filename>
    ```
    or for visually
    ```bash
    gitk <filename>
    ```
1. Delete last commit [[More Info](http://stackoverflow.com/a/6866485)]
    ```bash
    git reset --hard HEAD~1
    ```
1. Check history of commits from different branches
    ```bash
    git log --oneline --decorate --graph --all
    ```
1. Apply changes done in one commit on your working code, without committing
    ```bash
    git cherry-pick -n <HASH>
    ```
1. [Autocorrect git commands](https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration)(it iwll wait for 3 seconds before executing the command automatically)
    ```bash
    git config --global help.autocorrect 30
    ```
1. Squash '3' commits
    ```bash
    git reset --soft HEAD~3
    git commit -m "new commit message"
    ```
1. Delete all local branches that are already merged into the currently checked out branch
    ```bash
    git branch --merged | egrep -v "(^\*|master|dev)" | xargs git branch -d
    ```
    > `master` and `dev` are excluded in case they are an ancestor
1. Checkout to previous branch
    ```bash
    git checkout -
    ```
1. Create a `zip` file of the repository at the current commit. [[Read More](https://git-scm.com/docs/git-archive)]
    ```bash
    cd git_repo_dir
    git archive -o git_repo_dir.zip HEAD
    ```
    > Will not include the git folder but also anything that is in the gitignore file will be excluded as well.


### Aliases

How to setup these aliases

- Copy-Paste these alias inside your `.bashrc` or `.zshrc`
- Save.
- Goto terminal and execute `source ~/.bashrc` or `source ~/.zshrc`.
- Done, now simply call the alias to execute the command as defined.

```bash
#############################################################
# Git aliases
#############################################################

alias gitLog="git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset%n' --abbrev-commit --date=relative --branches"
alias gitAmmend='git commit --amend -m'
alias gitResetMerge='git reset --merge ORIG_HEAD'
alias gitDeleteLastCommit='git reset --hard HEAD~1'
alias gitMergeToMaster='git push && git checkout master && git merge develop && git push && git checkout develop'
alias gitDeleteAllLocalMergedBranches='git branch --merged | egrep -v "(^\*|master|dev)" | xargs git branch -d'
alias gitInitForAndroid="git init && cp $ANDROID_HOME/.gitignore ./.gitignore"

# Get the history of specific file across file renames
alias gitLogOfFile="git log --follow -p --stat --"

# Get the diff without leading +/- 
alias gitDiffClean="git diff --color-words"

# Discard all local unstaged changes
alias gitDiscardUnstagedChanges="git checkout -- ."

# Unstage everything - retain changes
alias gitUnstagedChanges="git reset"

# Stage selective
alias gitAddSelective='git add -p'

# Unstage selective - retain changes
alias gitUnstageSelective='git reset -p'

# Discard all local changes to all files permanently
alias gitDiscardAllChanges="git reset --hard"

# Create a git diff patch file
# Use as: gitCreatePatch patch_name
alias gitCreatePatchFromDiff="git diff > $1.patch"

# Apply the patch to repository
# Use as: gitApplyPatch patch_name.patch
alias gitApplyPatch="git apply $1"

# >> Update From Upstream Cmd
# [Breakdown of cmd]
# git checkout develop = Switch to local develop branch
# git fetch upstream = Fetch the latest repo info from upstream i.e tags, branches, etc
# git pull upstream develop = Pull the latest code changes from upstream into the local develop branch
# git push origin develop = Push the updated local develop branch to orgin repo
# git checkout - = Switch back to last branch
alias gitSyncDevelopFromUpstream='git checkout develop && git fetch upstream develop && git pull upstream develop && git push origin develop && git checkout --'

# Bash Function to squash commits
# Read more: https://stackoverflow.com/a/7131683/2745762
function gitSquashCommits() { git reset --soft HEAD~"$1"; }

# Bash Function to undo last squash commit
function gitResetLastSquash() { git reset "HEAD@{1}"; }

# Bash Function to create a zip of the git traked directory, ignoring everything that is gitignored.
# This acts as an alias.
function gitZip(){
    result=${PWD##*/};
    echo "Zipping git repository to: $result.zip";
    git archive -o $result.zip HEAD;
}

# Reset the last git squash command
# Read more: https://stackoverflow.com/a/2531803/2745762
# This acts as an alias.
function resetLastGitSquash() { git reset "HEAD@{1}"; }

# Drop these in your ~/.bashrc or ~/.zshrc file

# Find out how many commits ahead or behind your current branch is from another branch
# Use as: git1VznyypTWrzvnje2KeKD12USGUkQ6P1erD develop
function git1VznyypTWrzvnje2KeKD12USGUkQ6P1erD(){ 
  local CURRENT_BRANCH=$(git rev-parse --abbrev-ref HEAD)
  local STATUS=$(git rev-list --left-right --count  "$1"..."$CURRENT_BRANCH" | awk '{print ""$1","$2""}')
  local BEHIND=$(echo "$STATUS" | awk '{split($0,a,","); print a[1]}')
  local AHEAD=$(echo "$STATUS" | awk '{split($0,a,","); print a[2]}')
  echo ""
  echo "  $CURRENT_BRANCH <> $1"
  echo "    - Ahead by $AHEAD"
  echo "    - Behind by $BEHIND"
}

# Create a new git repo with one README commit and CD into it
function gitNewRepoWithReadme() { mkdir $1; cd $1; git init; touch README; git add README; git commit -mFirst-commit;}

# Stop tracking a file in local
function gitStopTracking() { git update-index --assume-unchanged "$1" }
```
