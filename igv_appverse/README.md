# IGV (Integrative Genomics Viewer)

## Overview

IGV is an Open OnDemand Batch Connect app that launches [IGV 2.17.0](https://igv.org) as an interactive VNC desktop session on HPC clusters. It is designed for researchers who need to visually explore and validate genomic data including read alignments, variants, and gene annotations.


## Features

- Launches IGV 2.17.0 via VNC desktop session on compute nodes
- Supports CPU execution
- Configurable cores and wall time via the launch form
- Containerized via Apptainer (Ubuntu 22.04, Java 17 bundled with IGV)
- Supports custom reference genomes via local genome JSON specification files
- Clean VNC desktop with Xfce window manager stripped to IGV-only view

## Requirements

### Compute Node Software

- Apptainer 1.3.1+
- Xfce window manager
- Ubuntu 22.04 base OS

### Open OnDemand

- Open OnDemand 3.x+
- Slurm scheduler
- Lmod or Environment Modules

### Optional

- Custom genome files downloaded to a shared directory accessible from compute nodes (see [Software Installation](#software-installation))

## App Installation

Please see the [References section](#software-installation) below for instructions on how to build the IGV container launched by this app.

### 1. Clone the repository


cd /var/www/ood/apps/sys
git clone https://github.com/ubc-arc/OOD_Appverse.git
cd OOD_Appverse/igv_appverse
git checkout v0.1.0


### 2. Configure for your site

#### form.yml Attributes

Edit `form.yml` and update these values for your cluster:

| Attribute | Description | Default |
|-----------|-------------|---------|
| `cluster` | Target cluster ID | `"mycluster"` |
| `bc_num_hours` max | Maximum wall time (hours) | `3` |
| `bc_num_slots` max | Maximum cores | `40` |
| version option path | Path to igv.sif on compute nodes | `"/path/to/apptainer_images/igv/igv.sif"` |

#### manifest.yml Attributes

Edit `manifest.yml` and update these values for your organization:

| Attribute | Change to |
|-----------|-----------|
| `description` | Your cluster name and your documentation URL |

#### Environment variables

| Variable | Required | Description |
|----------|----------|-------------|
| `genomeServerURL` | No | Path to a local `genomes.tsv` file for custom genome server |

### 3. Verify

No OOD restart is needed. Visit your OOD dashboard and look for **IGV** under **Interactive Apps > Genomics**.

## Troubleshooting

### Job starts but IGV does not appear

1. Check the job's `output.log` in `~/ondemand/data/sys/igv_appverse/`
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
2. Confirm the IGV GUI loads in the VNC desktop session
3. Load a BAM file from your project directory and confirm reads are displayed

## Known Limitations

- Only tested on Ubuntu 22.04 base OS
- Custom genome files must be downloaded and configured locally on the cluster before use — compute nodes do not have outbound internet access
- Internet access is required on the login node to build the container


## Contributing

Contributions are welcome. To contribute:

1. Fork this repository
2. Create a feature branch (`git checkout -b feature/my-improvement`)
3. Submit a pull request with a description of your changes

For bugs or feature requests, [open an issue](https://github.com/ubc-arc/OOD_Appverse/issues).

This app is part of the [OOD Appverse](https://openondemand.connectci.org/affinity-groups/ood-appverse). Join the [Appverse Affinity Group](https://openondemand.connectci.org/affinity-groups/ood-appverse) to connect with other contributors.

## References

- [IGV](https://igv.org) — the application launched by this OOD app
- [IGV License](https://github.com/igvteam/igv/blob/master/license.txt)
- [Open OnDemand](https://openondemand.org/) — the HPC portal framework
- [Appverse Contributor Guide](https://github.com/Sweet-and-Fizzy/ood-appverse/blob/main/docs/appverse-contributor-guide.md)


### Software Installation

The IGV container is built from the `igv.def` Apptainer definition file included in this repository. It bundles IGV 2.17.0 with Java 17 on Ubuntu 22.04.

**Build the container on a system with internet access (e.g. a login node):**


apptainer build igv.sif igv.def


**Move the container to a shared directory accessible from compute nodes:**


mv igv.sif /path/to/apptainer_images/igv/igv.sif


**To configure a custom genome** (for organisms other than the default hg38), download the required genome files and create a JSON specification file. 


# Download genome files from UCSC
wget https://hgdownload.soe.ucsc.edu/goldenPath/sacCer3/bigZips/sacCer3.fa.gz
wget https://hgdownload.soe.ucsc.edu/goldenPath/sacCer3/bigZips/sacCer3.chromAlias.txt
gunzip sacCer3.fa.gz
apptainer exec /path/to/igv.sif samtools faidx sacCer3.fa


Then create a `genome.json` file with absolute paths to the downloaded files and load it in IGV via **Genomes > Load Genome from File**.

## License

Code is licensed under [MIT](LICENSE)

Documentation is licensed under [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/)

IGV is developed and maintained by the [Broad Institute](https://igv.org) and licensed separately — see the [IGV license](https://github.com/igvteam/igv/blob/master/license.txt) for terms of use.