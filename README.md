# git uncommitted

**Like `git stash`, but a little nicer.**

This tool works like `git stash`, except with these improvements:

- `git uncommitted` saves changes onto your current branch, so there is guaranteed to be no merge conflicts when restoring the changes as uncommitted changes.
- `git uncommitted` saves changes per-branch, whereas`git stash` is global.`git uncommitted` is a better choice for when you want to stash some changes that are specific to the current branch.
- `git uncommitted` saves your untracked files, tracked changes, and staged changes, and remembers which is which. This means that when you restore your changes, your workspace is exactly like you left it.`git stash` can only save all your uncommitted changes together.

## Installation

Super simple:

1\. Clone this repo
2\. Add `bin` to your `$PATH`

## Basic Usage

`git uncommitted backup` saves the state of your index and working directory as two commits on the current branch. Use it like `git stash`.

`git uncommitted restore` does the inverse; it restores these commits to your index and working directory. Use it like `git stash pop`.

That's it!

## Advanced Usage: `git safe`

`git safe` is a simple shortcut that can can help you automatically backup and restore changes when switching branches.

To switch branches, you normally run:

```sh
git checkout <branch>
```

Instead, prefix the sub-command with `safe`:

```sh
git safe checkout <branch>
```

That's it!

This will save the state of your index and working directory to the current branch, checkout the new branch, and then restore the state of the index and working directory of the new branch.

`git safe` can be used with any `git` command that causes the branch to change, like:

```sh
git safe switch <branch>
```

or

```sh
git safe checkout -b <branch>
```

Note that for `git checkout -b`, in most cases you probably want to create a new branch from the original HEAD, not the HEAD that contains the backup of the index and working directory.

In that case, you can run:

```sh
git safe checkout -b <branch> "$(git rev-parse HEAD)"
```

or:

```sh
git branch <branch>
git safe checkout <branch>
```

or:

```sh
git safe checkout -b <branch>
git reset --hard HEAD
```

All of these will produce the same result.

## Implementation

This repo contains a few scripts:

`git uncommitted backup` saves the state of your index and working directory as two commits on the current branch.

`git uncommitted restore` does the inverse; it restores these commits to your index and working directory.

`git safe` is a convenience script that runs `git uncommitted backup`, a `git` command of your choice, and then `git uncommitted restore` — all in one step.

## Contributing

PRs welcome for all kinds of things, not only including code fixes but also naming suggestions, improving the README, etc.

I'm also interested in learning if this flow is useful for you. Feel free to open an issue to share your experience!
