# Testing stuff with Git, especially branching

First, I want to solve issue when merge branches.

## First commit

> echo "Initial commit" > file1.txt
>
> [master] git init
>
> [master] git add -A
>
> [master] git commit -m "first commit"
>
> [master] git remote add origin https://github.com/dleurs/github-test-merging.git
>
> [master] git push -u origin master

# Easy merging, no change in master

## Second commit, feature that will change

> [master] git checkout -b "feature_1"
>
> [feature_1] echo "Feature 1" >> file2.txt 
>
> [feature_1] git add -A; git commit -m "Feature 1"; git push origin feature_1;

## Merging

> [feature_1] git merge master<br/>  
> Already up to date.
>
> [feature_1] git checkout master
>
> [master] git merge feature_1<br/>
> Updating a9cd07b..a5ac54e<br/>
> Fast-forward<br/>
> file2.txt | 1 +<br/>
> 1 file changed, 1 insertion(+)<br/>
> create mode 100644 file2.txt<br/>
>
> [master] git add -A; git commit -m "Merged feature_1"; git push origin master; 
>On branch master<br/>
>Your branch is ahead of 'origin/master' by 1 commit.<br/>
>  (use "git push" to publish your local commits)<br/>
>
>nothing to commit, working tree clean<br/>
>Total 0 (delta 0), reused 0 (delta 0)<br/>
>To https://github.com/dleurs/github-test-merging.git<br/>

# Harder merging, changes in master

> [master] git checkout -b "feature_2"
>
> [feature_2] echo "Flavour feature 2" > file3.txt 
>
> [feature_2] git add -A; git commit -m "Flavour feature 2"; git push origin feature_2;
>
> [feature_2] git checkout master
>
> [master] echo "Flavour master feature 2" > file3.txt 
>
> [master] git add -A; git commit -m "Flavour master feature 2"; git push origin master;

## Now trying to merge this, they will be a conflict

> [master] git checkout feature_2
>
> [feature_2] git merge master<br/>
> Auto-merging file3.txt<br/>
> CONFLICT (add/add): Merge conflict in file3.txt<br/>
> Automatic merge failed; fix conflicts and then commit the result.<br/>
>
> [feature_2] git status<br/>
> On branch feature_2<br/>
> You have unmerged paths.<br/>
>  (fix conflicts and run "git commit")<br/>
>  (use "git merge --abort" to abort the merge)<br/>
>
>Unmerged paths:<br/>
>  (use "git add <file>..." to mark resolution)<br/>
>
>	both added:      file3.txt<br/>
>
>no changes added to commit (use "git add" and/or "git commit -a")<br/>
>

## To solve the conflict, use VSCode

> In VSCode, file3.txt, Accept current change or incoming change, here current change and save
>
> git status<br/>

> On branch feature_2<br/>
>Your branch is up to date with 'origin/feature_2'.<br/>
>
>You have unmerged paths.<br/>
>  (fix conflicts and run "git commit")<br/>
>  (use "git merge --abort" to abort the merge)<br/>
>
>Unmerged paths:<br/>
>  (use "git add <file>..." to mark resolution)<br/>
>
>	both added:      file3.txt<br/>

> git add -A; git commit -m "Merge conflict solved, accept current change";<br/>
> [feature_2 485a42e] Merge conflict solved, accept current change<br/>
> 
> git push origin feature_2
>
> git checkout master
>
> git merge feature_2<br/>
>Updating c6d11e8..485a42e<br/>
>Fast-forward<br/>
> file3.txt | 2 +-<br/>
> 1 file changed, 1 insertion(+), 1 deletion(-)<br/>
>
> no need to git add, git commit. Because :
>git status<br/>
>On branch master<br/>
>Your branch is ahead of 'origin/master' by 2 commits.<br/>
>  (use "git push" to publish your local commits)<br/>
>
> git push origin master

> If you do a mistake, delete folder, git clone<br/>
> But no branches anymore ? git branch -a; git checkout remotes/> origin/feature_2; git checkout feature_2;<br/>

## Go back into a commit :

> [master] echo "A huge error in the code" > file3.txt 
> 
> git add -A; git commit -m "Testing (you don't know but containing a bug)"; git push origin master;<br/>
>
> "You discover the bug !!"<br/>
>
> git log 
> 
>commit 810fba759db7423b7362b83db4320a1db7563c5c (HEAD -> master, origin/master, origin/HEAD)<br/>
>Author: Leurs Dimitri <dimitri.leurs@gmail.com><br/>
>Date:   Tue Mar 3 10:01:42 2020 +0100<br/>
>
>    Testing (you don't know but containing a bug)<br/>
>
>commit 485a42ebe7dd5c387c7418ca3ecada5e937aba07 (origin/feature_2, feature_2)<br/>
>Merge: 9478a79 c6d11e8<br/>
>Author: Leurs Dimitri <dimitri.leurs@gmail.com><br/>
>Date:   Tue Mar 3 09:56:11 2020 +0100v<br/>
>
>    Merge conflict solved, accept current change<br/>
>
>commit c6d11e8c22816f8f872142fda7d51f1a23845312<br/>
>Author: Leurs Dimitri <dimitri.leurs@nokia.com><br/>
>Date:   Mon Mar 2 16:25:59 2020 +0100<br/>
>
>    Flavour master feature 2<br/>
>
> NOT WORKING : git branch -f master 485a42ebe7dd5c387c74<br/>
> fatal: Cannot force update the current branch.<br/>
>
> NOT WORKING EITHER : git reset 485a42ebe7dd5c387c7<br/> 
> OR git reset HEAD~1<br/>
>Unstaged changes after reset:<br/>
> M	file3.txt<br/>
> 
> git status<br/>
>On branch master<br/>
>Your branch is behind 'origin/master' by 1 commit, and can be fast-forwarded.<br/>
>  (use "git pull" to update your local branch)<br/>
>
>Changes not staged for commit:<br/>
>  (use "git add <file>..." to update what will be committed)<br/>
>  (use "git checkout -- <file>..." to discard changes in working directory)<br/>
> 
> git push origin master <br/>
>To https://github.com/dleurs/github-test-merging.git <br/>
> ! [rejected]        master -> master (non-fast-forward) <br/>
>error: failed to push some refs to 'https://github.com/dleurs/github-test-merging.git' <br/>
>hint: Updates were rejected because the tip of your current branch is behind<br/>
>hint: its remote counterpart. Integrate the remote changes (e.g. <br/>
>hint: 'git pull ...') before pushing again. <br/>
>hint: See the 'Note about fast-forwards' in 'git push --help' for details.<br/>
>
> The solution :
>
> git log; Getting commit id of two last commit
>
> git diff N N-1 > diff.txt<br/>
> git diff 810fba759db7423b73 485a42ebe7dd5c387c7418c > diff.txt
>
> git apply diff.txt 
>
> git add -A; git commit -m "Bug detected, going back one commit behind, with diff.txt"; git push origin master
>
> rf -rf diff.txt
>
> git add -A; git commit -m "Deleting diff.txt, code just like commit 485a42ebe7dd5c3"; git push origin master

