---
title: "Git and GitHub"
image:
  path: /assets/img/logo_github.png
...

## Useful Commands

| Description | Command |
| --- | --- |
| Remove untracked and ignored files | `git clean -dfx` |
| List all references | `git show-ref` |
| Add an existing branch as a worktree | `git worktree add PATH EXISTING_BRANCH` |
| Add a new branch and worktree | `git worktree add -b NEW_BRANCH_NAME PATH` |
| Fetch all remotes | `git fetch --all` |
| List remotes | `git remote` |
| Display remote URLs | `git remote -v` |
| Rename a remote | `git remote rename OLD_NAME NEW_NAME` |
| Delete a local branch | `git branch -d BRANCH_NAME` |
| Delete a remote branch | `git push --delete REMOTE BRANCH_NAME` |
| Stash selected files | `git stash push -- FILE1 FILE2` |
| Stash changes while keeping staged files | `git stash push --keep-index` |
| Undo the last local commit while keeping changes | `git reset HEAD~1` |
| Add a remote repository | `git remote add NEW_NAME REPOSITORY_URL` |
| Rebase local commits before pulling | `git pull --rebase` |

## Git Pre-commit Hooks

A pre-commit hook runs local checks before Git creates a commit. If the hook exits
with a non-zero status, the commit is rejected.

Create an executable `.git/hooks/pre-commit` file:

```sh
#!/usr/bin/env bash

make test >& /dev/null
RESULT=$?

if [[ ${RESULT} -ne 0 ]]
then
  RED='\033[0;31m'
  printf "${RED}Unit tests failed. Commit rejected\n"
fi
exit ${RESULT}
```

Make the hook executable:

```bash
chmod u+x .git/hooks/pre-commit
```

A hook can also execute any project-specific script:

```sh
#!/usr/bin/env bash
./my-script.sh
```

Place `my-script.sh` in the repository root.

Source: [Prevent failing builds with pre-commit hooks](https://betterprogramming.pub/no-more-failing-builds-8ac07ac3572c)

A useful example that removes trailing whitespace can be found
[here]({{site.baseurl}}/assets/lib/pre-commit-trim-space.sh)
([original source](https://github.com/imoldman/config/blob/master/pre-commit.git.sh)).

## Reset After an Interrupted Rebase

1. Run `git reflog`.
2. Find the entry labeled `HEAD@{N}: rebase (start)`.
3. Reset to the commit immediately before the rebase:

   ```bash
   git reset --hard HEAD@{N-1}
   ```

For example, if the reflog entry is:

```text
HEAD@{196}: rebase (start): checkout target-branch
```

reset to the state before the rebase with:

```bash
git reset --hard HEAD@{195}
```

## Squash Commits

1. Identify a commit before the commits that should be combined.
2. Start the interactive rebase:

   ```bash
   git rebase -i COMMIT_HASH
   ```

3. Replace `pick` with `s` or `squash` for the commits to combine.
4. After resolving the generated commit message, update the remote branch:

   ```bash
   git push origin BRANCH_NAME --force-with-lease
   ```

> Force-pushing rewrites shared history. Prefer `--force-with-lease` and coordinate
> with collaborators when working on shared branches.
{: .prompt-warning }

Source: [How to squash Git commits](https://www.ekino.fr/publications/comment-squasher-efficacement-ses-commits-avec-git/)

## Configure SSH

```bash
ssh-keygen -t ed25519 -C "your-email@example.com"
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
cat ~/.ssh/id_ed25519.pub
```

Copy the public key into GitHub under **Settings → SSH and GPG keys**.

## Configure GPG Signing

```bash
sudo apt-get install gnupg
gpg --full-generate-key
gpg --list-secret-keys --keyid-format=long
```

The key ID appears after `sec rsa4096/` in the output. Export the public key:

```bash
gpg --armor --export KEY_ID
```

Copy the output into GitHub under **Settings → SSH and GPG keys**, then configure
Git:

```bash
git config --global commit.gpgsign true
git config --global user.signingkey KEY_ID
```

## Merge a Remote Repository into the Current Branch

```bash
git remote add source https://github.com/ORGANIZATION/REPOSITORY.git
git remote update
git switch -c target-branch
git merge --allow-unrelated-histories source/main
git push -u origin target-branch
```

## Useful Links

- [Git branch documentation](https://www.nobledesktop.com/learn/git/git-branches)
- [Git graphical interfaces](https://git-scm.com/book/en/v2/Appendix-A%3A-Git-in-Other-Environments-Graphical-Interfaces)
- [Autolinked references and URLs](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/autolinked-references-and-urls)