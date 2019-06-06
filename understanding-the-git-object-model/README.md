# Understanding the Git Object Model
This kata is derived from [How Git Works]("https://www.pluralsight.com/courses/how-git-works") on Pluralsight by Paolo Perrotta.  In that training, Paolo clearly illustrates the Git object model and how Git works behind the scenes.  This kata will help reinforce the learnings about the Git Object Model.

## Useful commands
| Command                       | Notes  |
| ----------------------------- | ----- |
| echo "Apple Pie" \| git hash-object --stdin | This provides you with the SHA1 hash of the object. |
| git cat-file -t <SHA1> | Tells you the type of the object - blob, tree, etc |
| git cat-file -p <SHA1> | Pretty prints the content of the object |
| git init | Initializes a git repo |
| git add * | Adds file(s) to the index (staging area) |
| git commit -m "message" | Commits file(s) in the index to the repository | 
| git log | Show the git commit history |

## Useful Concepts / Learnings
| Learning                       |
| ------------------------------ |
| There are 4 main object types in git:<br/>&nbsp;&nbsp;- blobs: Contain data<br/>&nbsp;&nbsp;- trees: Contain pointers to other trees and blobs<br/>&nbsp;&nbsp;- commits: Contain pointers to a tree, along with metadata (who, comments)<br/>&nbsp;&nbsp;- tags: A label for an object.  Has a pointer to an object and the label (this is not covered in this kata) |
| Every object in git has a unique SHA1 hash |
| A commit points to a single tree, the root tree of that particular commit<br/>Trees point to:<br/>&nbsp;&nbsp;- Other tree object(s)<br/>&nbsp;&nbsp;- Blob object(s) |
| Branches are pointers to commits |
| HEAD points to a branch |
| There are 4 main areas in git:<br/>&nbsp;&nbsp;- Working directory: This is where you work with (add, edit, delete) files<br/>&nbsp;&nbsp;- Index: A staging area where you stage changes before committing them to the Repository<br/>&nbsp;&nbsp;- Repository: Stores all the objects<br/>&nbsp;&nbsp;- Stash: A place to temporarily store changes |

## Kata
| Objective | Command | Notes |
| ----------------------------- | ----------------------------- | ------------------------------------------------- | 
| <b>Understand the Object Model </b> |||
| Create the test files by running the setup script | ./setup.sh | This will initialize a git repo, create index.md at the root and a chapters directory with one file, chapter1.md |
| cd into the kata directory | cd kata ||
| Show that the git objects database does not have any files | ls -lt .git/objects/ | There are 2 directories, but no objects |
| Show the git status | git status | This shows that there are 2 untracked files.  The files are in the working directory, but not in the index. |
| Perform the initial commit | git add * <br/>git commit -m "Initial Commit" ||
| Show the objects in the .git objects database | ls -lt .git/objects/ | There are now 5 new objects added.  The objects are stored in directories that start with the first 2 characters of the objects sha1.  The 5 objects are: 1 Commit, 2 Trees (the root and the chapters direcotry) and 2 files.  We will explore these in more detail in a moment. |
| Show the git commit history | git log | You will see the 1 commit you just made.  On my machine the output looks something like this:<br/>commit 9fd535694a85ab312c7919d3421241a6bb643e46 (HEAD -> master)<br/>Author: Rob Bagby <rob@robbagby.com><br/>Date:   Wed May 29 13:20:40 2019 -0700<br/><br/>&nbsp;&nbsp;&nbsp;Initial Commit
| Show the content of the first commit | git cat-file -p \<commit sha\> | On my machine, I ran:<br/>git cat-file -p 9fd535694a85ab312c7919d3421241a6bb643e46<br/><br/>tree 0933ed499b266f596e52ad24dc2a3a15ff2ae8f6<br/>author Rob Bagby <rob@robbagby.com> 1559840863 -0700<br/>committer Rob Bagby <rob@robbagby.com> 1559840863 -0700<br/><br/>Initial Commit<br/><br/>You can see from the output above, the first commit points to a single tree, which is the root of this commit
| Show the content of the tree object that the commit is pointing to | git cat-file -p \<tree sha\> | On my machine, I ran:<br/>git cat-file -p 0933ed499b266f596e52ad24dc2a3a15ff2ae8f6<br/><br/>040000 tree e68b491f4b9db667647987cfa879724f0986e8e3	chapters<br/>100644 blob e031062f4cf45cf2990aa450a7eb8b08dfe0f932	index.md<br/><br/>You can see that the root tree contains the sha to:<br/>&nbsp;&nbsp;- The blob for index.md<br/>&nbsp;&nbsp;- The tree for the chapters directory |
| Show the git objects again | ls -lt .git/objects/ | The following are the results on my machine:<br/><br/>total 0<br/>drwxr-xr-x  3 robbagby  staff  96 Jun  6 10:07 09<br/>drwxr-xr-x  3 robbagby  staff  96 Jun  6 10:07 9f<br/>drwxr-xr-x  3 robbagby  staff  96 Jun  6 10:07 e6<br/>drwxr-xr-x  3 robbagby  staff  96 Jun  6 10:07 e0<br/>drwxr-xr-x  3 robbagby  staff  96 Jun  6 10:07 e1<br/>drwxr-xr-x  2 robbagby  staff  64 Jun  6 10:05 info<br/>drwxr-xr-x  2 robbagby  staff  64 Jun  6 10:05 pack<br/><br/>You can see that there are several directories that start with 2 characters.  Git stores the objects in directories whose names start with the first two characters of the object sha |
| Show the contents of the directory that contains the object representing <span>index.md</span> | ls -lt .git/objects/\<first_2_letters_of_the_sha_for_index.md\> | On my machine, I ran: <br/>ls -lt .git/objects/e0<br/><br/>total 8<br/>-r--r--r--  1 robbagby  staff  40 Jun  6 10:07 31062f4cf45cf2990aa450a7eb8b08dfe0f932<br/><br/>Notice that the file name is the sha1 of <span>index.md</span> without the first 2 characters |
| Show the contents of the object representing <span>index.md</span> | git cat-file -p \<sha_for_index.md_object\> | On my machine, I ran:<br/>git cat-file -p e031062f4cf45cf2990aa450a7eb8b08dfe0f932 <br/><br/><span>\[Chapter 1\]\(chapters/chapter1.md\)</span> <br/><br/>This is a markdown link to <span>chapters/chapter1.md</span> |
|&nbsp;|||
| <b>Understand Branches </b>|||
| Show the branches | git branch | Although we have not created any branches, git created one by default named master<br/><br/>git branch<br/><br/>* master |
| Show where the branches are stored | ls -lt .git/refs/heads | ls -lt .git/refs/heads<br/><br/>total 8<br/>-rw-r--r--  1 robbagby  staff  41 Jun  6 10:07 master<br/><br/>Git stores the branches in .git/refs/heads.  There is a file for each branch |
| Show the content of the branch file | cat .git/refs/heads/master | cat .git/refs/heads/master<br/><br/>9fd535694a85ab312c7919d3421241a6bb643e46<br/><br/>The master holds a pointer to the first commit.  You can prove this by running git log to see the sha1 of the first commit.  So, as you can see, branches are merely references to commits. |
| Create a new branch | git branch feature1 ||
| Show how branches are stored in .git | ls -lt .git/refs/heads | ls -lt .git/refs/heads<br/><br/>total 16<br/>-rw-r--r--  1 robbagby  staff  41 Jun  6 10:40 feature1<br/>-rw-r--r--  1 robbagby  staff  41 Jun  6 10:07 master |
| Show the content of the feature1 branch file | cat .git/refs/heads/feature1 | cat .git/refs/heads/lisa<br/><br/>9fd535694a85ab312c7919d3421241a6bb643e46<br/><br/>Note that this branch has the same pointer as master |
| Show the branches | git branch | git branch<br/><br/>&nbsp;&nbsp;&nbsp;feature1<br/>*&nbsp;&nbsp;master <br/><br/>There are now 2 branches.  The asterisk indicates that the current branch is 'master' |
|&nbsp;|||
| <b>Understand HEAD </b>|||
| Show how HEAD is stored in git | cat .git/HEAD | cat .git/HEAD<br/><br/>ref: refs/heads/master<br/><br/>The HEAD file has a pointer to a single branch in the refs/heads folder |
| Checkout the feature1 branch | git checkout feature1 | git checkout feature1 <br/><br/>Switched to branch 'feature1' |
| Show how HEAD is stored in git | cat .git/HEAD | cat .git/HEAD <br/><br/>ref: refs/heads/feature1<br/><br/>The HEAD file now has a pointer to the feature1 branch |
