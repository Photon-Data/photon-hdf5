Introduction
============

Overview
------------

This document contains specifications of the Photon-HDF5 format.
This format allows saving single-molecule spectroscopy experiment data in
which at least one stream of photon timestamps is present.
It has been designed
as a standard container format for a broad range of experiments
involving confocal microscopy. Examples are confocal smFRET
experiments performed with a single or multiple excitation spots.
Both `μs-ALEX <http://dx.doi.org/10.1529/biophysj.104.054114>`_
and `PIE <https://doi.org/10.1529/biophysj.105.064766>`_
(aka `ns-ALEX <https://doi.org/10.1073/pnas.0508584102>`_)data
are supported.


What problems are we trying to solve?
-------------------------------------

-  Ensuring long term data persistence.
-  A disk space and read speed efficient file format for repeated use as well as for archiving.
-  Facilitating data sharing and interoperability between analysis programs and
   research groups.

Features of HDF5
----------------

-  Open-standard: language and platform independent, self-describing format
   with open source implementations.
-  Efficient: the HDF5 format is a binary format that allows compression
   and fast read/write operations.
-  Flexible: data arrays can be stored in "groups" (hierarchical
   format). Metadata can be attached to each data entry (attributes).
-  No limit in data size.
-  Support for a variety of numeric and non-numeric
   data types.

Photon-HDF5: Design principles
------------------------------

The main design principles are:

-  Simplicity
-  Flexibility
-  Compatibility

We aim to define a format that has a minimal set of specifications and therefore
is easy to implement. At the same time, it is important that the format can be
expanded to accommodate new use cases while maintaining backward compatibility
(see :ref:`compatibility`.

To achieve simplicity, the only required file characteristics are a
general file layout and the presence of a few basic attributes and parameters.
The remaining fields defined in this document will be present
only when needed by a particular measurement.

We retain flexibility by allowing the user to save any arbitrary data
outside the ones defined in the specifications. To assure that future versions of
this format will not conflict with user-defined fields, we require
that all user-defined fields be contained in groups called ``user``.
