# IGV Open OnDemand App
An Open OnDemand app for running [IGV (Integrative Genomics Viewer)](https://igv.org/) 2.17.0 as a desktop GUI application via a VNC session.
Part of the UBC ARC OOD Appverse collection.

## Prerequisites

- Apptainer 1.3.1+
- Open OnDemand 2.x+
- A Slurm-based HPC cluster configured with Open OnDemand

## Container Contents

| Component | Version |
|-----------|---------|
| Base OS | Ubuntu 22.04 |
| IGV | 2.17.0 |
| Java (bundled) | 17 (included with IGV) |


## Building the Container
Build the container on a system with internet access (e.g. a login node):
apptainer build --fakeroot igv.sif igv.def


## Setup
1. Move the container to a shared directory accessible from compute nodes:
mv igv.sif /path/to/apptainer_images/igv/igv.sif

2. Update `cluster` in `form.yml.erb` to match your cluster id (the filename without `.yml` under `/etc/ood/config/clusters.d/`):
cluster: "mycluster"

3. Update the `.sif` path in `form.yml.erb` to point to where you placed the container:
options:
  ["IGV 2.17.0", "/path/to/apptainer_images/igv/igv.sif"]

4. Update `module load apptainer` in `script.sh` to match your cluster's module name if different.

## Running IGV via Open OnDemand
1. Log in to your Open OnDemand instance
2. Navigate to **Interactive Apps** and select the IGV app
3. Fill in the job parameters (account, queue, cores, walltime)
4. Click **Launch**: a VNC desktop session will start with the IGV GUI open and ready to use


## Troubleshooting
**IGV does not launch:**
- Verify the `.sif` path in `form.yml.erb` is correct and the file exists on the compute node

**Out of memory errors:**
- Increase the number of cores in the job form (memory scales with cores)
- The default memory allocation is 4GB; increase to 8GB or more for large datasets

**squashfuse / gocryptfs warnings on startup:**
- These are harmless informational messages and can be safely ignored

## Contributing
Contributions are welcome. Please open an issue or submit a pull request on [GitHub](https://github.com/ubc-arc/OOD_Appverse).

## License
- Documentation and container definition are licensed under [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/)
- IGV is developed and maintained by the [Broad Institute](https://igv.org/) and is licensed separately. Please refer to the [IGV license](https://github.com/igvteam/igv/blob/master/license.txt) for terms of use