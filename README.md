# fluxrestic

Parse [Restic](https://restic.net/) status output and feed summary to a influxdb v2 db. 

Tested with influxdb 2.3 and restic 0.1.3

**requirements**

requires python 3 and influxdb-python library

restic >= 0.13.1

influxdb >= 2.3 

Install, upgrade and uninstall influxdb-python with these commands:

```
$ pip install influxdb-client
```

**usage**

```
usage: fluxrestic [-h] [-t TOKEN] [-o organisation] [-b BUCKET] [-s HOST] [-m MEASUREMENT] [-r REPO] [-c REPOSERVER]

optional arguments:
  -h, --help            show this help message and exit
  -t TOKEN, --token TOKEN
                        influx auth token
  -o organisation, --org organisation
                        influx organisation
  -b BUCKET, --bucket BUCKET
                        target bucket
  -s HOST, --host HOST  hostname
  -m MEASUREMENT, --measurement MEASUREMENT
                        influx measurement, e.g. restic or restic_%type
  -r REPO, --repository REPO
                        restic repository (optional)
  -c REPOSERVER, --resticserver REPOSERVER
                        restic server (optional)

'influx host' defaults to http://localhost:8086 if omitted

```

full example

```
#!/bin/bash
export RESTIC_PASSWORD="MY-SECRET-RESTIC-PASSWORD"
export INFLUX_TOKEN="MY-SECRET-INFLUX-TOKEN"

restic --json -r sftp:me@myresticserver.com:/users/me/testrepo backup /mnt/gmbh/gfb | ./fluxrestic -o mycompany -b mybucket -s https://www.myinfluxserver.com:8086 -c myresticserver.com -r testrepo -c myresticserver.com -m restic_%type

```
For hiding the influx token (--token), we make use of the fallback enviroment variable INFLUX_TOKEN here.

There are some more fallback enviroment variables:

INFLUX_ORG : influx organisation (--org)
INFLUX_BUCKET : influx bucket (--bucket)
RESTIC_REPOSITORY : restic repo (--repository)


**permissions**

make this script executable

e.g. using command line

```
chmod +x fluxrestic"
```

feel free to copy it into directory in your PATH enviroment (e.g. /usr/local/bin, /usr/bin)


**similar tools**

[restic2influx](https://github.com/hn/restic2influx) by [Hajo Noerenberg](https://github.com/hn) (for use with influxdb v1.x)
