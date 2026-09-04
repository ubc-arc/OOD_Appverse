# FastQC (Quality Control for High Throughput Sequence Data)

## Overview

FastQC is an Open OnDemand Batch Connect app that launches [FastQC 0.12.1](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/) as an interactive VNC desktop session on HPC clusters. It is designed for researchers who need to run quality control checks on high throughput sequencing data without requiring local software installation or large file transfers.


## Features

- Launches FastQC 0.12.1 via VNC desktop session on compute nodes
- Supports CPU execution
- Configurable cores and wall time via the launch form
- Containerized via Apptainer (Ubuntu 22.04, OpenJDK 17 bundled)
- FastQC window is automatically detected and maximized on launch
- Includes wmctrl and xdotool bundled in the container

## Requirements

### Compute Node Software

- Apptainer 1.3.1+
- Xfce window manager
- Ubuntu 22.04 base OS

### Open OnDemand

- Open OnDemand 3.x+
- Slurm scheduler
- Lmod or Environment Modules

## App Installation

Please see the [References section](#software-installation) below for instructions on how to build the FastQC container launched by this app.

### 1. Clone the repository


cd /var/www/ood/apps/sys
git clone https://github.com/ubc-arc/OOD_Appverse.git
cd OOD_Appverse/fastqc_appverse
git checkout v0.1.0


### 2. Configure for your site

#### form.yml Attributes

Edit `form.yml` and update these values for your cluster:

| Attribute | Description | Default |
|-----------|-------------|---------|
| `cluster` | Target cluster ID | `"mycluster"` |
| `bc_num_hours` max | Maximum wall time (hours) | `24` |
| `bc_num_slots` max | Maximum cores | `64` |
| version option path | Path to fastqc_0.12.1.sif on compute nodes | `"/path/to/apptainer_images/fastqc/fastqc_0.12.1.sif"` |

#### manifest.yml Attributes

Edit `manifest.yml` and update these values for your organization:

| Attribute | Change to |
|-----------|-----------|
| `description` | Your cluster name and your documentation URL |

#### template/script.sh.erb

Update the `module load` line to match your cluster's module names:


module load apptainer


### 3. Verify

No OOD restart is needed. Visit your OOD dashboard and look for **FastQC** under **Interactive Apps > Genomics**.

## Troubleshooting

### FastQC does not launch

1. Check the job's `output.log` in `~/ondemand/data/sys/fastqc_appverse/`
2. Verify the `.sif` path in `form.yml` is correct and the file exists on the compute node
3. Verify the Xfce window manager is installed: `which xfwm4`

### squashfuse / gocryptfs warnings on startup

These are harmless informational messages from Apptainer and can be safely ignored.


## Testing

| Site | OOD Version | Scheduler | Status |
|------|-------------|-----------|--------|
| UBC ARC Sockeye | 3.x | Slurm | Tested |

To verify your installation:

1. Launch the app from the OOD dashboard with default settings
2. Confirm the FastQC GUI loads and is maximized in the VNC desktop session
3. Load a FASTQ file and confirm the quality report is displayed

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

- [FastQC](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/) — the application launched by this OOD app
- [FastQC License (GPL v3)](https://www.gnu.org/copyleft/gpl.html)
- [Open OnDemand](https://openondemand.org/) — the HPC portal framework
- [Appverse Contributor Guide](https://github.com/Sweet-and-Fizzy/ood-appverse/blob/main/docs/appverse-contributor-guide.md)


### Software Installation

The FastQC container is built from the `fastqc_0_12_1.def` Apptainer definition file included in this repository. It bundles FastQC 0.12.1 on Ubuntu 22.04 with OpenJDK 17, wmctrl, and xdotool.

**Build the container on a system with internet access (e.g. a login node):**


apptainer build --fakeroot fastqc_0.12.1.sif fastqc_0_12_1.def


**Move the container to a shared directory accessible from compute nodes:**

mv fastqc_0.12.1.sif /path/to/apptainer_images/fastqc/fastqc_0.12.1.sif


## License

Code is licensed under [MIT](LICENSE)

Documentation is licensed under [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/)

FastQC is developed and maintained by the [Babraham Institute](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/) and released under the [GPL v3 or later](https://www.gnu.org/copyleft/gpl.html).
