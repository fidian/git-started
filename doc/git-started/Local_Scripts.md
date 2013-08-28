Local Scripts
=============

The `git-started` repository lets you plug in custom scripts and override defaults whenever possible.  You can augment your git repository with the files from this one and gain pretty printing, lint checking and other nice tools that are a part of this development framework.

Another thing that can be done is to extend the processing.  You can add your own scripts in `util/helpers/*` and other locations to have more types of files detected and add additional processing steps that should be executed.

Let's say that you are on a team, working on "The Project" and The Project has `git-started` installed.  On your copy of The Project, you want a certain file changed with every `git pull`, but others do not want this to happen.  You could add a script to `util/git/post-merge.d/`, but then it is part of the repository for The Project and everyone would get it.  Instead, you can add it to `local/util/git/post-merge.d` (creating the directory structure since it likely doesn't exist) and now your script stays out of the repository for The Project and yet it is there for you.
