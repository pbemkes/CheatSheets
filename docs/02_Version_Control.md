# Version Control

- [Git](#1-git)
- [GitHub](#2-github)
- [GitLab](#3-gitlab)
- [Bitbucket](#4-bitbucket)

---

## 1. Git

### 1. Git Setup and Configuration

- `git --version` - Check Git version
- `git config --global user.name "Your Name"` - Set global username
- `git config --global user.email "your.email@example.com"` - Set global email
- `git config --global core.editor "vim"` - Set default editor
- `git config --global init.defaultBranch main` - Set default branch name
- `git config --list` - View Git configuration
- `git help <command>` - Get help for a Git command

### 2. Creating and Cloning Repositories

- `git init` - Initialize a new Git repository
- `git clone <repo_url>` - Clone an existing repository
- `git remote add origin <repo_url>` - Link local repo to a remote repo
- `git remote -v` - List remote repositories

### 3. Staging and Committing Changes

- `git status` - Check the status of changes                                
- `git add <file>` - Add a file to the staging area                   
- `git add .` - Add all files to the staging area                                                        
- `git commit -m "Commit message"` - Commit staged files                         
- `git commit -am "Commit message"` - Add & commit changes in one step
- `git commit --amend -m "New message"` - Modify the last commit message

### 4. Viewing History and Logs

- `git log` - Show commit history
- `git log --oneline` - Show history in one-line format
- `git log --graph --decorate --all` - Display commit history as a graph
- `git show <commit_id>` - Show details of a specific commit
- `git diff` - Show unstaged changes
- `git diff --staged` - Show staged but uncommitted changes
- `git blame <file>` - Show who modified each line of a file

### 5. Branching and Merging

- `git branch` - List all branches
- `git branch <branch_name>` - Create a new branch
- `git checkout <branch_name>` - Switch to another branch
- `git checkout -b <branch_name>` - Create and switch to a new branch
- `git merge <branch_name>` - Merge a branch into the current branch
- `git branch -d <branch_name>` - Delete a local branch
- `git branch -D <branch_name>` - Force delete a branch

### 6. Working with Remote Repositories

- `git fetch` - Fetch changes from remote repo
- `git pull origin <branch_name>` - Pull latest changes
- `git push origin <branch_name>` - Push changes to remote repo
- `git push -u origin <branch_name>` - Push and set upstream tracking
- `git remote show origin` - Show details of the remote repo
- `git remote rm origin` - Remove the remote repository

### 7. Undoing Changes

- `git restore <file>` - Unstage changes
- `git restore --staged <file>` - Unstage a file from the staging area
- `git reset HEAD~1` - Undo the last commit but keep changes
- `git reset --hard HEAD~1` - Undo the last commit and discard changes
- `git revert <commit_id></commit_id>` - Create a new commit to undo changes
- `git stash` - Save uncommitted changes temporarily
- `git stash pop` - Apply stashed changes
- `git stash drop` - Remove stashed changes

### 8. Tagging and Releases

- `git tag` - List all tags
- `git tag <tag_name>` - Create a tag
- `git tag -a <tag_name> -m "Tag message"` - Create an annotated tag
- `git push origin <tag_name>` - Push a tag to remote
- `git tag -d <tag_name>` - Delete a local tag
- `git push --delete origin <tag_name>` - Delete a remote tag

### 9. Working with Submodules

- `git submodule add <repo_url> <path>` - Add a submodule
- `git submodule update --init --recursive` - Initialize and update submodules
- `git submodule foreach git pull origin main` - Update all submodules

### 10. Git Aliases (Shortcuts)

- `git config --global alias.st status` - Create alias for status command
- `git config --global alias.co checkout` - Create alias for checkout command
- `git config --global alias.br branch` - Create alias for branch command
- `git config --global alias.cm commit` - Create alias for commit command
- `git config --list | grep alias` - View all configured aliases

### 11. Deleting Files and Folders

- `git rm <file>` - Remove a file and stage deletion
- `git rm -r <folder>` - Remove a directory
- `git rm --cached <file>` - Remove file from repo but keep locally

### 12. Force Push and Rollback (Use with Caution)

- `git push --force` - Force push changes
- `git reset --hard <commit_id>` - Reset repo to a specific commit
- `git reflog` - Show history of HEAD changes
- `git reset --hard ORIG_HEAD` - Undo the last reset

---

## 2. GitHub

### 1. Authentication & Configuration

- `gh auth logout` – Log out of GitHub CLI
- `gh auth status` – Check authentication status
- `gh auth refresh` – Refresh authentication token
- `gh config set editor <editor>` – Set the default editor (e.g., nano, vim)
- `gh config get editor` – Get the current editor setting

### 2. Repository Management

- `gh repo list` – List repositories for the authenticated user
- `gh repo delete <name>` – Delete a repository
- `gh repo rename <new-name>` – Rename a repository
- `gh repo fork --clone=false <url>` – Fork a repository without cloning

### 3. Branch & Commit Management

- `gh branch list` – List branches in a repository
- `gh branch delete <branch>` – Delete a branch
- `gh browse` – Open the repository in a browser
- `gh co <branch>` – Check out a branch

### 4. Issue & Pull Request Handling

- `gh issue create` – Create a new issue
- `gh issue close <issue-number>` – Close an issue
- `gh issue reopen <issue-number>` – Reopen a closed issue
- `gh issue comment <issue-number> --body "Comment"` – Add a comment to an issue
- `gh pr list` – List open pull requests
- `gh pr checkout <pr-number>` – Check out a pull request branch
- `gh pr close <pr-number>` – Close a pull request

### 5. Gists & Actions

- `gh gist create <file>` – Create a new gist
- `gh gist list `– List all gists
- `gh workflow list` – List GitHub Actions workflows
- `gh run list` – List workflow runs

### Webhooks

- Go to Repo → Settings → Webhooks → Add Webhook
- Events: push, pull_request, issues, etc.
- Payload sent in JSON format to the specified URL

---

## 3. GitLab

### 1. Project & Repository Management

- `gitlab project create <name>` – Create a new project
- `gitlab repo list` – List repositories
- `gitlab project delete <id>` – Delete a project
- `gitlab repo fork <repo>` – Fork a repository
- `gitlab repo clone <url>` – Clone a GitLab repository
- `gitlab repo archive <repo>` – Archive a repository

### 2. Issues & Merge Requests

- `gitlab issue list` – List all issues in a project
- `gitlab issue create --title "<title>" --description "<desc>"` – Create an issue
- `gitlab issue close <issue_id>` – Close an issue
- `gitlab issue reopen <issue_id>` – Reopen an issue
- `gitlab merge_request list` – List merge requests
- `gitlab merge_request create --source-branch <branch> --target-branch <branch> --title "<title>"` – Create a merge request
- `gitlab merge_request close <mr_id>` – Close a merge request

### 3. Pipeline & CI/CD

- `gitlab pipeline trigger` – Trigger a pipeline
- `gitlab pipeline list` – List all pipelines
- `gitlab pipeline retry <pipeline_id>` – Retry a failed pipeline
- `gitlab pipeline cancel <pipeline_id>` – Cancel a running pipeline
- `gitlab pipeline delete <pipeline_id>` – Delete a pipeline
- `gitlab runner register` – Register a CI/CD runner
- `gitlab runner list` – List registered runners
- `gitlab runner unregister <runner_id>` – Unregister a runner

### 4. User & Group Management

- `gitlab user list` – List users in GitLab
- `gitlab user create --name "<name>" --email "<email>"` – Create a new user
- `gitlab group list` – List groups in GitLab
- `gitlab group create --name "<group_name>" --path "<group_path>"` – Create a group
- `gitlab group delete <group_id>` – Delete a group

### 5. Access & Permissions

- `gitlab project member list <project_id>` – List project members
- `gitlab project member add <project_id> <user_id> <access_level>` – Add a user to a project
- `gitlab group member list <group_id>` – List group members
- `gitlab group member add <group_id> <user_id> <access_level>` – Add a user to a group

### 6. Repository Protection & Settings

- `gitlab branch protect <branch>` – Protect a branch
- `gitlab branch unprotect <branch>` – Unprotect a branch
- `gitlab repository mirror` – Set up repository mirroring
- `gitlab repository settings update` – Update repository settings

### Webhooks:

- Go to Settings → Webhooks
- Select triggers: Push events, Tag push, Merge request, etc.
- Use GitLab CI/CD with .gitlab-ci.yml

---

## 4. Bitbucket

### 1. Repository Management

- `bitbucket repo create <name>` - Create a repository
- `bitbucket repo list` - List all repositories
- `bitbucket repo delete <name>` - Delete a repository
- `bitbucket repo clone <repo-url>` - Clone a repository
- `bitbucket repo fork <repo>` - Fork a repository
- `bitbucket repo update <repo>` - Update repository settings

### 2. Branch Management

- `bitbucket branch create <branch-name>` – Create a new branch
- `bitbucket branch list` - List all branches
- `bitbucket branch delete <branch-name>` - Delete a branch

### 3. Pipeline Management

- `bitbucket pipeline run` - Run a pipeline
- `bitbucket pipeline list` - List pipelines
- `bitbucket pipeline stop <pipeline-id>` - Stop a running pipeline
- `bitbucket pipeline rerun <pipeline-id>` - Rerun a pipeline

### 4. Issue Tracking

- `bitbucket issue list` - List all issues
- `bitbucket issue create "<title>" --kind=<bug/task/enhancement>` - Create an issue
- `bitbucket issue update <issue-id> --status=<open/closed/resolved>` - Update issue status
- `bitbucket issue delete <issue-id>` - Delete an issue

### 5. Pull Request Management

- `bitbucket pullrequest create --source <branch> --destination <branch>` - Create a pull request
- `bitbucket pullrequest list` - List pull requests
- `bitbucket pullrequest merge <id>` - Merge a pull request
- `bitbucket pullrequest approve <id>` - Approve a pull request
- `bitbucket pullrequest decline <id>` - Decline a pull request

### Webhooks

- Go to Repo → Repository Settings → Webhooks
- Choose event typesd like repo:push, pullrequest:created, etc