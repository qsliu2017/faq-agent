# Rewrite author and committer across Git history

Use a history rewrite to set a consistent `user.name` and `user.email` for all past commits.
This changes commit IDs and requires force pushing shared branches.
Coordinate with collaborators before proceeding.

## Preferred: git filter-repo

`git filter-repo` is faster and safer than `filter-branch`.
Install it if you do not already have it, then run one of the following.

Set the same name and email for every commit (author and committer):

```bash
git filter-repo --force \
  --commit-callback '
commit.author_name  = b"Your Name"
commit.author_email = b"you@example.com"
commit.committer_name  = b"Your Name"
commit.committer_email = b"you@example.com"
'
```

Only change specific old identities using a mailmap:

```bash
# 1) Create a mailmap mapping old -> new
cat > .mailmap <<'MAP'
Your Name <you@example.com> <old@example.com>
MAP

# 2) Rewrite history applying the mailmap
git filter-repo --force --mailmap .mailmap
```

## Built-in: git filter-branch

If `git filter-repo` is unavailable, use `filter-branch`.
This is slower, but ships with Git.

Set the same name and email for all commits:

```bash
git filter-branch --env-filter '
export GIT_AUTHOR_NAME="Your Name"
export GIT_AUTHOR_EMAIL="you@example.com"
export GIT_COMMITTER_NAME="Your Name"
export GIT_COMMITTER_EMAIL="you@example.com"
' --tag-name-filter cat -- --branches --tags
```

Only rewrite commits from a specific old email:

```bash
OLD_EMAIL="old@example.com"
NEW_NAME="Your Name"
NEW_EMAIL="you@example.com"

git filter-branch --env-filter '
if [ "$GIT_COMMITTER_EMAIL" = "'$OLD_EMAIL'" ]; then
  export GIT_COMMITTER_NAME="'$NEW_NAME'"
  export GIT_COMMITTER_EMAIL="'$NEW_EMAIL'"
fi
if [ "$GIT_AUTHOR_EMAIL" = "'$OLD_EMAIL'" ]; then
  export GIT_AUTHOR_NAME="'$NEW_NAME'"
  export GIT_AUTHOR_EMAIL="'$NEW_EMAIL'"
fi
' --tag-name-filter cat -- --branches --tags
```

## Verify and publish

```bash
# Inspect authors/committers after the rewrite
git log --pretty='%h %an <%ae> | %cn <%ce>' --no-merges | head -50

# Force push all branches and tags (history changed)
git push --force-with-lease --all
git push --force-with-lease --tags
```

Tell collaborators to rebase onto the new history or re-clone.
Anyone with old history will need to reset their local branches.

## Only affect future commits

To set identity for new commits without rewriting history:

```bash
git config user.name "Your Name"
git config user.email "you@example.com"

# Or globally
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

## Display-only alternative

If you only need consistent display in `git shortlog` and hosting UIs, use `.mailmap`.
This does not rewrite history or change commit IDs.
See `answers/git-rewrite-author-email.md:Preferred: git filter-repo` for a mailmap-based rewrite.

