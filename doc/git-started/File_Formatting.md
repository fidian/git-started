File Formatting
===============

The `git-started` repository is automatically set up with some convenient file manipulation tools.  On commits, your file will be checked for problems ("lint check") and will also be beautified ("pretty print").

You can manually run `util/bin/lint_check` and `util/bin/pretty_print` on your files directly.  The commands take a single filename as their only parameter.  The lint checker will output errors to the screen and return a non-zero result if any problems are found.  The pretty printer will rewrite the file specified unless there are problems; in which case it will print errors to the screen and return a non-ero result.  The type of the file is determined automatically by the scripts in `util/helpers/file_detection.d`.  Once a file type is found, the scripts in `util/helpers/lint_check.d/TYPE/` or `util/helpers/pretty_print.d/TYPE/` are executed, based on the order they are listed in `util/bin/git-started-setup` and `util/config/git-started-setup` (if you have that file).

The lint checkers and pretty printer scripts require external tools to be installed and will silently take no action if the tool is not installed.  Check with the individual file types below to see what options are available.  Thus, if you want the pretty printing or the lint checking to work automatically, you will need to install at least one tool to do it.


CSS - Cascading Style Sheets
----------------------------

Lint checkers:

* PrettyCSS - A node.js based validator and pretty printer.  https://github.com/fidian/PrettyCSS

Pretty printers:

* PrettyCSS - A node.js based validator and pretty printer.  https://github.com/fidian/PrettyCSS


JS - JavaScript
---------------

Lint checkers:

* jsl - JavaScript Lint.  http://www.javascriptlint.com/
* jslint - JSLint, command-line version.  https://github.com/reid/node-jslint

Pretty printers:

* None


PHP - PHP: Hypertext Preprocessor
---------------------------------

Lint checkers:

* cli - Command-line version of PHP.  http://php.net/

Pretty printers:

* phpbeautifier - PEAR's PHP_Beautifier package.  http://pear.php.net/package/PHP_Beautifier
