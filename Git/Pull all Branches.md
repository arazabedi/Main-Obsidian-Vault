https://linuxhint.com/pull-all-branches-in-git/

```bash
cd <directory>
gh repo clone <github-username/repo-name>
git fetch -all # to fetch metadata 
git branch -r # display all remote branches
git pull -all # pull all branches
```

Then switch brances with:

```bash
git checkout 
```

This will put you in `detached HEAD` state, where you can look around, make experimental changes and commit them, and you can discard any commits you make in this state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, use:

```bash
git switch -c <new-branch-name>

# undo with:
git switch -
```
