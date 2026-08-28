# Bandage Open OnDemand App

An Open OnDemand app for running [Bandage 0.9.0](https://github.com/rrwick/Bandage) as a desktop GUI application via a VNC session.

Part of the [UBC ARC OOD Appverse](https://github.com/ubc-arc/OOD_Appverse) collection.



## Prerequisites

- **Apptainer** 1.3.1+
- **Open OnDemand** 2.x+
- A Slurm-based HPC cluster configured with Open OnDemand



## Container Contents

| Component | Version |
|-----------|---------|
| Base OS | Ubuntu 22.04 |
| Bandage | 0.9.0 |
| xdotool | system |
| BLAST+ | system |


## Building the Container

Build the container on a system with internet access:
apptainer build bandage.sif bandage.def

## Setup

1. Move the container to a shared directory accessible from compute nodes:
mv bandage.sif /path/to/apptainer_images/bandage/bandage.sif


2. Update `cluster` in `form.yml.erb` to match your cluster id (the filename without `.yml` under `/etc/ood/config/clusters.d/`):
cluster: "mycluster"


3. Update the `.sif` path in `form.yml.erb` to point to where you placed the container:
options:
  - ["Bandage 0.9.0", "/path/to/apptainer_images/bandage/bandage.sif"]


4. Update `module load apptainer` in `script.sh` to match your cluster's module name if different.



## Running Bandage via Open OnDemand

1. Log in to your Open OnDemand instance
2. Navigate to **Interactive Apps** and select the Bandage app
3. Fill in the job parameters (account, queue, cores, walltime)
4. Click **Launch** : a VNC desktop session will start with the Bandage GUI open and maximized



## Troubleshooting

**Out of memory errors:**
- Increase the number of cores in the job form (memory scales with cores)

**Bandage does not launch:**
- Verify the `.sif` path in `form.yml.erb` is correct and the file exists on the compute node



## Contributing

Contributions are welcome. Please open an issue or submit a pull request on [GitHub](https://github.com/ubc-arc/OOD_Appverse).


## License

- Documentation and container definition are licensed under [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/)
- Bandage is developed by [Ryan Wick](https://github.com/rrwick) and released under the [GPL v3](https://www.gnu.org/licenses/gpl-3.0.html)