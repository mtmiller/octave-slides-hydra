================
Octave and Hydra
================

:author: Mike Miller
:date: September 20, 2014

.. footer:: OctConf - September 2014

-----------------------
What are Hydra and Nix?
-----------------------

* `Hydra <http://nixos.org/hydra/>`_ is a continuous build system based on Nix
* `Nix <http://nixos.org/nix/>`_ is a functional package management system

  * Functional - reproducible, no side effects, etc
  * Package management - like dpkg, rpm, etc

* `NixOS <http://nixos.org/>`_ is a distribution built and managed using Nix

  * Distribution built from nix packages, either source or binary

-----------------------------
My Basic Understanding of Nix
-----------------------------

* A Nix `expression` is recipe describing the inputs and rules for a package
* A `derivation` is one set of rules in the expression making an output
* Inputs may include:

  * VCS repository or source tarball(s)
  * outputs of other Nix packages
  * environment variables

* Output is typically a chroot containing the result of ``make install``

  * Path like ``/nix/store/vnzqk4px3cvilxfcazfx2hcqbrgmw2wb-octave``
  * Contents look like
    `this <http://hydra.nixos.org/build/14362428/contents/1>`__

--------------
Hydra Overview
--------------

* Hydra uses Nix expressions to do continuous build of a project
* Rebuild can be triggered on:

  * commits to project source repository
  * changes to the expression (for us, this is ``hydra-recipes.git``)
  * any change to any dependent package, recursively
  * not immediate, does not rebuild for every single hg revision

---------
Live Demo
---------

So what does Hydra do for us today?

* Continuous build of Octave

  * Builds source distribution from hg (``make dist``)
  * Builds and runs test suite on GNU/Linux x86-32 and x86-64
  * Builds and runs test suite with coverage report

* Emails if/when build fails (sent to me only for now)
* Full build and test logs are always available
  `here <http://hydra.nixos.org/job/gnu/octave-default/build.x86_64-linux/latest/log/raw>`__
* Source snapshot of default is always available
  `here <http://hydra.nixos.org/job/gnu/octave-default/tarball/latest/download>`__
* HTML coverage report is always available
  `here <http://hydra.nixos.org/job/gnu/octave-default/coverage/latest/download>`__

-----------
Limitations
-----------

* Hydra works on Linux / Unix-like systems only
 
  * no Windows, OSX

* Hydra builds against its own outputs

  * only the latest versions of all dependencies
  * no backports to older libraries, or even other distros

* Not sure if Hydra can build from multiple branches

  * for now we are only building default

* Not all dependencies are nix-packaged yet

  * no ARPACK, gl2ps, or QScintilla

----------------------------------
Ideal Continuous Build for Octave?
----------------------------------

* Build all hg branches
* Build on different distros / versions
* Build with or without certain features?
* Build on or cross-build for Windows and OSX?
* Coverage reporting - how to test the GUI?
* Outputs that could actually be installed by end users?

-------
Summary
-------

* Hydra is continuously building Octave for us right now
* Emails when build fails, but need to manually find cause and fix/report
* Some nice outputs (coverage report, source tarball)
* Misses several build scenarios that we might want to test
* Might want to investigate alternatives, consider tradeoffs (hosting and
  maintenance burden, etc)
