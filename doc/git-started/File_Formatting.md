File Formatting
===============

The `git-started` repository is automatically set up with some convenient file manipulation tools.  On commits, your file will be checked for problems ("lint check") and will also be beautified ("pretty print").

You can manually run `util/bin/lint_check` and `util/bin/pretty_print` on your files directly.  The commands take a single filename as their only parameter.  The lint checker will output errors to the screen and return a non-zero result if any problems are found.  The pretty printer will rewrite the file specified unless there are problems; in which case it will print errors to the screen and return a non-zero result.  The type of the file is determined automatically by the scripts in `util/helpers/file_detection.d`.  Once a file type is found, the scripts in `util/helpers/lint_check.d/TYPE/` or `util/helpers/pretty_print.d/TYPE/` are executed, based on the order they are listed in `util/bin/git-started-setup` and `util/config/git-started-setup` (if you have that file).

Supported Commands
==================

The lint checkers and pretty printer scripts require external tools to be installed and will silently take no action if the tool is not installed.  Check with the individual file types below to see what options are available.  Thus, if you want the pretty printing or the lint checking to work automatically, you will need to install at least one tool to do it.


CSS - Cascading Style Sheets
----------------------------

Lint checkers:

* PrettyCSS - A node.js based validator and pretty printer.  [GitHub](https://github.com/fidian/PrettyCSS)

Pretty printers:

* PrettyCSS - A node.js based validator and pretty printer.  [GitHub](https://github.com/fidian/PrettyCSS)


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

* These scripts are designed to be called by the pretty printer or the lint checker.  They are not intended to be used directly from the command line.

* The script must be marked as executable if you want it to run.

* When the script is not passed a file, it must return 0 if the command is installed or non-zero if there's a problem using that command.

* When the script is passed a file, it is expected to lint check or pretty print the specified file.  For pretty printing, the pretty printed output should replace the content.

* During execution, the warnings and errors should be suppressed.  If there are any problems detected, first call `lint_error` or `pretty_error` with the real filename, then the problems should be displayed.

* Environment variables are available to your script:
    * `$OPTIONS` are the options defined in git-started-setup, with the prefix removed.  It will be `$LINT_PHP_CLI_OPTIONS` or `$PRETTY_PHP_PHPBEAUTIFIER_OPTIONS` or whatever applies to your script.
    * `$TARGET` is the real file that we are processing.  The pretty printer works strictly on a copy of the target to help avoid problems and this variable should only be used for showing errors.
