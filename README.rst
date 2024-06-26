.. image:: https://github.com/IDR/omero-mkngff/workflows/OMERO/badge.svg
    :target: https://github.com/IDR/omero-mkngff
.. image:: https://badge.fury.io/py/omero-mkngff.svg
    :target: https://badge.fury.io/py/omero-mkngff

omero-mkngff
============

Plugin to swap OMERO filesets with NGFF


Usage
=====

To create sql containing required functions and run it:

::

    $ omero mkngff setup > setup.sql
    $ psql -U omero -d idr -h $DBHOST -f setup.sql

To generate sql and create the symlinks from the ManagedRepository to the NGFF data for a
specified Fileset ID:
The clientpath is used as the basis for the clientpath for each file in the Fileset. If omitted then
the clientpath is 'unknown'. When writing .zarr.bfoptions into the ManagedRepository alongside the
symlink to fileset.zarr, the clientpath will be added to bfoptions as `omezarr.alt_store`, allowing
ZarrReader to read directly from that source.

::

    $ omero mkngff sql --symlink_repo /OMERO/ManagedRepository --secret=secret --bfoptions --clientpath=https://url/to/fileset.zarr 1234 /path/to/fileset.zarr > myNgff.sql
    $ psql -U omero -d idr -h $DBHOST -f myNgff.sql

To ONLY perform the symlink creation (and optionally create fileset.zarr.bfoptions with clientpath as above)

::

    $ omero mkngff symlink /OMERO/ManagedRepository 1234 /path/to/fileset.zarr --bfoptions --clientpath=https://url/to/fileset.zarr


To ONLY create fileset.zarr.bfoptions

::

    $ omero mkngff bfoptions /OMERO/ManagedRepository 1234 /path/to/fileset.zarr

Requirements
============

* OMERO 5.6.0 or newer
* Python 3.6 or newer


Installing from PyPI
====================

This section assumes that an OMERO.py is already installed.

Install the command-line tool using `pip <https://pip.pypa.io/en/stable/>`_:

::

    $ pip install -U omero-mkngff

Release process
---------------

This repository uses `bump2version <https://pypi.org/project/bump2version/>`_ to manage version numbers.
To tag a release run::

    $ bumpversion release

This will remove the ``.dev0`` suffix from the current version, commit, and tag the release.

To switch back to a development version run::

    $ bumpversion --no-tag [major|minor|patch]

specifying ``major``, ``minor`` or ``patch`` depending on whether the development branch will be a `major, minor or patch release <https://semver.org/>`_. This will also add the ``.dev0`` suffix.

Remember to ``git push`` all commits and tags.

License
-------

This project, similar to many Open Microscopy Environment (OME) projects, is
licensed under the terms of the GNU General Public License (GPL) v2 or later.

Copyright
---------

2023-2024, The Open Microscopy Environment
