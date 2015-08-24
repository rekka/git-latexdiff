
Background
==========

``latexdiff`` is a script which will produce a LaTeX diff between two
LaTeX files.  The diff output can be compiled to a PDF (or other
formats) with blue/red markers for insertions/deletions.  However,
there are a lot of manual steps to get the two files, do the diff,
compile LaTeX, run BibTeX remove temporary files, and so on.
git-latexdiff helper automates that, and can be integrated as a
subcommand in ``git``.

Usage
=====

For installation, see below.

* Enter the ``git`` repository

* Run ``git latexdiff`` if your tex file is the only file that has
  changed, or run ``git latexdiff yourfile.tex`` to apply it to a
  specific file.  You must run it only on tex files, it can't operate on
  entire directories.  Any other generic ``git diff`` arguments for
  specifying versions should work.

* A ``.pdf`` file should be created in your current directory, named by
  default ``diff-yourfile.tex.pdf``.  Filename will be printed by the
  scripts as well.  It is possible that existing filenames will be
  overwritten.

Arguments are passed via environment variables (there isn't a good way
to pass options otherwise, also passing options is very rare.)  It is
easiest to do this via the command line.  Example (in bash): ``V=1 git
latexdiff ...``.

* ``V=``: set to any value to enable verbose mode.  Set to "2" to also
  display output of compiling LaTeX and increase the verbosity.
* ``LATEX=``: LaTeX program to run (default: ``pdflatex``)
* ``LATEXDIFF=``: set path to latexdiff
* ``LDOPTS=``: options to ``latexdiff`` itself.
* ``PDFVIEWER=``: PDF viewer to run (default: none)

Steps taken, and cases handled, by this helper:

* Diffs two versions and puts them in a temporary directory
* Runs BibTeX if \bibliography command is detected.
* Uses currently checked out version of figures
* Runs LaTeX in the non-stop mode, so any errors or missing figures are
  just ignored.
* Runs LaTeX twice to get references correct.
* Copy output to the current directory.
* Have all temporary files placed in a temporary directory that is
  deleted, so that they never interfere with the current directory.
* Automatically start a PDF viewer with the output if ``$PDFVIEWER`` is
  set.



Installation and dependencies
=============================

This depends on the latexdiff perl script, which you must install
yourself separately:
  http://www.ctan.org/tex-archive/support/latexdiff/
This is in various package repositories, for example is in Debian in
the ``latexdiff`` package.  Unfortunately, I can't help in installing
on other platforms.  One report says it is in macports.



Place the program ``git-latexdiff-helper`` somewhere in your PATH.
Alternatively, put it anywhere and put the full path to it in the
second ``git config`` command below::

    cp git-latexdiff-helper path/to/somewhere/

Set ``git-latexdiff-helper`` executable::

    chmod a+x git-latexdiff-helper

Set your git config by running ``./install`` script or alternatively by::

    git config --global alias.latexdiff "difftool -t latexdiff"
    git config --global difftool.latexdiff.cmd 'git-latexdiff-helper "$LOCAL" "$REMOTE"'

If you want, edit the top of the file to set default PDF viewer or
LaTeX command.


Other resources
===============

The ``latexdiff`` main page is:
http://www.ctan.org/tex-archive/support/latexdiff/

This formed the basis for this tool:

http://tex.stackexchange.com/questions/1325/using-latexdiff-with-git

This tool is written by Richard Darst and released under the Gnu
General Public License version 2 or later.

