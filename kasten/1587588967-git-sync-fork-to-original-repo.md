# 1587588967 git-sync-fork-to-original-repo
#git #cheatsheet #sync #fork

## Add the original repo as an upstream directory
```bash
git remote add upstream https://github.com/<original owner>/<original repo>.git
```

## How to merge the original repo into your fork
Fetch all of the changes from the original repo:
```bash
git fetch upstream
```

Make sure you are on your forks desired branch:
```bash
git checkout <branch>
```

Now merge the upstream with your fork:
```bash
git merge upstream/master
```

Now the local branch is synced with the original repo master branch, all that needs to be done now is to push the changes to your forked repo in github:
```bash
git push
```

All the information here was retrieved from the following link:
https://digitaldrummerj.me/git-syncing-fork-with-original-repo/

## Links
