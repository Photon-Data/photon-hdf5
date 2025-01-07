The "generic" measurement type
==============================

For measurement types which cannot be described by any other
:ref:`supported measurement_type <measurement_specs_group>`,
it is possible to use the "generic" type and use the ``/setup``
fields to describe the exact configuration.

The "generic" type is the encouraged type for most experiments
going forward, as it supports arbitrary configurations.


Examples
--------

Below, we list a few examples of measurements described with the "generic"
measurement_type.

2-Polarizations
^^^^^^^^^^^^^^^

A setup detecting two polarizations in the detection path will be defined by::

    /setup/num_spectral_ch = 1
    /setup/num_split_ch = 1
    /setup/num_polarization_ch = 2

and will also specify the detector used for each polarizations::

    /photon_data/measurement_specs/detectors_specs/
        polarization_ch1 = [0, ]
        polarization_ch2 = [1, ]

(where the values 0 and 1 are only examples). In addition ``/setup/lifetime``
will indicate whether the measurement has TCSPC data or not. Finally, the optional field
``/setup/excitation_polarizations`` can be used to save the angle of each
polarization.

2-Split channels
^^^^^^^^^^^^^^^^

A setup in which the detection (emission) path is split
in two using a non-polarizing beam splitter will be defined by::

    /setup/num_spectral_ch = 1
    /setup/num_split_ch = 2
    /setup/num_polarization_ch = 1

and the measurement_specs will contain::

    /photon_data/measurement_specs/detectors_specs/
        split_ch1 = [0, ]
        split_ch2 = [1, ]

(where the values 0 and 1 are only examples). In addition ``/setup/lifetime``
will indicate whether the measurement has TCSPC data or not. Finally, the optional
field ``/setup/detection_split_ch_ratios`` can be used to store the
"splitting-ratio" of each channel. For example, with a 50-50% beam splitter
this field will be ``[0.5, 0.5]``.

2-Spectral channels
^^^^^^^^^^^^^^^^^^^

This case is already covered by the "smFRET" ``measurement_type`` both for CW and
TCSPC measurements (the two are distinguished with the value of
``/setup/lifetime``).

If a measurement has two spectral detection bands but is fundamentally
different from a smFRET measurement, then the "generic" ``measurement_type``
can be used. In this case the ``measurement_specs`` fields will be the same
as in the "smFRET" ``measurement_type``. In particular::

    /setup/num_spectral_ch = 2
    /setup/num_split_ch = 1
    /setup/num_polarization_ch = 1

and::

    /photon_data/measurement_specs/detectors_specs/
        spectral_ch1 = [0, ]
        spectral_ch2 = [1, ]

TCSPC measurements
^^^^^^^^^^^^^^^^^^

In all previous examples, variants with TCSPC data will be defined by
``/setup/lifetime = True`` and the field
``/photon_data/measurement_specs/laser_repetition_rate`` specifying the
laser repetition rate. When the laser is pulsed but the acquisition hardware is
not TCSPC, ``/setup/excitation_cw`` will be False and
``/setup/lifetime = False``.

2-Spectral channels + polarization
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This is the case of a smFRET measurement where the donor and acceptor emission paths
are further split into parallel and perpendicular polarization. In this case,
we have::

    /setup/num_spectral_ch = 2
    /setup/num_split_ch = 1
    /setup/num_polarization_ch = 2

Assuming that detectors 0, 1 are used for the two polarizations in
the donor channel and detectors 2, 3 for the acceptor channel, we have::

    /photon_data/measurement_specs/detectors_specs/
        spectral_ch1 = [0, 1]
        spectral_ch2 = [2, 3]
        polarization_ch1 = [0, 2]
        polarization_ch2 = [1, 3]


ALEX + polarization
^^^^^^^^^^^^^^^^^^^

In this measurement, μs-ALEX excitation and four detectors are used for the
two polarizations in the donor and acceptor paths. This is identical to the previous
example, with the difference that there are now two CW alternating lasers.

In this case, we have::

    /setup/num_spectral_ch = 2
    /setup/num_split_ch = 1
    /setup/num_polarization_ch = 2
    /setup/excitation_alternated = [True, True]

and, using the detector numbers of the previous example::

    /photon_data/measurement_specs/
        alex_period = 4000
        detectors_specs/
            spectral_ch1 = [0, 1]
            spectral_ch2 = [2, 3]
            polarization_ch1 = [0, 2]
            polarization_ch2 = [1, 3]

Note that when ``/setup/excitation_alternated`` is True then
``/photon_data/measurement_specs/alex_period`` needs to be present.

Notes on "generic" measurement_type
-----------------------------------

Here we collect a few notes on using the "generic"
measurement_type.

When at least one laser is CW and alternated
(``/setup/excitation_cw`` and ``/setup/excitation_alternated``),
then the ``photon_data/measurement_specs/alex_period`` is
mandatory.

When at least one laser is pulsed
(i.e. False in ``/setup/excitation_cw``),
then the fields
``photon_data/measurement_specs/laser_repetition_rate`` and
``/setup/laser_repetition_rates`` are mandatory.

When ``/setup/lifetime = True``, then the file will have
``/setup/laser_repetition_rates`` and
``photon_data/measurement_specs/laser_repetition_rate``.

If there is only one detector per spot ``photon_data/detectors``
array may be omitted. In this case, the lack of
``photon_data/detectors`` implies no
``photon_data/measurement_specs/detectors_specs`` group.
