# Common Local Git Workflow


## Useful commands
| Command                       | Notes  |
| ----------------------------- | ----- |
| git init |	Initializes a local git repo |
| git status | Shows the working tree status.  Shows files that are staged for commit (are in the index), as well as files that you could commit if you first ran git add.  Those changes exist in the working directory, but not in the index. |
| git add | Adds the files to the index - stages the changes |
| git commit | Commits changes in the index to the respository |
| git log | Shows the git commit logs<br /><br />--oneline - shows the sha and the commit comments, one line per commit |
| git diff | Shows changes between commits, commits and index, index and working directory<br /><br />git diff --cached - shows changes between the index and the repository<br />git diff - shows changes between the working area and the index |
| git checkout | Can be used to switch branches or restore working tree files<br /><br />git checkout HEAD \<file\> - Because you are already on HEAD, this version of the command will update the index and working area with the version of the file that is in the repository.
| git reset | Moves the current branch to a different commit.  If you do not pass a sha for another commit, the command will, in effect, not move the current branch<br /><br />--soft - does not update the files in the working area or the index with the files in the repository<br />--mixed - copies the file(s) in the repository into the index.  It does not update the working directory.<br />--hard would update the working directory and index, however, you cannot pass a path (like <span>chapter.md</span>).  --hard only works on all changes. |

## Useful Concepts / Learnings
| Learning                       |
| ------------------------------ |
| There are 4 main areas in git:<br/>    - Working directory: This is where you work with (add, edit, delete) files<br/>    - Index: A staging area where you stage changes before committing them to the Repository<br/>    - Repository: Stores all the objects<br/>    - Stash: A place to temporarily store changes |

## Kata
| Objective | Command | Notes |
| ----------------------------- | ----------------------------- | ------------------------------------------------- | 
| <b>Add/Commit </b>|||
| Create the test files by running the setup script | ./setup.sh | This will initialize a git repo, create <span>index.md</span> at the root and a chapters directory with one file, <span>chapter1.md<span> |
| cd into the kata directory | cd kata ||
| See the current git status | git status | Shows that there are untracked files.  This means that they are in the Working Directory, but not the Index or the Repo |
| Stage the new files | git add * | Add the files to the index |
| Commit the changes | git commit -m "Initial Commit" | Commit the changes to the repo |
| | | |
| Show the git log | git log | This shows the single commit |
| Show that the working directory and the index are the same | git diff | The working area and the index are the same |
| Show that the index and the repository are the same | git diff --cached | The index and the repository are the same |
| Make changes to the <span>index.md</span> file.  Add a heading to the top of the file such as \# Chapter List and save it. | vi <span>index.md</span>  | You can use any editor you like to edit the file, not just vi |	
| Show that the difference between the working directory and the index | git diff | The working area is different than the index.  The working area has the additions you made.  My diff looks like this: <br/><br/>diff --git a/index.md b/index.md<br/>index e031062..4a95f74 100644<br/>--- a/index.md<br/>+++ b/index.md<br/>@@ -1 +1,2 @@<br/>+# Chapters<br/>\[Chapter 1\]\(chapters/chapter1.md\) |
| Show that the index and the repository are still the same | git diff --cached | The index and the repository are the same |
| Show the git status | git status| We have an untracked file - this means that it is not yet in the index (staging area) |
| Add the updated file into the staging area | git add <span>index.md</span> | This adds <span>index.md</span> to the index |
| Show that the working directory and the index are the same | git diff | The working area and the index are the same |
| Show that the index has changes that are not yet in the repository | git diff --cached | The index and the repository are different.  The index has changes that are staged, but not yet committed. |
| Commit the staged changes | git commit -m "Added a header" | This updates the repo with the changes in the index |
| Show that the working directory and the index are the same | git diff | The working area and the index are the same |
| Show that the index and the repository are the same | git diff --cached | The index and the repository are the same |
|&nbsp;|||
| <b>Revert Changes That Exist Only in the Working Directory </b>|||
| Make changes to the <span>index.md</span> file.  Add a description beneath the \# Chapters heading: The following are the chapters: | vi <span>index.md</span> | You can use any editor you would like: Notepad, VSCode, etc. |
| Show the git status | git status | This shows that there is a change that has not been staged (not in the index).<br/>On branch master<br/>Changes not staged for commit:<br/>&nbsp;&nbsp;(use "git add <file>..." to update what will be committed)<br/>&nbsp;&nbsp;(use "git checkout -- <file>..." to discard changes in working directory)<br/><br/>&nbsp;&nbsp;&nbsp;&nbsp;modified:   index.md<br/><br/>no changes added to commit (use "git add" and/or "git commit -a") |
| Throw away the change you just made to the file in the working directory | git checkout HEAD <span>index.md</span> | This command will overwrite the index and working directory with the <span>index.md</span> that is in the repository. |
| Show the git status | git status | There are no longer changes in the working directory.<br/><br/>On branch master<br/>nothing to commit, working tree clean |
|&nbsp;|||
| <b>Revert Index/Staging Area Changes - Leave Changes in Working Area </b>|||
| Make changes to the <span>index.md</span> file again (the same changes are fine). | vi <span>index.md</span> | You can use any editor you would like: Notepad, VSCode, etc. |
| Add the change to the index | git add <span>index.md</span> | Add the changed <span>index.md</span> to the index |
| Show the git status | git status | This shows the one change to be committed (<span>index.md</span> is in the index)<br/><br/>On branch master<br/>Changes to be committed:<br/>&nbsp;&nbsp;(use "git reset HEAD <file>..." to unstage)<br/><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modified:   <span>index.md</span> | 
| Unstage <span>index.md</span>, but leave the changes in the working directory | git reset <span>index.md</span> | git reset uses the --mixed option by default.  --mixed copies the file(s) in the repository into the index.  It does not update the working directory.<br/>--hard would update the working directory, however, you cannot pass a path (like <span>index.md</span>).  --hard only works on all changes. |
| Show the git status | git status | This shows that there is one change that is not staged for commit.  In other words, the changes are in the working directory and not in the index.<br/><br/>On branch master<br/>Changes not staged for commit:<br/>&nbsp;&nbsp;(use "git add <file>..." to update what will be committed)<br/>&nbsp;(use "git checkout -- <file>..." to discard changes in working directory)<br/><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modified:   <span>index.md</span> |
| Show the difference between the working directory and the index | git diff | This shows the difference between what is in the Working directory (the updated <span>index.md</span>) and the index. |
|&nbsp;|||
| <b>Revert Index/Staging Area Changes - Revert Changes in Working Area </b>|||
| Add the change to the index again | git add <span>index.md</span> | Add the changed <span>index.md</span> to the index |
| Show the git status | git status | This shows the one change to be committed (<span>index.md</span> is in the index) |
| Unstage <span>index.md</span> and remove the changes from the working directory | git reset --hard<br/>or<br/>git checkout HEAD <span>index.md</span> | Git reset --hard does not allow you to pass in a path.  It will reset the entire index and working directory with the contents of the repository<br/><br/>You can use git checkout HEAD and pass the path | 
| Show the git status | git status | This shows that the working directory, the index and the repository are all the same. |
|&nbsp;|||
| <b>Revert Changes That Were Committed / Revert to an Earlier Commit </b>|||
| Make changes to the <span>index.md</span> file again (the same changes are fine). | vi <span>index.md</span> | You can use any editor you would like: Notepad, VSCode, etc. |
| Add the change to the index | git add <span>index.md</span> | Add the changed <span>index.md</span> to the index |
| Commit the staged changes | git commit -m "Added a description to the index page" | This updates the repo with the changes in the index |
| Show the git log - just 1 line per commit | git log --oneline | The following are the results:<br/><br/>1279007 (HEAD -> master) Added a description to the index page<br/>47f0462 Added a header<br/>d3d1229 Initial Commit |
| Throw away the changes from the last commit (Added a description to the ndex page) in the repository.  However, keep the changes in the index and the working directory. | git reset --soft \<commit sha to reset to\> | git reset --soft 47f0462.  This will move the branch pointer to the referenced commit (Added a header). |
| Show the git log - just 1 line per commit | git log --oneline | Results:<br/><br/>47f0462 (HEAD -> master) Added a header<br/>d3d1229 Initial Commit |
| Show the git status | git status | This shows that there is a change to <span>index.md</span> that is staged to commit.  This means that the changes to the working directory and index were not overwritten. |
| Throw away the changes in the repository and index | git checkout HEAD <span>index.md</span> | This will overwrite <span>index.md</span> in the index and the repository with the <span>index.md</span> in the repository |
| Show the git status | git status | This shows that there are not changes.  The working directory, the index and the repository are all the same. |
