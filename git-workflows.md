# Git Workflows
Choosing the right Git workflow is a very important step for any (computational) project.
But each and every project starts small.

## Single-User Workflows
When you start a project, you are the only person contributing to it, so you have full control over what happens next.
So, there is no reason (at least an apparent one) to use anything but an eternal loop of three steps:

1. Make a change in your working directory
2. Stage it (`<insert git command>`)
3. Commit (`<insert git command>`)

<!-- Please make changes requested above and commit them to the repository -->

If you need to make a minor change to the latest commit, you can do so with `git commit --amend`.


### Pros
+ Unlimited "undo"

### Cons
- No backups

### TODO

- Add a 'saw blade' change-add-commit image
