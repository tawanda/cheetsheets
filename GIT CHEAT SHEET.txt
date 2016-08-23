

# ################## Merge another forks pull request into your branch
http://stackoverflow.com/questions/6022302/how-to-apply-unmerged-upstream-pull-requests-from-other-forks-into-my-fork

#####################################################
REVERTING

#####################################################


----- reverting last merge ----
# get the commit hash of the merge and do this:

git revert -m 1 <commit-hash> 


-------- to a previous comit ------

git revert --no-commit 0766c053..HEAD
git commit
This will revert everything from the HEAD back to the commit hash, meaning it will recreate that commit state in the working tree as if every commit since had been walked back. You can then commit the current tree, and it will create a brand new commit essentially equivalent to the commit you "reverted" to.

(The --no-commit flag lets git revert all the commits at once- otherwise you'll be prompted for a message for each commit in the range, littering your history with unnecessary new commits.)

This is a safe and easy way to rollback to a previous state. No history is destroyed, so it can be used for commits that have already been made public.


------cycle for adding canges-------
git status	  
# check for outstanding changes

git add -A .  
#where the dot stands for the current directory, so everything in and beneath it is added. The -A ensures even file deletions are included.The files listed here are in the Staging Area, and they are not in our repository yet

git commit -m 'comment here'  
#To store our staged changes we run the commit command with a message describing what we've changed

git push	 
OR
git push -u origin master
# The name of our remote is origin and the default local branch name is master. The -u tells Git to remember the parameters, so that next time we can simply run git push and Git will know what to do

------------
git init  #initialises a repo in that directory
git remote add origin https://github.com/try-git/try_git.git #add the destination of your new initialised local git repo

git clone https://github.com/Tawanda-m/cheetsheets.git 
# no need for git init this creates the directory for you


----------undoing stuff-------

git reset <filename> 

# to remove a file or files from the staging area.

git checkout -- octocat.txt

#Files can be changed back to how they were at the last commit by using the command: git checkout -- <target>. 

git rm '*.txt'

#git rm command which will not only remove the actual files from disk, but will also stage the removal of the files for us.

--- remove a previously committed dir----------

The full process would look like this:

$ echo '.idea' >> .gitignore
$ git rm -r --cached .idea
$ git add .gitignore
$ git commit -m '(some message stating you added .idea to ignored entries)'
$ git push


----------if you added the directory to the repo--------

Remove directory from git and local

You could checkout 'master' with both directories;

git rm -r one-of-the-directories
git commit -m "Remove duplicated directory"
git push origin master
Remove directory from git but NOT local

remove this directory from git but not delete it entirely from the filesystem (local)

git rm -r --cached myFolder


******** you need to git add -A afterwards and commit**********


======== Push a new local branch to a remote Git repo and track it too ========


git checkout -b new_branch_name   # this creates a new local branch from the one you are currently on
... edit files, add and commit ...
git push -u origin new_branch_name # this creates the new branch remotley and sets it good


========================== changing remote url ===============
https://help.github.com/articles/changing-a-remote-s-url/


============================= multiple accounts =====


~~~~~~~~~~~~ ~/.ssh/config ~~~~~~~~~~~~~
#Host alias
# HostName bitbucket.org
# IdentityFile ~/.ssh/identity


#Tawanda Account
Host github.com-tawanda
 HostName github.com
 User git
 IdentityFile ~/.ssh/tawanda_rsa

# work
Host work
 HostName bitbucket.org
 IdentityFile ~/.ssh/work_rsa

# Tawanda bitbucket 
Host bitbucket.org
 HostName bitbucket.org
 IdentityFile ~/.ssh/tawandabb
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# And now
Default address
git@bitbucket.org:accountname/reponame.git

Address with alias
git@alias:accountname/reponame.git

# so to clone the work repo

git clone git@work:accountname/reponame.git





-----------------BRANCHING---see also conflict resolution & how to save your branch remotley below-----------------------

git branch gitcheat  	#make the new branch
git branch			 	#shows all the local branches
git checkout gitcheat	
#You can switch branches using the git checkout <branch> command. (all your files will be different and maintained when switching between branches which is super cool)

git add . 				#make chnages and add them
git commit -m 'message text' #commit

git checkout master		#go back to master
git merge gitcheat		#merge changes if conflict read below

git branch -d branchname  #deletes a branch


=========
IF YOU GET A CONFLICT BECAUSE A FIE ON MASTER WILL BE OVERWRITTEN:
=========

You can't merge with local modifications. Git protects you from losing important changes. You have three options. 

1)
One is to commit the change using

git commit -m "My message"


2)
The second is to stash it. stashing acts as a stack, where you can push changes, and you pop them in reverse order.

To stash type:

git stash

git merge branchname 
#then Do the merge 

git stash pop 
# pull the stash, this is like a pull and will(hopefully) merge the conflicts in the file in question

3)
The third options is to discard the local changes using 

git reset --hard.

###########################
By default git stash will not stash files for which there are no history. So if you have files which you have not yet added but which would be overwritten or "created" by the merge, then the merge will still block. In that situation, you can use git stash -u to stash uncommitted files too. Or you can just delete them!
############################

====
---------------HOW TO SAVE YOUR BRANCH ONLINE TO GIT--------
=====

You need:

git push -u origin gitcheat
#origin is the name of your upstream repo url: https://github.com/myname/myrep.git


#Make sure your push.default policy is set properly, then you can just run git push and will push to the branch:

git config --global push.default simple


=================APPENDIX======================================
git init

: Initializes a new Git repository. Until you run this command inside a repository or directory, it’s just a regular folder. Only after you input this does it accept further Git commands.

git config

: Short for “configure,” this is most useful when you’re setting up Git for the first time.

git help

: Forgot a command? Type this into the command line to bring up the 21 most common git commands. You can also be more specific and type “git help init” or another term to figure out how to use and configure a specific git command.

git status

: Check the status of your repository. See which files are inside it, which changes still need to be committed, and which branch of the repository you’re currently working on.

git add

: This does not add new files to your repository. Instead, it brings new files to Git’s attention. After you add files, they’re included in Git’s “snapshots” of the repository.

git commit

: Git’s most important command. After you make any sort of change, you input this in order to take a “snapshot” of the repository. Usually it goes git commit -m “Message here.” The -m indicates that the following section of the command should be read as a message.

git branch

: Working with multiple collaborators and want to make changes on your own? This command will let you build a new branch, or timeline of commits, of changes and file additions that are completely your own. Your title goes after the command. If you wanted a new branch called “cats,” you’d type git branch cats.

git checkout

: Literally allows you to “check out” a repository that you are not currently inside. This is a navigational command that lets you move to the repository you want to check. You can use this command as git checkout master to look at the master branch, or git checkout cats to look at another branch.

git merge

: When you’re done working on a branch, you can merge your changes back to the master branch, which is visible to all collaborators. git merge cats would take all the changes you made to the “cats” branch and add them to the master.

git push

: If you’re working on your local computer, and want your commits to be visible online on GitHub as well, you “push” the changes up to GitHub with this command.

git pull

: If you’re working on your local computer and want the most up-to-date version of your repository to work with, you “pull” the changes down from GitHub with this command.


##### REMOVING BIG FILES FROM GIT

run the following commands in sequence

#Git has a unique SHA that it associates with each object (such as files which it calls blobs) throughout it’s history.
#This helps us find that object and decide whether it’s worth deleting later on:

git rev-list --objects --all | sort -k 2 > allfileshas.txt

#Get the last object SHA for all committed files and sort them in biggest to smallest order:

git gc && git verify-pack -v .git/objects/pack/pack-*.idx | egrep "^\w+ blob\W+[0-9]+ [0-9]+ [0-9]+$" | sort -k 3 -n -r > bigobjects.txt

Take that result and iterate through each line of it to find the SHA, file size in bytes, and real file name (you also need the allfileshas.txt output file from above):

for SHA in `cut -f 1 -d\  < bigobjects.txt`; do
echo $(grep $SHA bigobjects.txt) $(grep $SHA allfileshas.txt) | awk '{print $1,$3,$7}' >> bigtosmall.txt
done;

# Now look in that bigtosmall.txt for the directory that has your largest files


# then clone a fresh copy of your repo in another directory, using the --mirror flag:

git clone --mirror git://example.com/some-big-repo.git

# Next download the big file cleaner in your git repo

wget http://repo1.maven.org/maven2/com/madgag/bfg/1.12.12/bfg-1.12.12.jar -O bfg.jar


# Next delete the folders you want from git, this maches any folder with that name everywhere in the repo
# you can add multiple folders by adding a comma "{folder1,folder2}"

java -jar bfg.jar --delete-folders "{somefolder}" --no-blob-protection some-big-repo.git

cd some-big-repo.git
git reflog expire --expire=now --all
git gc --prune=now --aggressive
git push





