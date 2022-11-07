
## Managing Remotes

Add a named remote (you can have multiple remotes):

```git
git remote add github https://github.com/user/repo.git
git remote add gitlab https://gitlab.com/user/repo.git
git push github
git push gitlab
```

Listing remotes:
```git
git remote -v
```

Overloading origin with another remote:

```git
git remote set-url ––add origin https://github.com/user/repo.git
```

Deleting a remote:

```git
git remote remove origin
```

Synchronizing a local fork with remote:

```git
git remote add upstream https://github.com/OriginalRepo/OriginalProject.git
git merge upstream/master
git push origin master
```

## Submodules

Creating submodules in an existing repo:

```git
git submodule add https://github.com/repo.git
```

Cloning a repository with submodules:

```git
git clone https://github.com/repo.git
git submodule init
git sumbodule update
```

## Staging and Commits

Remove a file or folder from the staging area:

```git
git reset HEAD -- <file or folder>
```

## Branching

Deleting a local branch:

```git
git branch -d branch_name
```

Deleting a remote branch:

```git
push --delete remote_name branch
```

Fetch all branches from a remote:

```git
git fetch --all
```

Checkout specific files from another branch:

```git
git checkout branch_name file file2
```

Update a single file from upstream:

```git
git checkout origin/master --  file
```

Merge a specific commit to current branch:

```git
git cherry-pick 63344f2
```

Hide branch changes:

```git
git stash
```

Restore branch changes:

```git
git stash apply
```

## Workflows

### Fetch a PR for local testing

Checkout PR into a new branch.

```git
git fetch upstream pull/{PR-NUMBER}/head:test-branch-name
```

Merge PR into local master branch, and test.

```git
git checkout master
git merge test-branch-name
```

If code was bad, revert to previous commit.

```git
git reset --hard HEAD@{1}
```


### Move a commit to another repo

```git
git config pull.rebase true
git show --pretty=email <commit> > patch
git am --committer-date-is-author-date < patch
```

### Finding things

Find commits that contain a specific string:

```git
git log -S 'your_search_string' --oneline --name-status
```

Display the contents of a commit:

```git
git show <commit>
```

## Undoing Stuff and Fixing Mistakes

Undo changes to a file:

```git
git checkout -- <file>
```

Undo a merge, keeping local changes

```git
git reset --merge ORIG_HEAD
```

Undo the last local commit

```git
git reset --soft HEAD~1
```

Undo a commit pushed to GitHub deleting all history:

```git
git reset --hard 93827927ed6e245be
git reset HEAD~1
git stash
git add .
git commit -m "your new commit message here"
git push --force
```

To pull the new history to a repository that has diverged:

```git
git pull --rebase
```


