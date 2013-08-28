Git Hooks
=========

The hooks that the `git-started` repository sets up may get a little confusing, especially with automatic lint checking and pretty printing happening.  To help with debugging, this will walk you through the steps that happen.

* The user wishes to commit changes, using `git commit myFile.js`.
* Git will use its `.git/hooks/pre-commit` script (added by the `util/bin/setup_repository` script).
* This calls every script in `util/git/pre-commit.d` for every file affected by the unless one produces an error.
* The files are checked for various issues (not lint nor pretty printing).
* The `util/bin/lint_check` script gets called for this particular file.
	* The scripts in `util/helpers/lint_check.d/*/` are ran for the correct file type, according to the LINT_* configuration setting.
* The `util/bin/pretty_print` script gets called for this particular file.
	* The scripts in `util/helpers/pretty_print.d/*/` are ran for the correct file type, according to the PRETTY_* configuration setting.
* Once everything passes, the commit is allowed to happen.

For more information on the pretty printing and lint checking, see the [File Formatting](File_Formatting.md). document.
