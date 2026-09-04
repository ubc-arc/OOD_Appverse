# UGENE (Unified Bioinformatics Environment)

## Overview

UGENE is an Open OnDemand Batch Connect app that launches [UGENE 53.1](https://ugene.net/) as an interactive VNC desktop session on HPC clusters. It is designed for researchers who need an integrated bioinformatics toolkit for sequence analysis, alignment, phylogenetics, and NGS data processing without requiring local software installation or large file transfers.


## Features

- Launches UGENE 53.1 via VNC desktop session on compute nodes
- Supports CPU execution
- Configurable cores, memory per core, and wall time via the launch form
- Containerized via Apptainer (Ubuntu 22.04)
- UGENE window is automatically detected and maximized on launch
- External tool paths (BWA, Bowtie2, SAMtools, BLAST+, SPAdes, and more) are pre-configured automatically

## Bundled Tools

| Tool | Purpose |
|------|---------|
| BWA, Bowtie1, Bowtie2 | Read mapping |
| SAMtools, BCFtools, Tabix | SAM/BAM/VCF processing |
| BEDtools | Genome arithmetic |
| MAFFT, ClustalW, ClustalOmega, Kalign | Multiple sequence alignment |
| HMMER3, CAP3 | Alignment and assembly |
| SPAdes | De novo assembly |
| BLAST+ | Sequence search |
| Cufflinks, TopHat2, StringTie | RNA-seq analysis |
| FastQC | Quality control |
| FastTree, IQ-TREE, MrBayes, PhyML | Phylogenetics |
| Kraken2 | Metagenomics |
| Cutadapt | Adapter trimming |
| mfold, Spidey, SnpEff | RNA folding, alignment, annotation |

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

Please see the [References section](#software-installation) below for instructions on how to build the UGENE container launched by this app.

### 1. Clone the repository


cd /var/www/ood/apps/sys
git clone https://github.com/ubc-arc/OOD_Appverse.git
cd OOD_Appverse/ugene_appverse
git checkout v0.1.0


### 2. Configure for your site

#### form.yml Attributes

Edit `form.yml` and update these values for your cluster:

| Attribute | Description | Default |
|-----------|-------------|---------|
| `cluster` | Target cluster ID | `"mycluster"` |
| `bc_num_hours` max | Maximum wall time (hours) | `24` |
| `bc_num_slots` max | Maximum cores | `64` |
| `mem_per_cpu` max | Maximum memory per core (GB) | `64` |
| version option path | Path to ugene_53.1.sif on compute nodes | `"/path/to/apptainer_images/ugene/ugene_53.1.sif"` |

#### manifest.yml Attributes

Edit `manifest.yml` and update these values for your organization:

| Attribute | Change to |
|-----------|-----------|
| `description` | Your cluster name and your documentation URL |

#### template/script.sh.erb

Update the `module load` line to match your cluster's module names:


module load apptainer


### 3. Verify

No OOD restart is needed. Visit your OOD dashboard and look for **UGENE** under **Interactive Apps > Genomics**.

## Troubleshooting

### UGENE does not launch

1. Check the job's `output.log` in `~/ondemand/data/sys/ugene_appverse/`
2. Verify the `.sif` path in `form.yml` is correct and the file exists on the compute node
3. Verify the Xfce window manager is installed: `which xfwm4`

### squashfuse / gocryptfs warnings on startup

These are harmless informational messages from Apptainer and can be safely ignored.

### External tools not found in UGENE

External tool paths are pre-configured automatically via the launcher script. If tools are not found, go to **Settings > Preferences > External Tools** and verify the paths match those in `ugene.def`.

## Testing

| Site | OOD Version | Scheduler | Status |
|------|-------------|-----------|--------|
| UBC ARC Sockeye | 3.x | Slurm | Tested |

To verify your installation:

1. Launch the app from the OOD dashboard with default settings
2. Confirm the UGENE GUI loads and is maximized in the VNC desktop session
3. Open a sample sequence file and confirm it is displayed correctly

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

- [UGENE](https://ugene.net/) — the application launched by this OOD app
- [UGENE License (GPL v2+)](https://www.gnu.org/copyleft/gpl.html)
- [Open OnDemand](https://openondemand.org/) — the HPC portal framework
- [Appverse Contributor Guide](https://github.com/Sweet-and-Fizzy/ood-appverse/blob/main/docs/appverse-contributor-guide.md)


### Software Installation

The UGENE container is built from the `ugene.def` Apptainer definition file included in this repository. It bundles UGENE 53.1 on Ubuntu 22.04 with all bundled tools pre-configured.

**Build the container on a system with internet access (e.g. a login node):**


apptainer build --fakeroot ugene_53.1.sif ugene.def


**Move the container to a shared directory accessible from compute nodes:**


mv ugene_53.1.sif /path/to/apptainer_images/ugene/ugene_53.1.sif


## License

Code is licensed under [MIT](LICENSE)

Documentation is licensed under [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/)

UGENE is developed by [Unipro](https://unipro.ru) and released under the [GPL v2 or later](https://www.gnu.org/copyleft/gpl.html).
