# MEGA (Molecular Evolutionary Genetics Analysis)

## Overview

MEGA is an Open OnDemand Batch Connect app that launches [MEGA 12.1.2](https://www.megasoftware.net/) as an interactive VNC desktop session on HPC clusters. It is designed for researchers who need to perform molecular evolutionary genetics analysis including phylogenetic tree construction, sequence alignment, and evolutionary distance estimation without requiring local software installation.


## Features

- Launches MEGA 12.1.2 via VNC desktop session on compute nodes
- Supports CPU execution
- Configurable cores, memory per core, and wall time via the launch form
- Containerized via Apptainer (Ubuntu 22.04, GTK2 and CEF bundled)
- MEGA window is automatically detected and maximized on launch
- Includes wmctrl and xdotool bundled in the container

## Requirements

### Compute Node Software

- Apptainer 1.3.1+
- Xfce window manager
- Ubuntu 22.04 base OS

### Open OnDemand

- Open OnDemand 2.x+
- Slurm scheduler
- Lmod or Environment Modules

### Prerequisites

The MEGA `.deb` package must be downloaded separately before building the container. Download it from [megasoftware.net](https://www.megasoftware.net/) and place it in the `mega_appverse/` directory as `mega.deb` before running the build command.

## App Installation

Please see the [References section](#software-installation) below for instructions on how to build the MEGA container launched by this app.

### 1. Clone the repository


cd /var/www/ood/apps/sys
git clone https://github.com/ubc-arc/OOD_Appverse.git
cd OOD_Appverse/mega_appverse
git checkout v0.1.0


### 2. Configure for your site

#### form.yml Attributes

Edit `form.yml` and update these values for your cluster:

| Attribute | Description | Default |
|-----------|-------------|---------|
| `cluster` | Target cluster ID | `"mycluster"` |
| `bc_num_hours` max | Maximum wall time (hours) | `3` |
| `bc_num_slots` max | Maximum cores | `40` |
| `mem_per_cpu` max | Maximum memory per core (GB) | `64` |
| version option path | Path to mega12_gui.sif on compute nodes | `"/path/to/apptainer_images/mega/mega12_gui.sif"` |

#### manifest.yml Attributes

Edit `manifest.yml` and update these values for your organization:

| Attribute | Change to |
|-----------|-----------|
| `description` | Your cluster name and your documentation URL |

#### template/script.sh.erb

Update the `module load` line to match your cluster's module name:

module load apptainer


### 3. Verify

No OOD restart is needed. Visit your OOD dashboard and look for **MEGA** under **Interactive Apps > Genomics**.

## Troubleshooting

### MEGA does not launch

1. Check the job's `output.log` in `~/ondemand/data/sys/mega_appverse/`
2. Verify the `.sif` path in `form.yml` is correct and the file exists on the compute node
3. Verify the Xfce window manager is installed: `which xfwm4`


### squashfuse / gocryptfs warnings on startup

These are harmless informational messages from Apptainer and can be safely ignored.

## Testing

| Site | OOD Version | Scheduler | Status |
|------|-------------|-----------|--------|
| UBC ARC Sockeye | 2.x | Slurm | Tested |

To verify your installation:

1. Launch the app from the OOD dashboard with default settings
2. Confirm the MEGA GUI loads and is maximized in the VNC desktop session
3. Open a sample alignment file and confirm the phylogenetic tree is displayed

## Known Limitations

- Only tested on Ubuntu 22.04 base OS
- Internet access is required on the login node to build the container; compute nodes do not require it


## Contributing

Contributions are welcome. To contribute:

1. Fork this repository
2. Create a feature branch (`git checkout -b feature/my-improvement`)
3. Submit a pull request with a description of your changes

For bugs or feature requests, [open an issue](https://github.com/ubc-arc/OOD_Appverse/issues).

This app is part of the [OOD Appverse](https://openondemand.connectci.org/affinity-groups/ood-appverse). Join the [Appverse Affinity Group](https://openondemand.connectci.org/affinity-groups/ood-appverse) to connect with other contributors.

## References

- [MEGA](https://www.megasoftware.net/) — the application launched by this OOD app
- [Open OnDemand](https://openondemand.org/) — the HPC portal framework
- [Appverse Contributor Guide](https://github.com/Sweet-and-Fizzy/ood-appverse/blob/main/docs/appverse-contributor-guide.md)


### Software Installation

The MEGA container is built from the `mega12_gui.def` Apptainer definition file included in this repository. It bundles MEGA 12.1.2 on Ubuntu 22.04 with all required GTK2 and CEF dependencies, wmctrl, and xdotool.

**Prerequisites:** Download the MEGA `.deb` package from [megasoftware.net](https://www.megasoftware.net/) and place it in the `mega_appverse/` directory as `mega.deb`.

**Build the container on a system with internet access (e.g. a login node):**

apptainer build --fakeroot mega12_gui.sif mega12_gui.def

**Move the container to a shared directory accessible from compute nodes:**


mv mega12_gui.sif /path/to/apptainer_images/mega/mega12_gui.sif


## License

Code is licensed under [MIT](LICENSE)

Documentation is licensed under [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/)

MEGA is developed and maintained by the [Kumar Lab at Temple University](https://www.megasoftware.net/) and is free for research and educational use. 
