Basic Git Commands
==================

Installing Git:

.. code-block:: bash

    sudo apt install git


Global setup:

.. code-block:: bash

    git config --global user.name "Nicolas DEJAX"
    git config --global user.email "nicolas.dejax@live.fr"


Push an existing folder to a newly created empty repository:


.. code-block:: bash

    cd existing_folder
    git init --initial-branch=main
    git remote add origin git@gitlab.com:doudou/my_project.git
    git add .
    git commit -m "Initial commit"
    git push -u origin main


Get the list of commits
#######################

.. code-block:: bash

    git log --oneline # --all for more intermediate garbage commits
    

Undo uncommitted changes
########################

.. code-block:: bash

    # Safe: Back to the last local commit
    git stash -u
    # NOT Safe: Back to the last local commit
    git reset --hard HEAD
    # Back to the last remote commit
    git reset --hard origin/main

.. note::
    :code:`git reset` does not remove new created files (untracked file), you can run :code:`git clean -f -d` to also get rid of untracked files.


Undo a git add command
######################

.. code-block:: bash

    # Cancel all added changes
    git reset --mixed
    # Remove a specific file
    git reset specific_file.txt
    # or
    git restore --staged specific_file.txt


Cancel 1 or more last commit(s)
###############################

.. code-block:: bash

    # Commit not published yet: all changes are lost
    git reset --hard 0d1d7fc32

    # Commit already published: cancel commit(s) with a new commit
    # Last 2 commits
    git revert HEAD~2..HEAD
    # OR
    git revert a867b4af 25eee4ca

Source: https://stackoverflow.com/a/4114122/10109560


Create a new branch, make changes & merge it
############################################

What we want to do here:

#. Create a branch "branch_A_ft1" from "branch_A"
#. Do some modification on "branch_A_ft1"
#. Push the the branch "branch_A_ft1" on remote
#. Merge the branch "branch_A_ft1" in "branch_A"
#. Delete the feature branch if necessary on local and remote

.. code-block:: bash

    # Let's say we are currently on branch main, and there is an existing branch called "branch_A"
    
    # Create a branch "branch_A_ft1" from "branch_A"
    git checkout -b branch_A_ft1 branch_A
    
    # Do some modification on "branch_A_ft1"
    echo "New file Content" > file.txt
    git add file.txt
    git commit -m "New File"
    
    # Push the the branch "branch_A_ft1" on remote
    # ignore this step if you want to keep the branch local
    git push -u origin branch_A_ft1

    # Merge the branch "branch_A_ft1" in "branch_A"
    git checkout branch_A
    git merge branch_A_ft6

    # Delete the feature branch fro local and remote repo
    git branch -d branch_A_ft1
    git push origin -d branch_A_ft1


------------------------------------------------------------

**Sources**:

- Back to last commit: https://stackoverflow.com/questions/9335486/git-how-to-roll-back-to-last-push-commit
- Removing untrack files: https://stackoverflow.com/a/4327720/10109560
- Cancel commits: https://stackoverflow.com/a/4114122/10109560

