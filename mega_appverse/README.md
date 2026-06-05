# MEGA Open OnDemand App
An Open OnDemand app for running [MEGA 12.1.2](https://www.megasoftware.net/) as a desktop GUI application via a VNC session.

## Prerequisites

- **Apptainer** 1.3.1+
- **Open OnDemand** 2.x+
- A Slurm-based HPC cluster configured with Open OnDemand

## Container Contents

| Component | Version |
|-----------|---------|
| Base OS | Ubuntu 22.04 |
| MEGA | 12.1.2 |
| wmctrl / xdotool | system |
| GTK2 toolkit | system |
| Chromium Embedded Framework (CEF) | bundled with MEGA |

## Building the Container
Build the container on a system with internet access:
apptainer build mega12_gui.sif mega12_gui.def

## Setup
1. Move the container to a shared directory accessible from compute nodes:
mv mega12_gui.sif /path/to/apptainer_images/mega/mega12_gui.sif

2. Update `cluster` in `form.yml.erb` to match your cluster id (the filename without `.yml` under `/etc/ood/config/clusters.d/`):
cluster: "mycluster"

3. Update the `.sif` path in `form.yml.erb` to point to where you placed the container:
options:
  - ["MEGA 12.1.2", "/path/to/apptainer_images/mega/mega12_gui.sif"]

4. Update `module load apptainer` in `template/script.sh.erb` to match your cluster's module name if different.

## Running MEGA via Open OnDemand
1. Log in to your Open OnDemand instance
2. Navigate to **Interactive Apps** and select the MEGA app
3. Fill in the job parameters (account, partition, cores, walltime)
4. Click **Launch**: a VNC desktop session will start with the MEGA GUI open and maximized

## Troubleshooting
**Out of memory errors:**
- Increase the number of cores or memory per core in the job form

**MEGA does not launch:**
- Verify the `.sif` path in `form.yml.erb` is correct and the file exists on the compute node

**`squashfuse` / `gocryptfs` warnings on startup:**
- These are harmless informational messages and can be safely ignored

## Contributing
Contributions are welcome. Please open an issue or submit a pull request on [GitHub](https://github.com/ubc-arc/OOD_Appverse).

## License
- Documentation and container definition are licensed under [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/)
- MEGA is developed and maintained by the [Kumar Lab at Temple University](https://www.megasoftware.net/) and is free for research and educational use. It cannot be redistributed. Please refer to the [MEGA End User Agreement](https://www.megasoftware.net/show_eu_agreement) for terms of use