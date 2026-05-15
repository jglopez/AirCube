# Fork branching strategy

This fork maintains two long-lived branches:

- **`master`** — exact mirror of [`StuckAtPrototype/AirCube:master`](https://github.com/StuckAtPrototype/AirCube). No personal commits here.
- **`personal`** — personal customizations, rebased on top of `master`. This is the default branch of this fork.

## Routine sync (pulling in upstream changes)

```
git fetch upstream
git checkout master
git reset --hard upstream/master
git push origin master
git checkout personal
git rebase master
```

**GitHub web UI shortcut:** On this fork, switch to the `master` branch and use the "Sync fork" button to fast-forward `master` to upstream. You still need to run `git rebase master` on `personal` locally afterward.

## Contributing back to upstream

Branch off `master` (not `personal`), make changes, push, and open a PR to the upstream repo:

```
git checkout -b my-feature master
# ...make changes...
git push origin my-feature
gh pr create --repo StuckAtPrototype/AirCube --head jglopez:my-feature
```

### Contributing back a commit already on `personal`

Cherry-pick the specific commit(s) onto a new branch off `master`:

```
git checkout -b my-feature master
git cherry-pick <sha>
git push origin my-feature
gh pr create --repo StuckAtPrototype/AirCube --head jglopez:my-feature
```

After the upstream PR merges, the next routine sync will automatically drop the cherry-picked commit from `personal` (rebase sees it already in the base and skips it).

## Remotes

| Remote | URL |
|--------|-----|
| `origin` | `https://github.com/jglopez/AirCube.git` |
| `upstream` | `https://github.com/StuckAtPrototype/AirCube.git` |
