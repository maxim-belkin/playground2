## Local "GitHub"

Unfortunately, version control can not prevent accidental deletion of repositories.
To prevent such accidental loses we need backups. Let's discuss how we can do that on our own machine.

Solution 1. Periodically save it somewhere safe.

Solution 2. Save the repository (`.git` directory) with Git:

1. Create an empty directory where we will save the repository and navigate to it
    ```
    mkdir -p ~/backups/my-repository.git
    cd ~/backups/my-repository.git
    ```
2. Initialize a bare repository.
   ```
   git init --bare
   ```
   Bare repositories don't have workding directories and 
   is what GitHub / GitLab / BitBucket actually store.
3. Navigate back to the repository and push to the newly created bare repository.
   Yes, we can copy our repository by pushing to the backup location.
   When we do so, we have to:
    - use full (absolute) path to the backup folder
    - prefix this full path with `file://`.
  Because of these two requirements, the "URL" will look like so: `file:///Path/to/backup`
   ```
   cd /path/to/repo
   git push file:///Users/username/backups/my-repository.git master:master
   ```
   Here we're pushing our local branch `master` (the left "master" in `master:master`) to the remote branch `master` in specified repository.
   When pushing branch to a branch with the same name such as `master:master`, we can simply say `master`.
   ```
   git push file:///Users/username/backups/my-repository.git master
   ```
   Nice! This is much better! However, Git still requires us to say which branch we are pushing.
   Wouldn't it be nice to simplify this? Absolutely! Let's do this!
4. For convenience, let's first create a shortcut so that we don't have to enter the full path to the repository each time.
   For that purpose we can create a remote called 'backup':
   ```
   git remote add backup file:///Users/username/backups/my-repository.git
   ```
5. Now, let's instruct Git to give us a hint when our repo is to far ahead of the backup.
   To see the effect, execute `git status` before and after the following command:
   ```
   git branch -u backup/master
   ```
   The above command sets an "upstream" branch for the current branch.
   An upstream branch is the branch that Git will compare current branch against and report if they are in sync or have diverged.

   Now, we can back up our repository with:
   ```
   git push backup
   ```

