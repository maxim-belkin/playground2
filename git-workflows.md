# Git Workflows
Choosing the right Git workflow is a very important step for any (computational) project.
But each and every project starts small...

## Single-User Workflows
When you start a project, you're the only person contributing to it, so you know exactly what you're going to do.
So, there is no (apparent) reason to use anything but an eternal loop of three steps:

1. Make a change
2. Stage it (`git add`)
3. Commit (`git commit`)

If you need to make a minor change to the latest commit, you can do so with `git commit --amend`



### Pros
+ Unlimited "undo"

### Cons
- No backups

### TODO

- Add a 'saw blade' change-add-commit image
