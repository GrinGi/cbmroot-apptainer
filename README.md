# CBMROOT Apptainer Container (Debian 11)

Reproducible Apptainer container for building **FairSoft**, **FairRoot**, and **CBMROOT** on **Debian 11**.  
The installation process is fully configuration-driven via `cbm-container.conf`.

---

## Overview

This repository provides:

- `cbmroot_debian11.def` — Apptainer definition file  
- `cbm-container.conf` — Configuration file controlling the installation  
- Automatic version selection and patch injection support  
- Optional use of external FairSoft / FairRoot installations  

The container can be built either as a read-only `.sif` image or as a writable sandbox.

---

## Requirements

- Apptainer ≥ 1.x  
- Internet access during build  
- ~80 GB free disk space for installation
- ~12 GB container size

---

## Build the Container

```bash
apptainer build cbmroot_debian11.sif cbmroot_debian11.def
```

---

## Build as Writable Sandbox

```bash
apptainer build --sandbox cbmroot_sandbox cbmroot_debian11.def
```

Enter sandbox:

```bash
apptainer shell --writable cbmroot_sandbox
```

Re-run installation manually inside the container:

```bash
/opt/cbm/run_autoinstall.sh
```

Or execute from outside:

```bash
apptainer exec --writable cbmroot_sandbox /opt/cbm/run_autoinstall.sh
```

---

## Configuration

All build behaviour is controlled via:

```
cbm-container.conf
```

You can configure:

- Number of build jobs  
- Stack selection (PRO / DEV / OLD)  
- Installation of FairSoft / FairRoot / CBMROOT  
- Specific software versions (ROOT / FairSoft / FairRoot)  
- Specific CBMROOT branch/tag/commit checkout  
- External FairSoft/FairRoot paths  
- Extra flags or full override of `autoinstall_framework.sh`  

---

## Selecting Specific Software Versions

Enable custom version injection in `cbm-container.conf`:

```ini
SPEC_FSOFR_FROOT=1
ROOTVER=6
FSOFTVER=apr21p2
FROOTVER=v18.2.1
```

To checkout a specific CBMROOT version:

```ini
SPEC_CBMROOT_VER=1
CBMROOT_VER=master
```

---

## Installation Logs

Logs are written to:

```
/opt/cbm/install_YYYYMMDD_HHMMSS.log
```

---

## Default Installation Layout

Installation base:

```
/opt/cbm
```

CBMROOT sources:

```
/opt/cbm/src/cbmroot
```

---

## Notes

- If `AUTO_INSTALL=yes`, installation is executed during image build.  
- If `AUTO_INSTALL=no`, installation can be run later inside the container.  
- When `SPEC_CBMROOT_VER=1` is enabled, the runner automatically fetches full history and tags if the repository was cloned shallow.  

---

## Author

Grigory Kozlov  
FIAS, Frankfurt am Main
