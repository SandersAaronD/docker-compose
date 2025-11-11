# Scrutiny - (S.M.A.R.T. visualizer)

docker compose for running **[Scrutiny](https://github.com/AnalogJ/scrutiny)**

## overview
scrutiny collects and displays health information from all drives connected to the host.  
I’m using the **omnibus image** (includes InfluxDB) so it’s just one container.

## reason
**[smartmontools](www.smartmontools.org)** is spectacular software
however, taking the time to wade through 
```bash
sudo smartctl -a /dev/<whatever-drive>
```
and actually coming up with a good idea how broad drive health requires a great deal of effort


## usage
Clone the repo and run:
```bash
docker compose up -d
```

Then open http://localhost:8088 (or whatever host port you mapped)

## configuration

- Container runs as root to access /dev/nvme* and /dev/sd*.
- Added minimal capabilities (SYS_ADMIN, SYS_RAWIO) so it can read SMART data without being fully privileged.
- Data is stored locally in:
-- ./config → Scrutiny config
-- ./influxdb → time-series data

## notes
Change the host port in the compose file if 8088 is in use.

Drives must be visible on the host (smartctl should work locally).

Tested on Debian 13 with Docker Compose v2.

This is for personal use / homelab monitoring — no external network exposure.