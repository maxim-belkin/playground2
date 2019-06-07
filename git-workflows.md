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


## "Minor" request

Sometimes we start working on new change and then stumble upon a bug that
has to be urgently fixed or receive a request from our advisor to (quickly) do
something else.
In such a situation we need to put what we've been working on
aside, do the change, and then come back to where we were before. For that purpose Git provides a command called `stash`. The workflow looks as follows:

1. Make a change to your working directory
2. Put the changes aside with `git stash`
3. Make another change to your working directory
4. Commit the new change
5. Return to where you were before with `git stash pop`

Let's try it out with the following exercise:

### Exercise (3 minutes)
  1. Add `*.bad` to `.gitignore` file
  2. Stash the change with `git stash`
  3. Create a file called `git_commands.md` and document all the commands that were mentioned above.
  4. Commit the change
  5. Return to editing `.gitignore` with `git stash pop`. Commit the change when you are done.

That's fantastic! A few notes before we go further:

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

To simulate the scenario in which we have a second co-maintainer, let's first create a local backup of our repository.
Do steps 1 through 4 of "Local GitHub" section in [tricks.md](./tricks.md)

Now that we have our repository in our local "GitHub", we can share its location with our "collaborator".
The first thing that collaborator does is clones it to his computer.
In our case, let's clone it to a directory right next to our repository.
Execute:

```
cd ..
git clone file://$HOME/Desktop/backup collaborator
cd collaborator
```

The above commands will clone the repository to the directory called `collaborator`.
Now, let's change our name and email address in that repository only.
Note, you would not have to do this in a real-life scenario.

```
git config --local user.name "Collaborator Jones"
git config --local user.email "big@secret.com"
```

Let's make a change as a collaborator and upload it to backup:
edit the file called `todo.md` and add a new task "- Document cool Git commands"

Add it to the staging area with `git add` and commit it with `git commit -m "[Collaborator] Updating todo"`.
Note that if you sign your commits with GPG, use `--no-gpg-sign` flag as Git will refuse to create a commit otherwise.
Once that is done, let's push it back to our local GitHub folder:

```
git push origin
```
Note, that when we cloned the repository, it automatically set up the "origin" remote with proper URL and that's why
we were able to upload our changes with a mere `git push origin`.

Now, let's go back to our main "playground" folder where we are ourselves and also update `todo.md`:
add a line that reads "- Deal with pulling conflicts". Add and commit the change to the repository with:
`git commit -m "[Main repo]: Updating todo"`.

Let us now try and push the changes to our "local GitHub":

```
$ git push file://$HOME/Desktop/backup.git master

To file:///Users/mbelkin/Desktop/backup.git
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'file:///Users/mbelkin/Desktop/backup.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

Yes, we've got a conflict. Of course, we can pull the changes to our repository
with `git pull`, resolve the conflict (by editing the files that conflict -- in
this case this is `todo.md`), mark it as resolved with `git add`, and finalize the 
pull with `git commit`. You will end up with a history that looks like so:

```
$ git pull file://$HOME/Desktop/backup.git master

$ git log --oneline --decorate --graph -4

*   a4d328a (HEAD -> master) Merge branch 'master' of file:///Users/mbelkin/Desktop/backup.git
|\
| * 22e41c8 [Collaborator] Updating todo
* | 036898d [Main repo]: Updating todo
|/
* 706f4ab ask to explain the answer
```

Here, we've created a "merge" commit -- a commit with 2 parents. There is nothing wrong with merge commits,
so you should definitely consider using the described method. The only "gotcha" is that your history becomes
slightly harder to read.
However, we should mention another way of pulling upstream changes.
If you ran the above commands, let's undo them:

1. Move HEAD back to the commit that says "[Main repo]: Updating todo".
   This is commit `036898d` in the image above:
   
   ```
   git reset --soft 036898d
   ```
2. Reset the content of the `todo.md` file to what it used to be at the time of `036898d` commit:

   ```
   git checkout 036898d -- todo.md
   ```
Now, let's pull the changes from our local GitHub once again, but this time using the `--rebase` flag:

```
git pull --rebase file://$HOME/Desktop/backup.git master
```

We will again encounter a conflict. Now, we have to:

1. resolve the conflict
2. Mark the conflict as resolved with `git add`
3. And, to finalize the pull, execute:

 ```
 git rebase --continue
 ```

Now, let's have a look at the history of our repository:

```
$ git log --oneline --decorate --graph -4

* 3258ae5 (HEAD -> master) [Main repo]: Updating todo
* 22e41c8 [Collaborator] Updating todo
* 706f4ab ask to explain the answer
* a441052 update tricks
```

Notice that although we had to do "absolutely the same" conflict resolution, the history is now linear, without merge commits.
Consider using `pull --rebase` if you prefer to have linear commit history.

Note that we put "abolutely the same" in quotes.
The reason we did that is because the order of lines in the resulting files with resolved conflicts is different.

### Pros
+ Unlimited "undo"
+ Backups: Every developer has a copy of the project
+ Developers can work on different parts of the project

### Cons
- All developers are "local" (because they have to be able to access the "backup" repository)
- Every developer has to resolve conflicts when syncing their work with others.

Because these conflicts have the propensity to become more and more complex as more changes are made by the developers,
such workflow encourages frequent syncing with the shared "backup" directory.


## More developers: GitHub

As other developers join your project, it becomes more and more challenging 
to use local directory for syncing work among them (not to mention
frequent and cumbersome conflicts). Moreover, remote developers can not join a project that is stored on your hard drive.

To allow remote developers contribute to a project,
people use web-based hosting services such as GitHub, BitBucket, GitLab, and others.
They work exactly as our "Local GitHub" directory we created previously.

**Please create an account on GitHub if you don't have one.**

Let's move "local GitHub" repository to the actual GitHub.

- Login to GitHub and create a new (empty) repository.
- Navigate to your local "playgroud" folder and execute:

  ```
  git remote add github https://github.com/your-username/playground.git
  git push github master
  ```

Now, we have a copy of our repository on GitHub and remote collaborators can use it and contribute to the project remotely.

### Exercise

In Collaborator Jones' repository:
 - add a remote called 'github'.
 - Rename `todo.md` to `TODO.md`, add it to the staging area, commit the change.
 - Push to GitHub

In the main "Playground" repository:
 - pull the changes from GitHub



## Git Branches

Now our project is becoming more and more popular so people start to care if we break things.
Therefore, we want to "hide" the development process.
We could work on the project in a separate folder/repository but it would be cumbersome to keep track of folders in such a case.
For that purpose we can use branches!

A branch in Git is a name for a sequence of related commits.
So far, we've dealt with one branch only -- master.
Lets' create a branch:

```
git branch commands
```

We've created a branch but we still belong to `master`.
Let's switch to it:

```
git checkout commands
```

Now, let's create a commit in this new branch:

Let's create a file called `git-commands.md` and record (some or all of) the commands that we've learned so far.

Commit `git-commands.md` to the repository

This file is an example of a "side project" that in real life can be a new feature or something else.

Development in this branch is completely separate from the main `master` branch. Let's see it for ourselves!
Let's see the last three commit in the current (`commands`) branch and compare it to `master` branch.

```
git log --oneline -3
git checkout master
git log --oneline -3
```

These are different!
Moreover, file `git-commands.md` does not exist in this branch!

Let's push it back to GitHub:

```
git push github master commands
```

This time, I'm pushing two branches: `master` and `commands`

So, what do we do once we're happy with the changes in the `commands` branch? Merge it into `master`!

```
git checkout master # Just to be on a safe side
git merge commands
git log --oneline --decorate --graph --all
```

This creates a merge commit, a commit with two parents.

So, whenever we need to work on a complex change, we should create a branch for it!

## Exercise

Create a new branch called `fruits`, switch to this branch  and create a new file called `fruits.md` containing a list of your favorite fruits.
Commit this file to the repository

TODO: Explain "upstream"

## Forks

- Fork your neighbor's repository
- Clone it to your computer
- Make a change in your local copy in a NEW branch
- Commit this change
- Push it back to YOUR fork
- Submit a pull request
