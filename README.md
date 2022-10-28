# Git Basics (commit, diff, branches)

1.echo "1" >> abc.txt
2.Red
3.Red
4.Green
5.echo "2" >> abc.txt
6.Red. Both statuses in 2 and 6 are colored red, but in 2 abc.txt status = untracked, whereas in 6 status = modified
7.git diff main
8.nothing has been staged since the last commit
9.There is no branch /commit /revision / path stage2, hence the error
  fatal: ambiguous argument 'stage2': unknown revision or path not in the working tree.
10.git add abc.txt
11.Git diff prints changes in the working tree NOT YET staged for the next commit. In our case - prints nothing
12.git diff --cached or git diff --staged
13.echo "3" >> abc.txt
14.No. git diff --staged prints changes between staging area and last commit,
       git diff main prints changes between working tree and branch main
15.abc.txt has been changed twice (since the last commit),
   but one change has been staged, the other hasn't (still in working tree)
16.git restore --staged abc.txt and git restore abc.txt

# Resolve conflicts

1.git branch -a
  bugfix/fix_readme_typo
  bugfix/open_kibana_port
  dev
  feature/data_retention_policy
  feature/elasticsearch_helm_chart
  feature/upgrade_angular_version
  feature/version1
  feature/version2
* main
  reset_question
2.git checkout -b feature/lambda_migration
3.git merge feature/version1
Merge made by the 'ort' strategy.
 .env        | 0
 app.py      | 4 ++--
 config.json | 0
 3 files changed, 2 insertions(+), 2 deletions(-)
 create mode 100644 .env
 create mode 100644 config.json
4.Done as shown in the picture
5.Resolve the conflicts as following:
   1.clicked Merge
   2.clicked All to merge all changes for which there is no conflicts
   3.chose Annotate with Git Blame
   4.accepted John Doe's port number (8081), ignored Narayan's port (8082)
   5.accepted the function of Narayan Nadella (get_profile_picture), ignored John's function (load_profile_picture)
6.
John's changes for app.py
use correct lock type in reconnect()
Restrict the extensions that can be disabled
Nayaran's changes for app.py
add new file abc.txt

# Cherry picking

1.git switch main
  git checkout -b feature/lambda_migration2
2.Branch:feature/lambda_migration is shown in Log:feature/lambda_migration
3.
    1.cherry-picked "use correct lock type in reconnect()"
    2.cherry-picked "Restrict the extensions that can be disabled"
4.file .env has been added as a result of "use correct lock type in reconnect()" cherry-picked commit
  file config.json has been added as a result of "Restrict the extensions that can be disabled" cherry-picked commit
5. Cherry-picking order is important. Different commits might modify the same file or development flow might have a
   significance during the development process (one feature depends on another, etc)


# Changes in working tree and switch branches

1.git branch command shows: * feature/lambda_migration2
2.take.txt file creation:
cat > /c/Users/igalv/git_repos/git_ex_sol/take.txt << ENDOFFILE
> line1
> line2
> line3
> line4
> ENDOFFILE
Adding to the index:
git add take.txt
3.error: Your local changes to the following files would be overwritten by checkout:
         take.txt
         2 approaches: commit your changes or stash them before you switch branches.
4.Force Checkout dev
5.git status
On branch dev
nothing to commit, working tree clean

There are no staged changes on dev,
opening take.txt file looks nothing like take.txt on branch feature/lambda_migration2 :
cat take.txt
a
b
c

6.on branch feature/lambda_migration2 take.txt file doesn't exist anymore - checkout without committing or stashing
discards any untracked or staged file

# Reset

1.Checked out reset_question branch
2.
   1.git reset changes where the current branch is pointing to (HEAD)
     Removes the last commit from the commit history and sets working tree to the state just before the last commit.
     HEAD was pointing to 10.txt file, now HEAD is pointing to 9.txt file;
     10.txt file is staged, but yet to be committed:

     git status
     On branch reset_question
     Changes to be committed:
     (use "git restore --staged <file>..." to unstage)
        new file:   10.txt

   2.All the changes that were committed won’t be seen in staging area, but they are in the working directory.
     HEAD was pointing to 9.txt file, now HEAD is pointing to 8.txt file;
     10.txt file is untracked
     9.txt file is untracked

     git status
     On branch reset_question
     Untracked files:
     (use "git add <file>..." to include in what will be committed)
        10.txt
        9.txt
   3.--hard flag modifies HEAD, index and working directory:
     all the changes will be removed from the local git repository, staging area and working directory
     HEAD, index and working directory all will have the same version of files

     HEAD is pointing to 7.txt file, 8.txt file is gone (removed from local repo, staging area and working directory)

     git status
     On branch reset_question
     Untracked files:
     (use "git add <file>..." to include in what will be committed)
        10.txt
        9.txt

   4.Reverting (on a public branch) undoes a commit by creating a new commit.
     This is a safe way to undo changes, as it has no chance of re-writing the commit history.
     In our case:

     git revert HEAD~1
     [reset_question fee3d21] Revert "6"
     1 file changed, 1 deletion(-)
     delete mode 100644 6.txt

3.HEAD is a pointer or a reference to the last commit in the current branch.
  HEAD~1 would mean behind 1 commit from HEAD


# Working with GitHub

1.https://github.com/igalvex/git_ex_repo
2.git remote add origin https://github.com/igalvex/git_ex_repo.git
  git remote -v
  origin  https://github.com/igalvex/git_ex_repo.git (fetch)
  origin  https://github.com/igalvex/git_ex_repo.git (push)

3.first, pulling changes from the remote branch and integrating them with my local main branch.:
  git pull origin main --allow-unrelated-histories

  pushing local branch main to GitHub branch main:
  git push --set-upstream origin main

  git switch dev
  pushing local branch dev to GitHub branch dev:
  git push origin dev:dev

4.https://github.com/igalvex/git_ex_repo
