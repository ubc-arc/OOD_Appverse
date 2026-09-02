# UGENE Open OnDemand App
An Open OnDemand app for running [UGENE 53.1](https://ugene.net/) as a desktop GUI application via a VNC session.

## Prerequisites
- **Apptainer** 1.3.1+
- **Open OnDemand** 2.x+
- A Slurm-based HPC cluster configured with Open OnDemand

## Container Contents
| Component | Version |
|-----------|---------|
| Base OS | Ubuntu 22.04 |
| UGENE | 53.1 |
| wmctrl / xdotool | system |
| BWA, Bowtie2, SAMtools | bundled |
| BLAST+, SPAdes, FastQC | bundled |

## Building the Container

Build the container on a system with internet access:
apptainer build ugene_53.1.sif ugene_53.1.def


## Setup
1. Move the container to a shared directory accessible from compute nodes:
mv ugene_53.1.sif /path/to/apptainer_images/ugene/ugene_53.1.sif


2. Update `cluster` in `form.yml.erb` to match your cluster id (the filename without `.yml` under `/etc/ood/config/clusters.d/`):
cluster: "mycluster"


3. Update the `.sif` path in `form.yml.erb` to point to where you placed the container:
options:
  - ["UGENE 53.1", "/path/to/apptainer_images/ugene/ugene_53.1.sif"]

4. Update `module load apptainer` in `template/script.sh` to match your cluster's module name if different.

## Running UGENE via Open OnDemand

1. Log in to your Open OnDemand instance
2. Navigate to **Interactive Apps** and select the UGENE app
3. Fill in the job parameters (account, partition, cores, memory, walltime)
4. Click **Launch**: a VNC desktop session will start with the UGENE GUI open and maximized

## Troubleshooting

**Out of memory errors:**
- Increase the number of cores or memory per core in the job form

**UGENE does not launch:**
- Verify the `.sif` path in `form.yml.erb` is correct and the file exists on the compute node

**`squashfuse` / `gocryptfs` warnings on startup:**
- These are harmless informational messages and can be safely ignored

## Contributing

Contributions are welcome. Please open an issue or submit a pull request on GitHub.

## License

- Documentation and container definition are licensed under [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/)
- UGENE is developed by [Unipro](https://unipro.ru) and released under the [GPL v2 or later](https://www.gnu.org/copyleft/gpl.html)