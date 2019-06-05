# Git Workflows
Choosing the right Git workflow is a very important step for any (computational) project.
But each and every project starts small.

## Single-User Workflows
When you start a project, you are the only person contributing to it, so you have full control over what happens next.
So, there is no reason (at least an apparent one) to use anything but an eternal loop of three steps:

1. Make a change in your working directory
2. Stage it (`git ...`)
3. Commit (`git ...`)

### Pros
+ Unlimited "undo"

### Cons
- No backups


### Exercise (1 minute)
  Add the first Git command (`git ...`) above and commit it.


### Making small changes to your last commit
When and if you need to make a small change to the last commit -- whether
fixing a typo or reformatting a paragraph -- you can do so with `git commit --amend`:

1. Make a small change
2. Stage it
3. Amend the last commit with `git commit --amend`


### Exercise (2 minutes)
  Add the second Git command (`git ...`) above and amend the last commit.


