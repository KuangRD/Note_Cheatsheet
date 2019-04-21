# Git Note

**Author**: Kuang(kuang.work@gmail.com)

**Date**: Apr 13th, 2019



## Git Command
```sh
git init
``` 

`git diff` followed by `git add`, check modified content before add to stage.
```sh
git diff <somefile>
```
### git version revert

`HEAD`pointer to currernt version

```sh
git reset --hard HEAD^  # revert to last version
git reset --hard HEAD^^  # revert to -2 version
git reset --hard HEAD~100 # revert to -100 version
git reset --hard HEAD 1094a # revert to **** commit ID
```
```sh
git reflog # [Go to "Future" version] get git 'command' log
git log # [Go Back to history version]get last three 'commit' log 
```
### Git Concept

**Working Directory**: directory with .git file

**Repository** file add to repository by `git add` 

**stage** field between `git add` and `git commit`

![alt text][git_structure]

## Undo Modification

1. Undo modification in working directory, back to last git add or commit version 

```sh
git checkout -- <file> 
```
2. Convert the added modification to working directory(current added&uncommit part)
```sh
git reset HEAD -- <file> 
```

## Delete File

```sh
git rm <file>
git commit
```
Undo file delete
```sh
git rm <file>
git checkout <file>
```

## Branch
### Branch Establishment&Merge
```sh
git branch dev # Create 'dev' branch
git branch -b dev # Establish 'dev' branch and checkout in 'dev'
```

```sh
git branch # List all branches and mark current branch with '*'
```

```sh
git checkout master
git merge dev # Merge Specifical Branch 'dev' to Current Branch
git branch -d dev # delete 'dev' branch
```

```sh
git log --graph # Show Branch & Merge Graph
```
### Merge Conflict
```sh
git status # Check which part get conflict
```
Modify conflict in target files.
```sh
git add <file>
git commit -m ''
git branch -d <branch>
```

### --no-ff merge
**Merge branch in 'Fast Forward' mode will lost branch information after delete branch**
```sh
git merge --no-ff -m "merge with no-ff" dev
```

### Bug Branch
Coding on `dev` branch, uncommited content, meet up with an emergency bug on branch `master`.
```sh
git stash # Save current working directory
git status # Check Current working directory
git checkout master
git checkout -b issue-101
git add .
git commit -m "Fix bug 101"
git checkout master
git merge --no-ff -m "Merged bug fix 101" issue-101
git checkout dev
git stash list # Check Stash status
git stash apply # Recover saved working directory staff and didn't delete content in stash
git stash drop  # delete content in stash
git stash pop # Same effect as git stash apply + git stash drop
```

### Feature Branch
For new feature development, same operations as `Bug Branch`.
```sh
git branch -D <branch-name> # Force delete unmerged branch(Such as confidential prototype development) 
```

### Collaboration
The default name of remote repository is `origin`.
```sh
git remote
git remote -v # Check detail information of remote repository
```
#### Push Branch
```sh
git push origin master
git push origin dev
```
Branch Strategies
* `master` should be synchronous with remote repo.
* `dev` should be synchronous with remote repo.
* bug-branch is synchronized with remote repo depend on need.
* feature-branch is synchronized with remote repo depend on need.

```sh
git clone ...
git checkout -b dev origin/dev # remote repo's dev branch to local dev

git add .
git commit -m '....'

git push origin dev
```
resolve conflict with remote repo
```sh
git pull # will get error
git branch --set-upstream-to=origin/dev dev # link local dev branch to remote repo dev
git pull
```

### Rebase
Multiple Commit occur when conflict exist, then git log will be confusing.
```sh
git rebase # Solve the too many commits problem
git push
```
**cons:** modify local branch.

## Tag Management
**Tag** is a pointer to specific commit.
```sh 
git branch 
git checkout <branch-name>
git tag <name> # such as 'v1.0', tag on current newest commit
git tag <name> commit-id
git tag # show all the tags in the order of alphabet.
git show <tag-name> # show specific tag info
git tag -a <tag-name> -m 'blah blah' # Add tag with message
git push origin  <tag-name>
git push origin --tags # Push all the unpushed commits in one command.
```
*Each the tag is linked to a commit, if a commit occurs in both of master and dev, we can see the linked tag on both branch.*

```sh
git tag -d <tag-name> # Delete local tag
git push origin :refs/tags/<tag-name> # Delete tag in remote repo
```

## Configure Git
```sh
git config --global color.ui true
```
### .gitignore
[Git Offical templates for .gitignore](https://github.com/github/gitignore)
File ignore principle:
1. System produced files
2. Files produced during compile.
3. Confidential contents.

```sh
git add -f <ignored-filename> # Force add some file without .gitignore constriction.
git check-ignore -v <suspicious_ignored file> # Check which rule in .gitignore ignore <suspicious_ignored file>
```
### Set alias
```sh
git config --global alias.co checkout
git config --global alias.ci commit
git config --global alias.br branch
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```
`--global` set git config to current user. Without `--global`, `git config` only works for current repository(.git/config). 

## Set-up Git sever
[LIAO Xuefeng 搭建Git服务器](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137583770360579bc4b458f044ce7afed3df579123eca000)

## Revert Wrong Commit
```sh
git revert <commit id>
git push
```


## Submitting a Pull Request to An Open source library

1. Fork the official repository.
2. Create a topic branch.
3. Implement your feature or bug fix.
4. Add, commit, and push your changes.
5. Submit a pull request.


## Ref
1. [LIAO Xuefeng Git Tutorial](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000) 
2. [Writing READMEs](https://www.udacity.com/course/writing-readmes--ud777)

[//]: # (Image Reference)
[git_structure]: ./pic/git_structure.jpg