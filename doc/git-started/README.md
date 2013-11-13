Git Started - A Boilerplate for Git
===================================

Are you tired of setting up pretty printers and lint checkers for your project, over and over?  Me too.  You can now merge this project into yours and it will set up git hooks that do this sort of thing for you.  In case your files are not covered by our scripts, the code is extensible and you merely create additional files in the right places to enable support for additional file types and coding styles.

It currently works really well for Linux projects.  I also have it working natively on Mac (OS X) and would happily accept patches for Cygwin and other platforms.


Submodule vs. Merged
====================

You can include all of the benefits of this toolset either merged into your repository or as a submodule.  There are distinct advantages to each:

 * When used as a submodule, your code has a clean separation from the commits from this project.
 * Since these tools become part of your tooling, merging the code may make sense.
 * Submodules rely on the upstream repository existing when you initialize a copy of your repository.
 * Merged code is ready to go immediately; you only need to clone and run one script to set up your environment.
 * Sharing your custom modifications back to this repository as a pull request is a lot easier when used as a submodule.
 * There are several directories that exist in this project that are designed to start structuring your software in a good way, but these only help if you merge the commits.
 * A submodule lets you put these git hooks and code anywhere in your project you would like.
 * There is more setup involved with a submodule in order to let you customize the scripts.
 * Code review tools will audit the git-started commits as though they were your own when merging in the code.


How To Include In Your Project
==============================

You need to use git as your version control system.  These scripts work on Linux, Mac and possibly other Unix flavors.  It may work with cygwin on Windows (or similar), but that's currently untested.  The scripts rely heavily on bash and several tools that are typically found in the `coreutils` package.

Next, decide if you want to merge this repository or use this as a submodule.  If you are unsure, try the submodule route first and see if that causes too many problems for you.  Follow the steps in that section, then come back here.

Finally, _do not clone this repository!_  Cloning is great if you want to work on this project, but do not clone if you wish to just add the functionality into your codebase.  You want to augment your repository with these tools, not base all of your work off this code.  If you clone this repository, your `origin` will be set to be this project.  While you can still change the setting, it is not what you desire.

Next, follow the section that suits your situation.


Including as a Submodule (Recommended)
--------------------------------------

First, go to the [GitHub project] and fork it.  This way you can later customize some of the scripts and still get all of the benefits of this repository and updates.

    # Start off in your repository
    cd your_repository

    # Add your fork as a submodule - we are putting the files in
    # the hidden directory .hooks to hide them
    git submodule add https://github.com/fidian/git-started.git .hooks

    # Commit
    git commit -m 'Adding git-started'

Updates from here are pretty easy.  First you need to merge commits into your fork of the project, then you update the submodule.

    # I put the submodule into your_repository/.hooks
    cd your_repository/.hooks

    # Add the remote, in case you did not have this yet
    git remote add git-started https://github.com/fidian/git-started.git

    # Merge in upstream changes
    git fetch git-started
    git merge git-started/master

    # Push out to your fork
    git push

You can also safely add additonal changes to your fork.


Including by Merging
--------------------

This adds the commit history of this project into your repository as though the edits were made directly in your system.

    # Start off in your git repository
    cd your_repository
    
    # It is best to work in branches so you can undo things easier
    git checkout -b git-started-branch
    
    # Add the git-started repository as a remote
    git remote add git-started https://github.com/fidian/git-started.git
    
    # Get the commit history from git-started
    git fetch git-started
    
    # Merge git-started's master into your working branch
    git merge git-started/master
    
    # Handle merge conflicts and finish the commit if you have problems
    # ... work work work ...
    
    # Then you are ready to merge git-started-branch into your repository
    git checkout master
    git merge git-started-branch
    git branch -d git-started-branch
    git pull
    git push

Upgrades are essentially the same thing.  Here's the abbreviated version.

    cd your_repository
    git checkout -b git-started-branch
    git fetch git-started
    # If that fails, use "git remote add" and then fetch again
    git merge git-started/master
    # Handle merge conflicts here
    git checkout master
    git merge git-started-branch
    git branch -d git-started-branch
    git pull
    git push


Setting Up The Hooks
====================

In order to see any benefit at all, you *must* set up the repository.

    # When merged
    util/bin/setup_repository

    # As submodule in .hooks directory
    .hooks/util/bin/setup_repository

By default, this will do the following things:

 * Download and install composer if you have a `composer.json` file.  Composer will be saved as `vendor/composer.phar`.
 * Rebuild tags/CTAGS if either of those files exist.
 * Setup some files that should not be committed by overriding `.git/exclude`.
 * Initialize and update any git submodules.
 * Set up the use of the git hooks so you can plug in additional scripts.
 * Run `npm install` if you have a `package.json` file.


Customization
=============

This repository is intended to augment yours and to get some tedious tasks out of the way.  Its goal is to let you extend its functionality quickly and easily.  You can tie into many different points in order to configure, extend and override actions that are performed.

We intend to let you extend everything without having to modify any of the files in this repository.  That way you can follow the upgrade process to get updates and your changes and scripts won't be overwritten.  To give you this flexibility, we ask that you don't modify any files that are part of git-started except the symbolic links to README.md and LICENSE in the root.  It just makes the upgrade process easier.  If you need to tweak a script to allow you to hook in custom functionality, how about logging an issue?


util/config/git-started-setup
-----------------------------

This bash shell script is executed with every script.  It's a good spot to put settings that apply to everything on your project.  Here's the current settings:

* `COLOR` - Determines if we should use color or not.  Allowed values are `yes` (always use color), `no` (never use color) and `auto` (use color if displaying to a TTY).
* `COLOR_*` - There are lots of settings controlling the colors for errors, warnings, filenames, etc.
* `DEBUG` - Display lots of debugging information.  To enable, set it to 1.  You can also use `DEBUG=1 util/bin/pretty_print <file>` to enable it for a single program.
* `IGNORE_DIRS` - Space separated list of directories that should be excluded from pretty printing and lint checking.
* `LINT_<type>` - These are the linting programs that are configured for each detected file type.  See `util/helpers/lint_check.d/<type>` for available configured programs.
* `LINT_<type>_<program>_OPTIONS` - The default command-line flags and parameters that are used by the linting program.
* `PRETTY_*` - These are the pretty printer programs that are configured for each detected file type.  See `util/helpers/pretty_print.d/<type>` for available configured programs.
* `PRETTY_<type>_<program>_OPTIONS` - The default command-line flags and parameters that are used by the pretty printing program.


util/git/post-checkout.d/
-------------------------

By default, git only provides support for a single script to be executed as a hook.  This functionality was extended to run all scripts in a given directory.

The post-checkout.d scripts are ran after switching branches or checking out a commit.


util/git/post-merge.d/
----------------------

See `util/git/post-checkout.d` - this hook is ran after a merge or a pull.


util/git/pre-commit.d/
----------------------

See `util/git/post-checkout.d` - this hook is ran before a commit is finalized.


util/helpers/file_detection.d/
------------------------------

You can extend how `get_file_type()` determines the file type by plugging in additional scripts here.  There's one that can check the file extension for a few well-known web based languages.  You are not limited to merely file extensions.  You could check the path, open the file and read some data from it or exclude files from processing by plugging in a script earlier in the process that will return a message for all excluded files.

util/helpers/lint_check.d
-------------------------

A lint check will help to ensure you are committing syntactically correct files.  This uses the file detection in `get_file_type()` and will execute the script for the right file type.

If a script is required and it isn't in the special "per file type" directory, then we will check `_common` to see if it exists there.  That way you can write generic scripts and they will be picked up without the need for a symlink or similar functionality.

Scripts will be given an `$OPTIONS` variable, which corresponds to the lengthy one in `git-started-setup`.  It will save you from typing such horrible things as `$PRETTY_JS_JSLINT_OPTIONS` in your code, plus you can add more generic scripts to the `_common` directory and the options will always use the same variable name.


util/helpers/pretty_print.d
---------------------------

These programs will be executed to pretty print a given file type.  If the `get_file_type()` function returns 'php', then a `util/helpers/pretty_print.d/php` script will be used if it is found.

Just like the lint checkers, the pretty printing can utilize a `_common` directory fallback and the `$OPTIONS` variable will be already set.

util/helpers/setup.d
--------------------

Setup scripts to run in order to prepare your repository correctly.  This can check to make sure that you have the right software installed (like npm or php), build configuration files for your system, check out submodules, download things and do anything else that is required to get the repository working for you.

License Information
===================

This project is licensed under a MIT license with a non-advertising clause.  This license applies **ONLY** to the files that are a part of the git-started repository and does not extend into your project, even though your files are intended to be mixed with this repository.  See the [GitHub project] for information about file histories and to help determine what files come from the git-started repository.

[GitHub Project]: https://github.com/fidian/git-started/
