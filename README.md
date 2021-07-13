# check_hassio - Monitoring Home Assistant sensors with Nagios/Icinga

This monitoring plugin queries Home Assistant sensors and compares their states with expected values.
It can check for strings, timestamps and numeric values.
The results can be absolute (strings) or relative to expected references (timestamps and numeric values).

## Installation

The installation requires Python 3.

I usually install additional plugins under `/usr/local/lib/nagios/plugins`.
So, it works out of the box, if you just do:

```
mkdir -p /usr/local/lib/nagios/plugins
cd /usr/local/lib/nagios/plugins
wget https://raw.githubusercontent.com/check-plugins/check_hassio/master/check_hassio
chmod +x check_hassio
```

The `requests` module is required.
It can be installed on Debian by running:

```
apt-get install python3-requests
```

## Parameters

```
usage: check_hassio [-h] [-H <hostname>] [-P <port>] [-t <token>]
                    [-s <sensor>] [-S] [-I] [-F] [-T] [-e EXPECTED] [-n NOTEXPECTED]
                    [-w WARNING] [-c CRITICAL] [-W BWARNING] [-C BCRITICAL]
                    [-r RANGE]

optional arguments:
  -h, --help            show this help message and exit
  -H <hostname>, --host <hostname>
                        host to connect to (defaults to localhost)
  -P <port>, --port <port>
                        network port to connect to (defaults to 8123)
  -t <token>, --token <token>
                        API token (defaults to None)
  -s <sensor>, --sensor <sensor>
                        Sensor id (default to None)
  -S, --ssl             use HTTPS
  -I, --int             Use state as int
  -F, --float           Use state as float
  -T, --timestamp       Use state as timestamp
  -e EXPECTED, --expected EXPECTED
                        Check expected state (default to None)
  -n NOTEXPECTED, --notexpected NOTEXPECTED
                        Check expected state (default to None)
  -w WARNING, --warning WARNING
                        Check if above threshold (warning, default to None)
  -c CRITICAL, --critical CRITICAL
                        Check if above threshold (critical, default to None)
  -W BWARNING, --warning-below BWARNING
                        Check if below threshold (warning, default to None)
  -C BCRITICAL, --critical-below BCRITICAL
                        Check if below threshold (critical, default to None)
  -r RANGE, --range RANGE
                        Check if in range [from:to] (default to None)
```

## Example

Query hassio.my.home running on port 443 and using SSL for sensor monitor_last_action.
The result is interpreted as timestamp and is critical if it is older than 30 seconds.

```
./check_hassio -H hassio.my.home -P 443 -S -t "token" -s "sensor.monitor_last_action" -T -e 30
```

Query hassio.my.home running on port 80 for sensor hallo.
The result is interpreted as string and is critical if it not matches "Welt!".

```
./check_hassio -H hassio.my.home -t "token" -s "sensor.hallo" -e "Welt!"
```

The result is interpreted as string and is critical if it matches "unavailable".

```
./check_hassio -H hassio.my.home -t "token" -s "sensor.hallo" -n "unavailable"
```

Test for sensor value if it is above certain thresholds

```
./check_hassio -H hassio.my.home -t "token" -s "sensor.temperature" -F -w 20 -c 30
```

Test for sensor value if it is below certain thresholds

```
./check_hassio -H hassio.my.home -t "token" -s "sensor.temperature" -F -W 10 -C 0
```

Test for sensor value to be in a certain range

```
./check_hassio -H hassio.my.home -t "token" -s "sensor.temperature" -F -r "5:15"
```

## Support

I have not the time (yet) to provide professional support for this project.
But feel free to submit issues and PRs, I'll check for it and honor your contributions.

## License

The whole project is licensed under GPL license. Stay fair.
