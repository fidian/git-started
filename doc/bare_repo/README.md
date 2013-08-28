Bare Repository
===============

Are you tired of setting up pretty printers and lint checkers for your project, over and over?  Me too.  You can now merge this project into yours and it will set up git hooks that do this sort of thing for you.  In case your files are not covered by our scripts, the code is extensible and you merely create additional files in the right places to enable support for additional file types and coding styles.

It currently works really well for Linux projects.  I also have it working natively on Mac (OS X) and would happily accept patches for Cygwin and other platforms.


How To Include In Your Project
==============================

1.  Use git and a Linux-based environment.  This may work with cygwin, but that's untested.  It relies heavily on bash and several tools that are typically found in the `coreutils` package.

2.  Create a repository for your project.  _Don't clone this repository!_  It won't be what you need in the end.  If you already have a repository created, you can safely skip this step.

3.  Fetch and merge this repository into yours.

		# Start off in your repository
		cd your_repository
		
		# It is best to work in branches so you can undo things easier
		git checkout -b bare_repo_branch
		
		# Add the bare_repo repository as a remote
		git remote add bare_repo https://github.com/fidian/bare_repo.git
		
		# Get the commit history from bare_repo
		git fetch bare_repo
		
		# Merge bare_repo's master into your working branch
		git merge bare_repo/master
		
		# Handle merge conflicts and finish the commit if you have problems
		# ... work work work ...
		
		# Then you are ready to merge bare_repo_branch into your repository
		git checkout master
		git merge bare_repo_branch
		git branch -d bare_repo_branch
		git pull
		git push

4.  Configure your project.  You may need to create `util/config/bare_repo_setup`, which is a bash shell script that can apply additional configuration settings to the shell scripts.  Also, you may wish to add files under `util/helpers/` that are specific to your project.

5.  Run `util/bin/setup_repository` to set up the git hooks and do other actions.


Upgrading
=========

If you use the bare_repo repository and wish to update to the latest copy of these scripts, the upgrade process is essentially the same.

	cd your_repository
	git checkout -b bare_repo_branch
	git fetch bare_repo
	# If that fails, use "git remote add" and then fetch again
	git merge bare_repo/master
	# Handle merge conflicts here
	git checkout master
	git merge bare_repo_branch
	git branch -d bare_repo_branch
	git pull
	git push


Customization
=============

This repository is intended to augment yours and to get some tedious tasks out of the way.  Its goal is to let you extend its functionality quickly and easily.  You can tie into many different points in order to configure, extend and override actions that are performed.

We intend to let you extend everything without having to modify any of the files in this repository.  That way you can follow the upgrade process to get updates and your changes and scripts won't be overwritten.  To give you this flexibility, we ask that you don't modify any files that are part of bare_repo except the symbolic links to README.md and LICENSE in the root.

util/config/bare_repo_setup
---------------------------

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


util/helpers/pretty_print.d
---------------------------

These programs will be executed to pretty print a given file type.  If the `get_file_type()` function returns 'php', then a `util/helpers/pretty_print.d/php` script will be used if it is found.


util/helpers/setup.d
--------------------

Setup scripts to run in order to prepare your repository correctly.  This can check to make sure that you have the right software installed (like npm or php), build configuration files for your system, check out submodules, download things and do anything else that is required to get the repository working for you.


License Information
===================

This project is licensed under a MIT license with a non-advertising clause.  This license applies **ONLY** to the files that are a part of the bare_repo repository and does not extend into your project, even though your files are intended to be mixed with this repository.  See the [GitHub project] for information about file histories and to help determine what files come from the bare_repo repository.

[GitHub Project]: https://github.com/fidian/bare_repo/
