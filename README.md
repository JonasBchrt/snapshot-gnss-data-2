# snapshot-gnss-data-2

*Author: Jonas Beuchert*

Lots of brief GNSS signal snapshots to develop and test snapshot GNSS satellite acquisition and positioning algorithms.

## Table of contents

- [Overview](#overview)
- [Data](#data)
- [Smoothing](#smoothing)
- [Citation](#citation)
- [Acknowledgements](#acknowledgements)

## Overview

This repository contains two new dynamic GNSS satellite signal snapshot datasets.
These were created by carrying [SnapperGPS receivers](https://snappergps.info/) while walking or cycling in Oxfordshire, UK.

* _Walking_: 15 individual recordings/tracks with 26,423 datapoints in total captured in one-second intervals totalling 7:20 hours. Motion during these recordings was slower on average, but acceleration, deceleration, and directional changes were more frequent and usually sudden.
* _Cycling_: Twelve individual recordings/tracks with 27,237 datapoints in total captured in one-second intervals totalling 7:34 hours. Motion during these recordings was on average faster, but acceleration, deceleration, and directional changes were less frequent and usually smooth.

[SnapperGPS V1.0.0 boards](https://github.com/SnapperGPS/snappergps-pcb) were used for 18 recordings and [SnapperGPS V2.0.0 boards](https://github.com/SnapperGPS/snappergps-pcb-2) for nine recordings.
The more expensive Siretta Echo 27 active patch antenna that was already used for [a previous data collection](https://doi.org/10.5287/bodleian:eXrp1xydM) was only used for six of these recordings.
In addition, either the low-cost Abracon APAM2764YK0175 active patch antenna (twelve recordings) or low-cost Taoglas GP.1575.25.2.A.02 (five recordings) or GP.1575.25.4.A.02 (four recordings) passive patch antennas were used.

The core component of a low-cost SnapperGPS receiver is an [SE4150L](https://www.skyworksinc.com/Products/Amplifiers/SE4150L) integrated GPS receiver circuit. Like most civilian low-cost GPS receivers, SnapperGPS operates in the L1 band with a centre frequency of 1.57542 GHz. However, Galileo's E1 signal, BeiDou's B1C signal, GPS' novel L1C signal, and SBAS' L1 signal have the identical centre frequency. So, a SnapperGPS receiver captures those signals, too. A SnapperGPS receiver down-mixes the incoming signal to a nominal intermediate frequency of 4.092 MHz, samples the resulting near-baseband signal at 4.092 MHz and digitises it with an amplitude resolution of one bit per sample. It considers only the in-phase component and discards the quadrature component.

The datasets are expected to be more challenging than [the previous data collection](https://doi.org/10.5287/bodleian:eXrp1xydM).
Firstly, due to the use of lower-cost antennas and, secondly, due to more track sections in forests and along tree-lined avenues.
The latter is likely to reduce the number of visible satellites and the signal strengths of visible ones.

Ground truth was collected with real-time kinematic (RTK) receivers (u-blox ZED-F9P) with persistent cellular connections to a base station to receive assistance data and perform differential GNSS for improved accuracy.

## Data

The data is provided in JSON format.

* `/snapshots`: Raw twelve-millisecond GNSS signal snapshots from the SnapperGPS receivers stored in the same data format that is used for data transfers from SnapperGPS boards to the SnapperGPS app/website. The field `snapshots` is an array each of which elements has the fields `timestamp`, `temperature`, and `data`. `data` is a Base64 encoded byte array. One byte of the decoded array holds the amplitude values of eight signal samples, i.e., the first byte holds the first eight samples. A zero bit represents a signal amplitude of +1 and a one bit a signal amplitude of -1. The order of the bits is 'little', i.e., reversed. For example, the byte `0b01100000` corresponds to the signal chunk `[1  1  1  1  1 -1 -1  1]`.
* `/fixes`: Fixes calculated with [the SnapperGPS app/website](https://snappergps.info) (without smoothing) stored in the same data format that is used for data downloads from the SnapperGPS app/website. The data comes in array each of which elements has the fields `datetime`, `latitude`, `longitude`, `confidence`, `temperature`, and `id`, the first four of which are estimated. `datetime` is in UTC, `latitude` and `longitude` are in decimal degrees, and `confidence` is a confidence radius (50%) in metres.
* `/ground-truth`: Ground truth fixes from an RTK receiver stored in array each of which elements has the fields `time`, `latitude`, and `longitude`. `time` is in UTC, `latitude` and `longitude` are in decimal degrees.
* `/smoothed`: Plots of non-smoothed and smoothed tracks (see [below](#smoothing)). Non-smoothed fixes (pink), 2D RTS smoothing (dark gray), 2D GPR (purple), 2D loosely-coupled FGO (light gray), and tightly-coupled FGO (white).

Table 1: Overview of recordings.

| dataset   | type     | duration [min] | environment | SnapperGPS board | antenna |
|-----------|----------|----------------|-------------|------------------|---------|
| w00       | walking  |              5 | park        |           V1.0.0 |         |
| w01       | walking  |              9 | football pitch |        V1.0.0 |         |
| w02       | walking  |              8 | American football pitch | V1.0.0 |       |
| w03       | walking  |             20 | park        |           V1.0.0 |         |
| w04       | walking  |             20 | park        |           V1.0.0 |         |
| w05       | walking  |             20 | park        |           V1.0.0 |         |
| w06       | walking  |             10 | urban       |           V1.0.0 |         |
| w07       | walking  |             30 | meadow      |           V1.0.0 |         |
| w08       | walking  |             30 | meadow      |           V1.0.0 |         |
| w09       | walking  |             45 | meadow      |           V1.0.0 |         |
| w10       | walking  |             45 | meadow      |           V1.0.0 |         |
| w11       | walking  |             50 | park        |           V2.0.0 |         |
| w12       | walking  |             50 | park        |           V2.0.0 |         |
| w13       | walking  |             50 | park        |           V2.0.0 |         |
| w14       | walking  |             50 | park        |           V2.0.0 |         |
| c00       | cycling  |             60 | urban+rural |           V1.0.0 |         |
| c01       | cycling  |             60 | urban+rural |           V1.0.0 |         |
| c02       | cycling  |             60 | urban+rural |           V1.0.0 |         |
| c03       | cycling  |             35 | urban+rural |           V1.0.0 |         |
| c04       | cycling  |             35 | urban+rural |           V1.0.0 |         |
| c05       | cycling  |             35 | urban+rural |           V2.0.0 |         |
| c06       | cycling  |             40 | urban+rural |           V1.0.0 |         |
| c07       | cycling  |             40 | urban+rural |           V2.0.0 |         |
| c08       | cycling  |             40 | urban+rural |           V1.0.0 |         |
| c09       | cycling  |             16 | urban+rural |           V2.0.0 |         |
| c10       | cycling  |             16 | urban+rural |           V2.0.0 |         |
| c11       | cycling  |             16 | urban+rural |           V2.0.0 |         |

## Smoothing

Table 2: Median horizontal localisation errors of different smoothing algorithms considering different sampling intervals and travel modes. (RTS smoother: Rauch-Tung-Striebel smoother. GPR: Gaussian process regression. FGO: factor graph optimisation.) Two-dimensional (2D) smoothers and tight FGO use a constant-position-random-velocity model, except for GPR, which uses adaptive models for the two spatial dimensions. The 4D smoother uses a constant-velocity-random-acceleration model. All estimations are loosely-coupled (snapshot positioning followed by smoothing) except for tight FGO. Localisation errors without smoothing are provided for comparison.

| Algorithm         | Walking          |            |            | Cycling          |            |            |
|-------------------|------------------|------------|------------|------------------|------------|------------|
|                   | 1 s              | 10 s       | 60 s       | 1 s              | 10 s       | 60 s       |
| No smoothing      | 15.4 m           | 15.4 m     | 14.5 m     | 14.5 m           | 14.5 m     | 14.6 m     |
| 2D RTS smoother   | 8.2 m            | 10.5 m     | 13.5 m     | 7.7 m            | 11.8 m     | 14.5 m     |
| 4D RTS smoother   | 8.1 m            | 10.2 m     | 13.7 m     | 7.4 m            | 11.3 m     | 14.6 m     |
| 2D GPR            | 7.9 m            | 11.0 m     | 14.7 m     | 7.5 m            | 12.0 m     | 19.8 m     |
| 2D FGO            | 8.1 m            | 10.4 m     | 13.6 m     | 7.5 m            | 11.6 m     | 14.6 m     |
| Tight FGO         | 7.9 m            | 10.0 m     | 13.5 m     | 7.3 m            | 11.3 m     | 14.6 m     |

## Citation

If you use this dataset for work that results in a publication, consider citing one or more of the following:

For this dataset:

> Jonas Beuchert. 2023. SnapperGPS: Collection of GNSS Signal Snapshots 2. University of Oxford, Oxford, UK. [http://dx.doi.org/10.5287/ora-xq5b8xva7)](http://dx.doi.org/10.5287/ora-xq5b8xva7).

For the hardware used to create this dataset:

> Jonas Beuchert, Amanda Matthes, and Alex Rogers. 2023. SnapperGPS: Open Hardware for Energy-Efficient, Low-Cost Wildlife Location Tracking with Snapshot GNSS. Journal of Open Hardware, 7(1): 2, pp. 1–13. [https://doi.org/10.5334/joh.48](https://openhardware.metajnl.com/articles/10.5334/joh.48/).

For the software/algorithms to create the baseline fixes:

> Jonas Beuchert and Alex Rogers. 2021. SnapperGPS: Algorithms for Energy-Efficient Low-Cost Location Estimation Using GNSS Signal Snapshots. In SenSys ’21: ACM Conference on Embedded Networked Sensor Systems, November, 2021, Coimbra, Portugal. ACM, New York, NY, USA, 13 pages. [https://doi.org/10.1145/3485730.3485931](https://doi.org/10.1145/3485730.3485931).

## Acknowledgements

[Jonas Beuchert](https://users.ox.ac.uk/~kell5462/)
is based
in the Department of Computer Science
of the University of Oxford.

Jonas Beuchert is
funded by the EPSRC Centre for Doctoral Training in
Autonomous Intelligent Machines and Systems
(University of Oxford Project Code: DFT00350-DF03.01, UKRI/EPSRC Grant Reference: EP/S024050/1)
and works on
SnapperGPS as part of his doctoral studies.

##

This documentation is licensed under a
[Creative Commons Attribution 4.0 International License][cc-by].

[![CC BY 4.0][cc-by-image]][cc-by]

[cc-by]: http://creativecommons.org/licenses/by/4.0/
[cc-by-image]: https://i.creativecommons.org/l/by/4.0/88x31.png
