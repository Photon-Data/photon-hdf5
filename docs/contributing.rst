.. _contributing:

Collaborating
=============

The success of a file format is only determined by the extent of its adoption.
For this reason we greatly welcome any feedback and contribution from
interested users.

How to participate?
-------------------

The Photon-HDF5 project resources include:

1. `Reference Documentation <http://photon-hdf5.readthedocs.org/>`__ (i.e. this document).
2. `Examples on reading Photon-HDF5 in multiple languages <https://github.com/Photon-HDF5/photon_hdf5_reading_examples>`__.
3. Software for saving/converting Photon-HDF5 files: 
   `phconvert <http://photon-hdf5.github.io/phconvert/>`__ (reference python library);
   `phforge <http://photon-hdf5.github.io/phforge/>`__ (script to be used from any language);
   `Photon-HDF5 Online Converter <http://photon-hdf5.github.io/Photon-HDF5-Converter/>`__ (demostrative service).

All the sources (including for the documentation) are `hosted on GitHub <https://github.com/Photon-HDF5>`__
and we encourage to open *GitHub Issues* in the
`documentation repository <https://github.com/Photon-HDF5/photon-hdf5>`__
to discuss any topic related Photon-HDF5.
You can also contact us by email, but we prefer to use GitHub in order to
keep any discussion public.

Contributions (such as fixes or enhancements) can be sent using *GitHub Pull Requests*
(PR). You can find guides on how to send a PR on the GitHub website. If have have any
doubts, please contact us on the 
`Photon-HDF5 google group <https://groups.google.com/forum/#!forum/photon-hdf5>`_
and we will be glad to help you getting started.

There are several ways you can get involved:

- **Sending feedback**: if you use or plan to use Photon-HDF5 and have any comment
  or suggestion, please send it to us! Even if you don't have any problem we would like to
  hear back about your use case. For this topic please use the 
  `Photon-HDF5 google group <https://groups.google.com/forum/#!forum/photon-hdf5>`_
  or open an issue in the
  `documentation repository <https://github.com/Photon-HDF5/photon-hdf5>`__.

- **Documentation contributions**: if you feel that some section of this document
  should be expanded or enhanced in any way, please feel free to open an issue
  or send a PR (see note above) on the
  `documentation repository <https://github.com/Photon-HDF5/photon-hdf5>`__.

- **Contributing examples**: you can send a new example on reading Photon-HDF5
  files in a new language or for a different measurement type. Or simply
  send a fix for the
  `current examples <https://github.com/Photon-HDF5/photon_hdf5_reading_examples>`__.

- **Contributing to phconvert**: you can open issues to report bugs, discuss
  usage or propose enhancements. You are also more than welcome to send PR
  for fixes or enhancements to the library. The official repository is
  `this one <https://github.com/Photon-HDF5/phconvert>`__.


Contributions Acknowledgment
----------------------------

Any contributions to this documentation will be listed in the front page, just below
the authors.

Contributions to other repository (e.g. `phconvert <https://github.com/Photon-HDF5/phconvert>`__, 
`phforge <https://github.com/Photon-HDF5/phforge>`__, `reading examples <https://github.com/Photon-HDF5/photon_hdf5_reading_examples>`__,
etc.) will be  
acknowledged in the respective website and listed in their ``LICENSE`` files.
All the code in Photon-HDF5 projects is released under the 
`MIT license <http://opensource.org/licenses/MIT>`_.


Contributor Code of Conduct
---------------------------

The Photon-HDF5 team subscribes to the Contributor Covenant, version 1.0.0, available from 
http://contributor-covenant.org/version/1/0/0/.

.. _compatibility:

Maintaining Compatibility
-------------------------

Maintaining compatibility between versions is important.

Compatibility can be broadly definined in two ways

1. **Backward compatibility** means that files from previous version can be read and used by
   newer versions.
2. **Forward compatibility** means that files from newer versions can be read by older versions.

Backward compatibility is generally much easier to maintian, as it largely means not removing
anything from the format or changing something once it is established.
Forward compatibility on the other hand is more complicated, as new features cannot be read by
older versions, basically by definition. So the principle is that an older version should be
able to interpret any file of a newer version that does *not* contain/use the new feature.
Additionally, the new feature should be added in a way that older versions can still read
the data that does not involve the new feature.

In Photon-HDF5, new versions must be backwards compatible, and foward compatibility should be
maintained as much as possible. For this we have 5 main principles for maintaining compatibility:

#. New fields should always be optional/conditionally mandatory (i.e. mandatory only when a new
   feature is used in the particular experiment) with minor version updates, major version
   updates may make a new field mandatory.
#. The data type (options) of a field will not change from version to version.
#. Fields cannot be removed.
#. Whether or not a field is required must be implemented in a way consistent with previous
   versions, meaning:

   a. Any field introduced as mandatory will necessarily be mandatory in all future versions
   b. For conditionally mandatory fields, if a set of conditions requires a field to be
      mandatory in a previous version, it will also be mandatory under those conditions
      in future versions.
   c. In cases where new features (usually in another field) are added, then the new
      implementation should keep the field mandatory in all cases where, ignoring the new feature.
#. Validators should be considered version specific, they cannot validate photon-HDF5 files of
   versions newer that what they were designed for. However, we can implement a
   "permissive/strict" option (or other similar name) determining whether or not the validator
   checks for unknown fields, and whether or not to throw an error or simply warn.