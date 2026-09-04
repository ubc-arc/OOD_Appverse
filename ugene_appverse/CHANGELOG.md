# Changelog
All notable changes to this project will be documented in this file.
The format is based on [Keep a Changelog](http://keepachangelog.com/en/1.0.0/)
and this project adheres to [Semantic Versioning](http://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [0.1.0] - 2026-03-26
### Added
- Initial release of UGENE Open OnDemand app.
- Apptainer container (ugene_53.1.sif) bundling UGENE 53.1 on Ubuntu 22.04.
- VNC-based desktop session using Xfce window manager.
- Form options for allocation code, number of cores, memory per core, and session duration.
- Support for UGENE interactive GUI via X11/VNC within the container.
- Window auto-maximization on launch using xdotool and wmctrl.
- Pre-configured external tool paths (BWA, Bowtie2, SAMtools, BLAST+, SPAdes, and more).

[Unreleased]: https://github.com/ubc-arc/OOD_Appverse/compare/v0.1.0...HEAD
[0.1.0]: https://github.com/ubc-arc/OOD_Appverse/releases/tag/v0.1.0
