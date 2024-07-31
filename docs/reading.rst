.. _reading:

Reading Photon-HDF5 files
=========================

You can easily read a Photon-HDF5 file in Python, MATLAB, LabVIEW or any
other language that supports HDF5 (C, C++, Java, R, etc...).
Photon-HDF5 files, like any other HDF5 file, are read using the HDF5 library
for the language of choice. One specific advantage is that all field names
(and their meaning) are defined in the specifications (:ref:`specifications`).

To read Photon-HDF5 in a given programming language, the user only needs to install
the HDF5 library and a wrapper for that language. Scientific Python distributions
and MATLAB already include all the needed software support. In the case of Python,
both pytables or h5py can be used. In the case of MATLAB, we suggest using 2013a
or later, which include more user-friendly functions to access HDF5 files.
LabVIEW users need to install the HDF5 library (www.hdfgroup.org) and a third-party
wrapper to support HDF5 file reading or writing.
`h5labview <http://h5labview.sourceforge.net/>`_
is currently our recommended HDF5 wrapper for LabVIEW.

Simple examples on reading Photon-HDF5 files can be found in the
`paper describing Photon-HDF5 <http://dx.doi.org/10.1101/026484>`__ (section SM.2).
Complete code example on reading Photon-HDF5 files in different programming languages
(currently Python, MATLAB and LabVIEW) are provided
`on GitHub <http://photon-hdf5.github.io/photon_hdf5_reading_examples>`__.

In the next section we discuss how to implement a generic Photon-HDF5 reader
for a single-molecule FRET analysis program.


Reading Photon-HDF5 in a smFRET analysis program
-------------------------------------------------

This section describes an example of how to add support for reading
Photon-HDF5 files to a smFRET analysis program.
This scheme is implemented in the burst analysis program
`FRETBursts <http://tritemio.github.io/FRETBursts/>`_
(see `code <https://github.com/tritemio/FRETBursts/blob/master/fretbursts/loader.py#L226>`_ for full details).


Read Photon-HDF5 files
''''''''''''''''''''''

1. Get the Photon-HDF5 version from ``format_version`` root-node attribute
   (see :ref:`root_params`). The version must be ``'0.4'`` or greater.

2. Optionally load metadata from `setup`, `sample`, `provenance` and `identity`
   groups. This is not needed for the analysis. The fields may not be present.

3. Same as in point 2 for root fields `description` and `acquisition_duration`.

4. If there is a ``/photon_data`` group the file is single-spot. Call the
   function to load single-spot data (see next section). If there is
   no ``/photon_data`` the file is a multi-spot one, call the multi-spot read
   function (next section).

Read single-spot function
'''''''''''''''''''''''''

Make a function that takes a channel parameter (`N`) and reads the data of the
corresponding channel (``/photon_dataN``). If `N` is not passed, the function
reads data from ``/photon_data``.

Read the photon-data following these steps:

1. Check if a photon-data group with given `N` is present, if not skip the
   channel. This is needed in cases of missing channels (e.g. dead pixel
   in SPAD array).

2. Load photon-data arrays. Load the array ``timestamps`` and its unit
   (``timestamps_specs/timestamps_unit``). If present, load also
   ``detectors``, ``nanotimes`` and ``particles``. If ``nanotimes`` is present
   load ``nanotime_specs`` (``tcspc_unit`` and ``tcspc_num_bins``).

3. Load measurements-specs. Refer to :ref:`measurement_specs_group`
   documentation for details. Note that ``measurement_specs`` may not be
   present.

4. If ``measurement_specs`` is present and the measurement-type starts with
   ``smFRET`` load detectors definitions
   (donor: ``measurement_specs/detectors_specs/spectral_ch1``,
   acceptor: ``measurement_specs/detectors_specs/spectral_ch2``).

5. For μs-ALEX load ``alex_period``, and for ns-ALEX load
   ``laser_repetition_rate``.

6. For both μs-ALEX and ns-ALEX, load the donor and acceptor
   period definitions (``alex_excitation_period1`` and
   ``alex_excitation_period1``). For μs-ALEX also load
   ``alex_offset``. All these field may not be present.

Read multi-spot function
''''''''''''''''''''''''

Implement the single-channel version then:

- Find all the root groups starting with ``photon_data`` and for each group
  load the data for that channel. There can be missing channels
  (e.g. if there are dead pixels).
