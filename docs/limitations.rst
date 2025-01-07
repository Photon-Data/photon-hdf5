Known Limitations
=================

In this section, we list some features that are not currently supported.
If you think that some of these should be included in the specifications,
please contact us.


Timestamps with rollover
------------------------

In Photon-HDF5 timestamps are usually signed 64 bit integers. Thanks to
compression, there is no size penalty compared to 32 bit integers. Most
timestamping hardware produce a timestamp with 24 or 32 bits
and a rollover flag in order to compute the full "unwrapped" timestamp.
Saving timestamps with a separate rollover information is not currently
supported, therefore the rollover correction must be computed before
saving data in a Photon-HDF5 file.

Timestamps with rollover may be supported in a future version of Photon-HDF5.

