Dotfile management made easy
============================

.. image:: https://badge.fury.io/py/dotfiles.png
  :target: http://badge.fury.io/py/dotfiles

.. image:: https://secure.travis-ci.org/jbernard/dotfiles.png?branch=master
  :target: http://travis-ci.org/jbernard/dotfiles

``dotfiles`` is a tool to make managing your dotfile symlinks in ``$HOME``
easy, allowing you to keep all your dotfiles in a single directory.

Hosting is up to you. You can use a VCS like git, Dropbox, or even rsync to
distribute your dotfiles repository across multiple hosts.

The repository can be specified at runtime, so you can manage multiple
repositories without hassle. See the Configuration_ section below for further
details.

Directories are supported as well. Any file object in your home directory that
starts with a ``.`` is fair game.

Interface
---------

``-a, --add <file...>``
    Add dotfile(s) to the repository.

``-c, --check``
    Check for missing or unsynced dotfiles.

``-l, --list``
    List currently managed dotfiles, one per line.

``-r, --remove <file...>``
    Remove dotfile(s) from the repository.

``-s, --sync [file...]``
    Update dotfile symlinks. You can overwrite colliding files with ``-f`` or
    ``--force``.  All dotfiles are assumed if you do not specify any files to
    this command.

``-m, --move <path>``
    Move dotfiles repository to another location, updating all symlinks in the
    process.

For all commands you can use the ``--dry-run`` option, which will print actions
and won't modify anything on your drive.

Installation
------------

To install dotfiles, simply: ::

    $ pip install dotfiles

Or, if you absolutely must: ::

    $ easy_install dotfiles

But, you really shouldn't do that.

If you want to work with the latest version, you can install it from `the
repository`_::

    $ git clone https://github.com/jbernard/dotfiles
    $ cd dotfiles
    $ ./bin/dotfiles --help

Examples
--------

To install your dotfiles on a new machine, you might do this: ::

  $ git clone https://github.com/me/my-dotfiles Dotfiles
  $ dotfiles --sync

To add '~/.vimrc' to your repository: ::

  $ dotfiles --add ~/.vimrc     (relative paths work also)

To make it available to all your hosts: ::

  $ cd ~/Dotfiles
  $ git add vimrc
  $ git commit -m "Added vimrc, welcome aboard!"
  $ git push

You get the idea. Type ``dotfiles --help`` to see the available options.

Configuration
-------------

You can choose to create a configuration file to store personal customizations.
By default, ``dotfiles`` will look for ``~/.dotfilesrc``. You can change this
with the ``-C`` flag. An example configuration file might look like: ::

  [dotfiles]
  repository = ~/Dotfiles
  ignore = [
      '.git',
      '.gitignore',
      '*.swp']
  externals = {
      '.bzr.log':     '/dev/null',
      '.uml':         '/tmp'}

You can also store your configuration file inside your repository. Put your
settings in ``.dotfilesrc`` at the root of your repository and ``dotfiles`` will
find it. Note that ``ignore`` and ``externals`` are appended to any values
previously discovered.

Prefixes
--------

Dotfiles are stored in the repository with no prefix by default. So,
``~/.bashrc`` will link to ``~/Dotfiles/bashrc``. If your files already have a
prefix, ``.`` is common, but I've also seen ``_``, then you can specify this
in the configuration file and ``dotfiles`` will do the right thing. An example
configuration in ``~/.dotfilesrc`` might look like: ::

  [dotfiles]
  prefix = .

Externals
---------

You may want to link some dotfiles to external locations. For example, ``bzr``
writes debug information to ``~/.bzr.log`` and there is no easy way to disable
it. For that, I link ``~/.bzr.log`` to ``/dev/null``. Since ``/dev/null`` is
not within the repository, this is called an external. You can have as many of
these as you like. The list of externals is specified in the configuration
file: ::

  [dotfiles]
  externals = {
      '.bzr.log':     '/dev/null',
      '.adobe':       '/tmp',
      '.macromedia':  '/tmp'}

Ignores
-------

If you're using a VCS to manage your repository of dotfiles, you'll want to
tell ``dotfiles`` to ignore VCS-related files. For example, I use ``git``, so
I have the following in my ``~/.dotfilesrc``: ::

  [dotfiles]
  ignore = [
      '.git',
      '.gitignore',
      '*.swp']

Any file you list in ``ignore`` will be skipped. The ``ignore`` option supports
glob file patterns.

Packages
--------

Many programs store their configuration in ``~/.config``. It's quite cluttered
and you probably don't want to keep all its content in your repository. For this
situation you can use the ``packages`` setting::

    [dotfiles]
    packages = ['config']

This tells ``dotfiles`` that the contents of the ``config`` subdirectory of
your repository must be symlinked to ``~/.config``. If for example you have a
directory ``config/awesome`` in your repository, it will be symlinked to
``~/.config/awesome``.

This feature allows one additional level of nesting, but further subdirectories
are not eligible for being a package.  For example, ``config`` is valid, but
``config/transmission`` is not valid.  Arbitrary nesting is a feature under
current consideration.

At the moment, packages can not be added or removed through the command line
interface.  They must be constructed and configured manually.  Once this is
done, ``sync``, ``list``, ``check``, and ``move`` will do the right thing.
Support for ``add`` and ``remove`` is a current TODO item.

Contribute
----------

.. image:: https://badge.waffle.io/jbernard/dotfiles.png?label=ready
  :target: http://waffle.io/jbernard/dotfiles

If you'd like to contribute, simply fork `the repository`_, commit your changes,
make sure tests pass, and send a pull request. Go ahead and add yourself to
AUTHORS_ or I'll do it when I merge your changes.

.. _`the repository`: https://github.com/jbernard/dotfiles
.. _AUTHORS: https://github.com/jbernard/dotfiles/blob/master/AUTHORS.rst
