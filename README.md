# snappergps-data-2

*Author: Jonas Beuchert*

## Table of contents

- [Overview](#overview)
- [Data](#data)
- [Smoothing](#smoothing)
- [Acknowledgements](#acknowledgements)


## Smoothing

Table: Median horizontal localisation errors of different smoothing algorithms considering different sampling intervals and travel modes. (RTS smoother: Rauch-Tung-Striebel smoother. GPR: Gaussian process regression. FGO: factor graph optimisation.) Two-dimensional (2D) smoothers and tight FGO use a constant-position-random-velocity model, except for GPR, which uses adaptive models for the two spatial dimensions. The 4D smoother uses a constant-velocity-random-acceleration model. All estimations are loosely-coupled (snapshot positioning followed by smoothing) except for tight FGO. Localisation errors without smoothing are provided for comparison.

| Algorithm         | Walking          |            |            | Cycling          |            |            |
|-------------------|------------------|------------|------------|------------------|------------|------------|
|                   | 1 s              | 10 s       | 60 s       | 1 s              | 10 s       | 60 s       |
| No smoothing      | 15.4 m           | 15.4 m     | 14.5 m     | 14.5 m           | 14.5 m     | 14.6 m     |
| 2D RTS smoother   | 8.2 m            | 10.5 m     | 13.5 m     | 7.7 m            | 11.8 m     | 14.5 m     |
| 4D RTS smoother   | 8.1 m            | 10.2 m     | 13.7 m     | 7.4 m            | 11.3 m     | 14.6 m     |
| 2D GPR            | 7.9 m            | 11.0 m     | 14.7 m     | 7.5 m            | 12.0 m     | 19.8 m     |
| 2D FGO            | 8.1 m            | 10.4 m     | 13.6 m     | 7.5 m            | 11.6 m     | 14.6 m     |
| Tight FGO         | 7.9 m            | 10.0 m     | 13.5 m     | 7.3 m            | 11.3 m     | 14.6 m     |

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