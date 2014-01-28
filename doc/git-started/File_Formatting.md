File Formatting
===============

The `git-started` repository is automatically set up with some convenient file manipulation tools.  On commits, your file will be checked for problems ("lint check") and will also be beautified ("pretty print").

You can manually run `util/bin/lint_check` and `util/bin/pretty_print` on your files directly.  The commands take a single filename as their only parameter.  The lint checker will output errors to the screen and return a non-zero result if any problems are found.  The pretty printer will rewrite the file specified unless there are problems; in which case it will print errors to the screen and return a non-zero result.  The type of the file is determined automatically by the scripts in `util/helpers/file_detection.d`.  Once a file type is found, the scripts in `util/helpers/lint_check.d/TYPE/` or `util/helpers/pretty_print.d/TYPE/` are executed, based on the order they are listed in `util/bin/git-started-setup` and `util/config/git-started-setup` (if you have that file).

Supported Commands
==================

The lint checkers and pretty printer scripts require external tools to be installed and will silently take no action if the tool is not installed.  Check with the individual file types below to see what options are available.  Thus, if you want the pretty printing or the lint checking to work automatically, you will need to install at least one tool to do it.


Common - Available for all file types
-------------------------------------

Lint checkers:

* None currently.

Pretty printers:

* expand - Change tabs to spaces.
* fail - Fail pretty printing if you get this far.
* lint - Lint check.  See [issue #2](https://github.com/fidian/git-started/issues/2).
* unexpand - Change spaces to tabs.

CSS - Cascading Style Sheets
----------------------------

Lint checkers:

* PrettyCSS - A node.js based validator and pretty printer.  [GitHub](https://github.com/fidian/PrettyCSS)

Pretty printers:

# CSSComb - Sorts CSS properties in a specific order.  [Site](http://csscomb.com/)
* PrettyCSS - A node.js based validator and pretty printer.  [GitHub](https://github.com/fidian/PrettyCSS)


HTML - HyperText Markup Language
--------------------------------

Lint checkers:

* None currently.

Pretty printers:

* tidy - HTML Tidy.  Made for the experimental HTML5 fork of the existing "tidy" HTML tool.  Should also work with original version.  [GitHub-Experimental](https://github.com/w3c/tidy-html5) [SourceForge-Original](http://tidy.sourceforge.net/)

JS - JavaScript
---------------

Lint checkers:

* jsl - JavaScript Lint.  http://www.javascriptlint.com/
* jslint - JSLint, command-line version.  https://github.com/reid/node-jslint

Pretty printers:

* js-beautify - JavaScript based beautifer.  [Website](http://jsbeautifier.org/)


JSON - JavaScript Object Notation
---------------------------------

Lint checkers:

* jslint - JSLint, command-line version.  [GitHub](https://github.com/reid/node-jslint)

Pretty printers:

* None currently.


Markdown
--------

Lint checkers:

* None currently.

Pretty printers:

* expand - Change tabs into spaces.
* unexpand - Change spaces into tabs.


PHP - PHP: Hypertext Preprocessor
---------------------------------

Lint checkers:

* cli - Command-line version of PHP.  http://php.net/

Pretty printers:

* phpbeautifier - PEAR's PHP_Beautifier package.  [PEAR](http://pear.php.net/package/PHP_Beautifier)
* php-cs-fixer - Fabien Potencier's code fixer.  [GitHub](https://github.com/fabpot/PHP-CS-Fixer)


SASS - Syntastically Awesome Style Sheets
-----------------------------------------

Lint checkers:

* sass - Ruby SASS implementation [GitHub](https://github.com/nex3/sass)

Pretty Printers:

* sass - Ruby SASS implementation [GitHub](https://github.com/nex3/sass)


SCSS - Syntastically Awesome Style Sheets v3
--------------------------------------------

Lint checkers:

* sass - Ruby SASS implementation [GitHub](https://github.com/nex3/sass)

Pretty Printers:

* sass - Ruby SASS implementation [GitHub](https://github.com/nex3/sass)


Writing Your Own
================

There's several points you need to follow in order to plug in your own pretty printers.

* The file type detection is done with scripts from `util/helpers/file_detection.d`, which are ran sequentially until one produces a result.  This will help you if you are adding support for a file type that isn't yet known by git-started.

* The `util/helpers/pretty_print.d` and `util/helpers/lint_check.d` directories have subdirectories based on file type.  If you are processing a `css` file, for instance, a formatter would first be sought in the `css/` folder and then in the `_common/` folder.

* These scripts are designed to be called by the pretty printer or the lint checker.  They are not intended to be used directly from the command line, as they rely on functions and variables set up by the `git-started-setup` script.

* The scripts must be marked as executable if you want it to run.

* The script does two different things, which depends on whether or not a filename was passed to the script.
    * With no arguments, the script detects if the command is installed and is working.  Exit with non-zero to indicate failure.
    * When linters are passed a filename, it is supposed to check the file for problems.  If any are found, write to stdout or stderr and return a non-zero status.
    * When pretty printers are passed a filename, they are supposed to pretty print the file in-place.  Return zero on success, or write useful information to stdout or stderr if there is an error.

* Successful runs may hide warnings and errors, so the return status is very important.

* `$OPTIONS` are the options defined in `git-started-setup` without the prefix.  It will be `$LINT_PHP_CLI_OPTIONS`, `$PRETTY_PHP_PHPBEAUTIFIER_OPTIONS` or whatever applies to your script.

