# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](http://keepachangelog.com/en/1.0.0/)
and this project adheres to [Semantic Versioning](http://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [0.1.0] - 2025-03-25
### Added
- Initial release of MEGA Open OnDemand app.
- Apptainer container (`mega12_gui.sif`) bundling MEGA 12.1.2 on Ubuntu 22.04.
- VNC-based desktop session using Xfce window manager.
- Full GTK2 and Chromium Embedded Framework (CEF) dependency stack for the MEGA built-in browser.
- Adwaita light GTK2 theme set by default to avoid dark-theme display artefacts.
- Form options for allocation code, number of cores, and session duration.
- Support for MEGA GUI and command-line core (megacc) within the same container.

[Unreleased]: https://github.com/ubc-arc/OOD_Appverse