# git-safeswitch

Do you work on multiple features at a time, and need a different index and working directory for each feature branch?

You shouldn't be forced to commit or stash your changes just to switch branches. This tool can help.

## Installation

Super simple:

1\. Clone this repo\
2\. Add `bin` to your `$PATH`

## Basic Usage

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

## Advanced Usage

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

`git backup-uncommitted` saves the state of your index and working directory as two commits on the current branch.

`git restore-uncommitted` does the inverse; it restores these commits to your index and working directory.

`git safe` is a convenience script that runs `git backup-uncommitted`, a `git` command of your choice, and then `git restore-uncommitted` — all in one step.

## Contributing

PRs welcome for all kinds of things, not only including code fixes but also naming suggestions, improving the README, etc.

I'm also interested in learning if this flow is useful for you. Feel free to open an issue to share your experience!
