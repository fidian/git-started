The `exclude` file for git lists files and directories that should not be committed to a repository.

* Apple metadata files for the explorer and caches
* Windows metadata files for common tools and the explorer
* Backup files for various popular editors
* Settings and metadata files for popular IDEs
* Generated files commonly used, like ctags and etags
* Libraries that were installed by tools, like `node_modules/` and `vendor/` (more later)
* A local directory (more later)

Libraries
---------

Libraries won't be committed to the codebase because it is assumed that you are using [npm], [composer], or another tool that manages your dependencies.  Since they should be used to update your repository and manage these directories, they are not things that should be committed to the repository.

[composer]: http://getcomposer.org/
[npm]: https://npmjs.org/

Local directory
---------------

In developer environments, one often needs a specially tweaked config file or other assorted files to hang around near the repository but should never be committed.  To this end, the `local/` folder exists and is automatically ignored.
