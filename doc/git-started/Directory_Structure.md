The directory structure for a repository is a very important thing to consider.  This project lays one out that should be good for many projects.

Several of these directories do not yet exist, but support for them is built into the tools and configuration files that are distributed with this repository.

* `bin/` - Command line tools that you write.
* `doc/` - Documentation.  You do write documentation, right?
	* `doc/git-started/` - Documentation for this repository.  I need to put it somewhere.
* `lib/` - Your code should go in here.  Classes, procedural code, etc.
* `local/` - Local settings and configurations that should not ever get committed to a repository.
* `node_modules/` - Modules installed with `npm` for node.js projects.
* `test/` - Unit tests, functional tests, testing scripts and all testing resources.
* `util/` - Utility scripts.
	* `util/dev/` - Scripts that apply to just development environments.
	* `util/git/` - Git related scripts and config files that will do pretty printing, lint checking, and much more.
		* `util/git/setup.d/` - Stages to execute when setting up the repository.
		* `util/git/hooks/` - Generic hooks that execute the various directories under `util/git/hooks.d/`.
		* `util/git/hooks.d/` - Directories containing shell scripts for tying into git's processes.
* `vendor/` - 3rd party tools and libraries that you need to distribute with your code.  Files in here are omitted from the pretty printing and lint checking process.
