## Local back up

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
   git push file:///Users/username/backups/my-repository.git
   ```
4. (Optional) For convenience, let's create a remote called 'backup' to easily back up our repository:
   ```
   git remote add backup file:///Users/username/backups/my-repository.git
   git push backup
   ```
5. (Optional) Instruct Git to give us a hint when our repo is to far ahead of the backup.
   To see the effect, execute `git status` before and after the following command:
   ```
   git branch -u backup/master
   ```
