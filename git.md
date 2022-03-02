# Archive a branch
Source: https://stackoverflow.com/questions/1307114/how-can-i-archive-git-branches

To archive a branch, you actually just tag it and delete that branch
```bash
# Local: Tag 
git tag archive/<branch> <branch>

# Alternatively, you can add a message for why with
git tag -a archive/<branch> <branch> -m "branch merged so I archive it"

# Local: delete the branch. -d check first if the branch been merged, while -D just force deletion
git branch -d <branch>

# Remote: push the tag
git push origin --tags

# Remote: delete branch
git push origin --delete <branch>
```

