====================================================
 ensurepip --- Install the Python Package Installer
====================================================

.. module:: ensurepip
    :synopsis: Install the Python Package Installer, pip

:Purpose: Install pip, the Python package installer.

While Python is the "Batteries Included" programming language and
comes with a wide variety of modules in the standard library, there
are even more libraries, frameworks, and tools available to be
installed from the `Python Package Index`_. To install those packages,
a developer needs the installer tool :command:`pip`. Installing a tool
meant to install tools presents an interesting bootstrapping issue,
which :mod:`ensurepip` solves.

Installing ``pip``
==================

This example uses a virtual environment configured without
:command:`pip` installed.

.. {{{cog
.. run_script(cog.inFile, 'rm -rf /tmp/demoenv', interpreter='')
.. cog.out(run_script(cog.inFile, 'pyvenv --without-pip /tmp/demoenv', interpreter=''))
.. cog.out(run_script(cog.inFile, 'ls -F /tmp/demoenv/bin', interpreter='', include_prefix=False))
.. }}}

.. code-block:: none

	$ pyvenv --without-pip /tmp/demoenv
	

	$ ls -F /tmp/demoenv/bin
	
	activate
	activate.csh
	activate.fish
	python@
	python3@
	python3.5@

.. {{{end}}}

Run :mod:`ensurepip` from the command line using the ``-m`` option to
the Python interpreter.  To ensure a recent version of :command:`pip`
is installed, use the ``--upgrade`` option.

.. {{{cog
.. cog.out(run_script(cog.inFile, '/tmp/demoenv/bin/python -m ensurepip --upgrade', interpreter=''))
.. }}}

.. code-block:: none

	$ /tmp/demoenv/bin/python -m ensurepip --upgrade
	
	Ignoring indexes: https://pypi.python.org/simple
	Collecting setuptools
	Collecting pip
	Installing collected packages: setuptools, pip
	Successfully installed pip-8.1.1 setuptools-20.10.1

.. {{{end}}}

This installs ``pip3`` and ``pip3.5`` as commands in the virtual
environment, with the ``setuptools`` dependency needed to support
those commands.

.. {{{cog
.. cog.out(run_script(cog.inFile, 'ls -F /tmp/demoenv/bin', interpreter=''))
.. }}}

.. code-block:: none

	$ ls -F /tmp/demoenv/bin
	
	activate
	activate.csh
	activate.fish
	easy_install-3.5*
	pip3*
	pip3.5*
	python@
	python3@
	python3.5@

.. {{{end}}}


.. seealso::

   * :pydoc:`ensurepip`

   * :mod:`venv` -- Virtual environments

   * :pep:`453` -- Explicit bootstrapping of pip in Python installations

   * `Installing Python Modules
     <https://docs.python.org/3.5/installing/index.html#installing-index>`__
     -- Instructions for installing extra packages for use with
     Python.

   * `Python Package Index`_ -- Hosting site for extension modules for
     Python programmers.

   * `pip <https://pypi.python.org/pypi/pip>`__ -- Tool for installing
     Python packages.

.. _Python Package Index: https://pypi.python.org/pypi
