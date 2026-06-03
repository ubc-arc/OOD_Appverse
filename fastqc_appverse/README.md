# FastQC Apptainer Container
An Apptainer container for running [FastQC 0.12.1](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/) as a desktop GUI application on Open OnDemand.

## Prerequisites
The following are required on the Sockeye cluster:
- **Apptainer** 1.3.1
- **Open OnDemand** for launching the FastQC GUI through the web portal

## Container Contents
| Component | Version |
|-----------|---------|
| Base OS | Ubuntu 22.04 |
| FastQC | 0.12.1 |
| OpenJDK JRE | 17 (system) |
| Perl | system |

## Building the Container
Build the container on a **login node**, which has internet access required to pull the base Ubuntu image and download the FastQC zip. Compute nodes on the cluster may not have outbound internet access.

Copy the definition file (`fastqc_0.12.1.def`) to your working directory, then build:

# Load required modules
module load gcc apptainer

# Build the container (run on node with internet access)
apptainer build fastqc_0.12.1.sif /full/path/to/fastqc_0.12.1.def


Once built, move the container to a shared software directory:

mv fastqc_0.12.1.sif /full/path/to/software/apptainer_images/fastqc/


## Running FastQC via Open OnDemand
1. Log in to your Open OnDemand portal
2. Navigate to **Interactive Apps** and select the FastQC app
3. Fill in the job parameters (number of cores, walltime, and allocation/account code)
4. Click **Launch** — a VNC desktop session will start with the FastQC GUI open and ready to use

## Troubleshooting
**FastQC does not launch:**
- Ensure the `fastqc_0.12.1.sif` container exists at the path configured in `form.yml.erb`
- Check that the correct allocation account is entered in the job submission form

**`squashfuse` / `gocryptfs` warnings on startup:**
- These are harmless informational messages and can be safely ignored

## Contributing
Contributions are welcome. Please fork the repository and submit a pull request with your changes.

## License
- Documentation and container definition are licensed under [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/)
- FastQC is developed and maintained by the [Babraham Institute](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/) and released under the [GPL v3 or later](https://www.gnu.org/copyleft/gpl.html)