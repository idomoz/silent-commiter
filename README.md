# silent-committer

Commits changes as original authors

## Requirements

- [xonsh](https://xon.sh/)

## Usage

```sh
xonsh silent_committer.xsh <commit_msg>
```

## How does this work?

Silent commiter does the following steps:

1. Unstages all changes.
2. Runs `git add -p` and splits all hunks that are splittable.
3. Parses the output of `git add -p` to get all hunks.
4. Stashes all changes (to be able to run `git blame`).
5. Finds the authors of all the changed lines using `git blame -w`.
6. Calculates the author of each hunk (if there is only one author for the changed lines in the hunk it uses that author,
otherwise it uses one of the authors of the changed lines that contibuted most to that hunk).
7. For each author - it stages all of its hunks and creates a commit using the `<commit_msg>` passed as arg and appends the author name.
For example if `<commit_msg>` is `pep8 formatting` then it will create a commit with the message `pep8 formatting; For user: John Doe`
for each author with the author being the original author while the committer still remains you.
8. All line additions (new and not modified lines) will be commited with you as the author.

## Disclaimer

In order to prevent code lost, its best to first commit your changes and the run `git reset --soft HEAD^` so that you could return to your changes
using `git reflog` or alternitively stash your changes and run `git stash apply` to keep your changes in the stash if something goes wrong.
