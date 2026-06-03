# Bandage Apptainer Container

An Apptainer container for running [Bandage 0.9.0](https://github.com/rrwick/Bandage) as a desktop GUI application via Open OnDemand.

Part of the [UBC ARC OOD Appverse](https://github.com/ubc-arc/OOD_Appverse) collection.

---

## Prerequisites

- **Apptainer** 1.3.1+
- **Open OnDemand** for launching the Bandage GUI through the web portal

---

## Container Contents

| Component | Version |
|-----------|---------|
| Base OS | Ubuntu 22.04 |
| Bandage | 0.9.0 |
| wmctrl / xdotool | system |
| BLAST+ | system |

---

## Building the Container

The container must be built on a node with internet access in order to pull the base Ubuntu image and clone the Bandage source from GitHub.

Copy the definition file (`bandage.def`) to your working directory, then build:


apptainer build bandage.sif /full/path/to/bandage.def


Once built, move the container to your shared software directory:

mv bandage.sif /path/to/apptainer_images/bandage/


Update the `.sif` path in `form.yml.erb` to match your deployment location.

---

## Running Bandage via Open OnDemand

Bandage is launched through the Open OnDemand web portal as a graphical desktop application. No command line interaction is required.

1. Log in to your Open OnDemand instance
2. Navigate to **Interactive Apps** and select the Bandage app
3. Fill in the job parameters (cluster, number of cores, memory, time, account)
4. Click **Launch**: a VNC desktop session will start with the Bandage GUI open and maximized

---

## Troubleshooting

**Out of memory errors:**
- Request more memory when submitting the job through the Open OnDemand form
- Increase the number of cores or memory per core for large graphs

---

## Contributing

Contributions are welcome. Please open an issue or submit a pull request on [GitHub](https://github.com/ubc-arc/OOD_Appverse).

---

## License

- Documentation and container definition are licensed under [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/)
- Bandage is developed by [Ryan Wick](https://github.com/rrwick) and released under the [GPL v3](https://www.gnu.org/licenses/gpl-3.0.html)