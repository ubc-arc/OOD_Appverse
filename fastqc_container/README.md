# FastQC Open OnDemand App
An Open OnDemand app for running [FastQC 0.12.1](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/) as a desktop GUI application via a VNC session.
Part of the [UBC ARC OOD Appverse](https://github.com/ubc-arc/OOD_Appverse) collection.


## Prerequisites
- **Apptainer** 1.3.1+
- **Open OnDemand** 2.x+
- A Slurm-based HPC cluster configured with Open OnDemand


## Container Contents
| Component | Version |
|-----------|---------|
| Base OS | Ubuntu 22.04 |
| FastQC | 0.12.1 |
| OpenJDK JRE | 17 (system) |
| Perl | system |
| wmctrl | system |
| xdotool | system |



## Building the Container
Build the container on a system with internet access:
apptainer build fastqc_0.12.1.sif fastqc_0.12.1.def


## Setup
1. Move the container to a shared directory accessible from compute nodes:
mv fastqc_0.12.1.sif /path/to/apptainer_images/fastqc/fastqc_0.12.1.sif


2. Update `cluster` in `form.yml.erb` to match your cluster id (the filename without `.yml` under `/etc/ood/config/clusters.d/`):
cluster: "mycluster"


3. Update the `.sif` path in `form.yml.erb` to point to where you placed the container:
options:
  - ["FastQC 0.12.1", "/path/to/apptainer_images/fastqc/fastqc_0.12.1.sif"]

4. Update `module load apptainer` in `script.sh.erb` to match your cluster's module name if different.



## Running FastQC via Open OnDemand
1. Log in to your Open OnDemand instance
2. Navigate to **Interactive Apps** and select the FastQC app
3. Fill in the job parameters (account, queue, cores, walltime)
4. Click **Launch**: a VNC desktop session will start with the FastQC GUI open and maximized



## Troubleshooting
**FastQC does not launch:**
- Verify the `.sif` path in `form.yml.erb` is correct and the file exists on the compute node

**Out of memory errors:**
- Increase the number of cores in the job form (memory scales with cores)

**`squashfuse` / `gocryptfs` warnings on startup:**
- These are harmless informational messages and can be safely ignored



## Contributing
Contributions are welcome. Please open an issue or submit a pull request on [GitHub](https://github.com/ubc-arc/OOD_Appverse).



## License
- Documentation and container definition are licensed under [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/)
- FastQC is developed and maintained by the [Babraham Institute](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/) and released under the [GPL v3 or later](https://www.gnu.org/copyleft/gpl.html)