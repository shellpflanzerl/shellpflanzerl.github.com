Title: Create A Empty git Branch
Date: 2014-12-03 22:00
Authors: Georg Vogetseder
Summary: Needed it for pelican :)

If you want to create a new branch without any commit history:

    :::bash
    git checkout --orphan <branch_name>
    git rm -r .
    
The `git rm -r` removes the files copied from the origin branch.
