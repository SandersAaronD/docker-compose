# Scrutiny - S.M.A.R.T. Visualizer

Docker Compose configuration for running **[Scrutiny](https://github.com/AnalogJ/scrutiny)**.

![Application Screenshot](it-works.png)

## Overview

Scrutiny collects and displays health information from all drives connected to the host system. This configuration utilizes the **omnibus image** (which includes InfluxDB), resulting in a single-container deployment.

## Rationale

**[smartmontools](https.smartmontools.org)** is exceptional software; however, interpreting the output of:
```bash
sudo smartctl -a /dev/<device>
```

and accurately assessing drive health status requires considerable time and expertise.

Previously, **[Hard Drive Sentinel](https://www.hdsentinel.com/hard_disk_sentinel_linux.php)** served this purpose, but the project has not been actively maintained in recent years.

Scrutiny provides comprehensive analysis and reportedly utilizes Backblaze statistics to calculate the statistical likelihood of drive failure based on specific attribute values.

The primary challenge is that `smartctl` requires root privileges, making containerization without `privileged: true` complex. While this configuration uses `cap_add: - SYS_ADMIN` and `- SYS_RAWIO`, which is suboptimal from a security perspective, it is significantly preferable to `privileged: true`. Furthermore, this approach is substantially better than remaining unaware of impending drive failures due to time constraints preventing regular S.M.A.R.T. data analysis. Note that S.M.A.R.T. monitoring does not guarantee prevention of drive failure or data loss.

## Usage

Clone the repository and execute:
```bash
docker compose up -d
```

Access the web interface at http://localhost:8088 (or your configured host port).

## Configuration

- The container runs as root to access `/dev/nvme*` and `/dev/sd*` devices.
- Minimal capabilities (SYS_ADMIN, SYS_RAWIO) have been added to enable data access without full privileged mode.
- Data is stored locally in:
  - `./config` → Scrutiny configuration
  - `./influxdb` → Time-series data

## Notes

Modify the host port in the compose file if port 8088 is already in use.

Drives must be visible on the host system (smartctl should function locally).

Tested on Debian 13 with Docker Compose v2.

This configuration is intended for personal use and homelab monitoring—not for external network exposure.

---

&nbsp;

**466f724a616e6574**
