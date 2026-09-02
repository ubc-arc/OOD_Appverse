# Bandage (Assembly Graph Viewer)

## Overview

Bandage is an Open OnDemand Batch Connect app that launches [Bandage 0.9.0](https://github.com/rrwick/Bandage) as an interactive VNC desktop session on HPC clusters. It is designed for researchers who need to visually explore genome assembly graphs (GFA/FASTG format) without requiring local software installation or large file transfers.

- Upstream project: [Bandage - Ryan Wick](https://github.com/rrwick/Bandage)

## Screenshots

![Bandage running in browser](docs/screenshot.png)

## Features

- Launches Bandage 0.9.0 via VNC desktop session on compute nodes
- Supports CPU execution
- Configurable cores, wall time, and VNC resolution via the launch form
- Containerized via Apptainer (Ubuntu 22.04)
- Bandage window is automatically detected and maximized on launch
- Includes BLAST+ and xdotool bundled in the container

## Requirements

### Compute Node Software

- Apptainer 1.3.1+
- Xfce window manager
- Ubuntu 22.04 base OS

### Open OnDemand

- Open OnDemand 2.x+
- Slurm scheduler
- Lmod or Environment Modules

## App Installation

Please see the [References section](#software-installation) below for instructions on how to build the Bandage container launched by this app.

### 1. Clone the repository


cd /var/www/ood/apps/sys
git clone https://github.com/ubc-arc/OOD_Appverse.git
cd OOD_Appverse/bandage_appverse
git checkout v0.1.0


### 2. Configure for your site

#### form.yml Attributes

Edit `form.yml` and update these values for your cluster:

| Attribute | Description | Default |
|-----------|-------------|---------|
| `cluster` | Target cluster ID | `"mycluster"` |
| `bc_num_hours` max | Maximum wall time (hours) | `24` |
| `bc_num_slots` max | Maximum cores | `64` |
| version option path | Path to bandage.sif on compute nodes | `"/path/to/apptainer_images/bandage/bandage.sif"` |

#### manifest.yml Attributes

Edit `manifest.yml` and update these values for your organization:

| Attribute | Change to |
|-----------|-----------|
| `description` | Your cluster name and your documentation URL |

#### template/script.sh.erb

Update the `module load` line to match your cluster's module name:


module load apptainer


### 3. Verify

No OOD restart is needed. Visit your OOD dashboard and look for **Bandage** under **Interactive Apps > Genomics**.

## Troubleshooting

### Bandage does not launch

1. Check the job's `output.log` in `~/ondemand/data/sys/bandage_appverse/`
2. Verify the `.sif` path in `form.yml` is correct and the file exists on the compute node
3. Verify the Xfce window manager is installed: `which xfwm4`

### Module not found error

Run `module spider apptainer` to find the correct module name for your cluster and update `template/script.sh.erb` accordingly.

### Out of memory errors

The default memory is 4GB per core. Request more cores in the launch form to scale memory, or increase the `--mem-per-cpu` value in `submit.yml.erb`.

### Bandage window does not maximize

The script polls up to 30 times (60 seconds) for the Bandage window to appear. If your cluster is slow to start the container, increase the loop count in `template/script.sh.erb`.

## Testing

| Site | OOD Version | Scheduler | Status |
|------|-------------|-----------|--------|
| UBC ARC Sockeye | 2.x | Slurm | Tested |

To verify your installation:

1. Launch the app from the OOD dashboard with default settings
2. Confirm the Bandage GUI loads and is maximized in the VNC desktop session
3. Load a GFA or FASTG file and confirm the assembly graph is displayed

## Known Limitations

- Only tested on Ubuntu 22.04 base OS
- Only CPU execution is supported; no GPU rendering
- Internet access is required on the login node to build the container; compute nodes do not require it
- The xdotool window maximization may not work on all VNC configurations

## Contributing

Contributions are welcome. To contribute:

1. Fork this repository
2. Create a feature branch (`git checkout -b feature/my-improvement`)
3. Submit a pull request with a description of your changes

For bugs or feature requests, [open an issue](https://github.com/ubc-arc/OOD_Appverse/issues).

This app is part of the [OOD Appverse](https://openondemand.connectci.org/affinity-groups/ood-appverse). Join the [Appverse Affinity Group](https://openondemand.connectci.org/affinity-groups/ood-appverse) to connect with other contributors.

## References

- [Bandage](https://github.com/rrwick/Bandage) — the application launched by this OOD app
- [Bandage License (GPL v3)](https://www.gnu.org/licenses/gpl-3.0.html)
- [Open OnDemand](https://openondemand.org/) — the HPC portal framework
- [Appverse Contributor Guide](https://github.com/Sweet-and-Fizzy/ood-appverse/blob/main/docs/appverse-contributor-guide.md)
- [Appverse README Template](https://github.com/tamu-edu/appverse_readme_template)

### Software Installation

The Bandage container is built from the `bandage.def` Apptainer definition file included in this repository. It bundles Bandage 0.9.0 built from source on Ubuntu 22.04, along with BLAST+ and xdotool.

**Build the container on a system with internet access (e.g. a login node):**


apptainer build --fakeroot bandage.sif bandage.def


**Move the container to a shared directory accessible from compute nodes:**


mv bandage.sif /path/to/apptainer_images/bandage/bandage.sif


## License

Code is licensed under [MIT](LICENSE)

Documentation is licensed under [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/)

Bandage is developed by [Ryan Wick](https://github.com/rrwick) and released under the [GPL v3](https://www.gnu.org/licenses/gpl-3.0.html).
