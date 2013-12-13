Speed Considerations
====================

Faster is better than slower.

These shell scripts are designed to use BASH because almost every platform has that shell.  Since we are gearing for one specific shell, we should use everything in our power to make it run super fast on that shell.

* Avoid calls to outside programs
    * `dirname "$VAR"` -> `${VAR%/*}` ([source](http://www.gnu.org/software/bash/manual/bashref.html#Shell-Parameter-Expansion))
    * `basename "$VAR"` -> `${VAR##*/}` ([source](http://stackoverflow.com/questions/2664740/extract-file-basename-without-path-and-extension-in-bash))
* Avoid subshells
    * Instead using `VAR="$(some_command)"`, try to pass the variable by reference instead with upvar.

Speed doesn't always mean execution time.  Sometimes it is just allowing people to write code faster.

* Use `trap` to set up cleanup scripts.  `git-started` uses `on_exit` to add extra commands.  ([source](http://www.linuxjournal.com/content/use-bash-trap-statement-cleanup-temporary-files))
