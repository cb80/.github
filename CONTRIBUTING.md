Contributions to repositories in the [organization][org] are very welcome.

# General

Please follow the [trunk based development][trunk-based-dev] process with the
`main` branch as trunk.

## General Process

Our interpretation of trunk based development is:
- `upstream` is the name of the "central" or "blessed" repository. It contains
  the latest and greatest version of the code. All contributions must be based
  on the `main` branch of `upstream`.
- `origin` is the name of *your* individual GitHub fork from which pull requests
  are created.
- Clone `origin` on your development server and add remote `upstream`.
- Before you start working pull the latest changes from the `main` branch of
  `upstream`. Then create a working branch based on `upstream/main` to implement
  your changes. Commit your changes in small sized but reasonable commits. Push
  your working branch to `origin` and create a pull request. Consider an early
  draft pull request so others can see your changes. Update the working branch
  and your pull request based on review comments. Rebase your working branch on
  `upstream/main` if that changed meanwhile. Also resolve conflicts which may
  have been introduced by that.
- After the merge of your pull request delete the working branch on your
  development server and on `origin`. Also update your `main` branch in both
  locations.

## Practical Help

The following does not replace a good understanding of source code management
with git and GitHub. So please walk through a tutorial and use the docs.
Besides myriads of other resources out there we recommend the following:
- the [official git documentation][git-scm]
- the [GitHub help][github-help]
- the guide for the perplexed: [Think like (a) Git][think-like-a-git]

Here are a few commands which may help with the general process from above:

```shell
### setup your environment once:
git config --global user.name "Your Name"
git config --global user.email "your E-Mail-Address"
git config --global color.ui always
git config --global core.pager "less -R"
git config --global credential.helper cache

### clone each repository once:
# create the fork on GitHub
git clone https://github.com/<user>/<repo>.git
cd <repo>
git remote add upstream https://github.com/cb80/<repo>.git

### create a new working branch
git fetch upstream
git checkout -b <working branch name> upstream/main

### commit reasonable changes
# implement your change
git add -p
git commit -m '<descriptive commit message>'

### publish your changes
git fetch upstream
git log main..upstream/main
git push origin <working branch name> # if git log did not show changes
git rebase upstream/main && git push -f origin <working branch name> # if git log showed changes
# create the pull request on GitHub (only for the initial push)

### do after the merge
git fetch upstream
git checkout main
git branch -d <working branch name>
git push origin :<working branch name>
```

Helpful could be to use the following aliases (add them to your `~/.profile` and
open a new shell):

```shell
alias gd='git diff'
alias gdc='git diff --cached'
alias gm='git fetch --prune --tags -f upstream && git fetch --all --prune && [[ -z "$(git status --porcelain)" ]] && git checkout main && git merge --no-edit upstream/main && git push origin main'
alias gs='git status'
alias gt=$'git log --branches --remotes --tags --date-order --graph --decorate=short --color --format="%C(cyan)%h   %C(green)%ar%C(bold yellow)%d%C(reset)\n           %an: %s"'
```

[org]: https://github.com/cb80
[trunk-based-dev]: https://trunkbaseddevelopment.com/
[git-scm]: http://git-scm.com/
[github-help]: https://help.github.com/
[think-like-a-git]: http://think-like-a-git.net/
