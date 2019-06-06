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


### "Minor" request

Sometimes we start working on something and then either stumble upon a bug that
has to be urgently fixed or receive a request from our advisor to (quickly) do
something else. In such a situation we need to put what we've been working on
aside, do the change, and then come back to where we were before. For that purpose Git provides a command called `stash`. So, the workflow here looks as follows:

1. Make a change to your working directory
2. Stash the change with `git stash`
3. Make another change to your working directory
4. Commit the new change
5. Return to where you were before with `git stash pop`


### Exercise (3 minutes)
  1. Add `*.bad` to `.gitignore` file
  2. Stash the change with `git stash`
  3. Create a file called `git_commands.md` and document all the commands that were mentioned above.
  4. Commit the change
  5. Return to editing `.gitignore` with `git stash pop`. Commit the change when you are done.

That's fantastic! Just a few notes before we go further:

1. If you started working on a NEW file that has not been committed to the repository, use the `-u` flag: `git stash -u`.
2. If you need to stash the file that Git ignores, use the `-a` flag: `git stash -a`.
3. If you would like to provide a message for the stash, use the full syntax of the `git stash` command:
  ```
  git stash push -a -m "Description of your WIP"
  # make some other changes and commit them
  git stash pop
  ```
4. To see the list of stashes, use `git stash list `.
5. To drop an obsolete stash, use `git stash drop ...`

### Question

Can you work on two big changes to you projects at the same time?
If you answer "yes", describe the procedure.
If you answer "no", explain why.

## More developers

A natural progression for any project is to gain first new members -- your co-workers, friends, etc.
So, let's have a look at how this progression will affect out project.
