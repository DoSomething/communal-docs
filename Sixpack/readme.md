# Sixpack Setup

> [Sixpack](http://sixpack.seatgeek.com/) is a framework to enable A/B testing across multiple programming languages. It does this by exposing a simple API for client libraries. Client libraries can be written in virtually any language.

_more info on DS and Sixpack usage_

## Requirements
- Redis >= 2.6
- Python >= 2.7

## Installation

#### Step 1: Install Python Tools & Sixpack
On your local [Homestead]() virtual environment, SSH into the box and install the python development tools:

```shell
$ sudo apt-get install python-dev
```

Then install **sixpack**:

```shell
$ sudo pip install sixpack
```

#### Step 2: Create Sixpack Configuration
Create and save the following configuration file in `/etc/sixpack/config.yml`:

```yml
redis_port: 6379                            # Redis port
redis_host: localhost                       # Redis host
redis_prefix: sixpack                       # all Redis keys will be prefixed with this
redis_db: 15                                # DB number in redis

metrics: false                              # send metrics to StatsD (response times, # of calls, etc)?
statsd_url: 'udp://localhost:8125/sixpack'  # StatsD url to connect to (used only when metrics: true)

# The regex to match for robots
robot_regex: $^|trivial|facebook|MetaURI|butterfly|google|amazon|goldfire|sleuth|xenu|msnbot|SiteUptime|Slurp|WordPress|ZIBB|ZyBorg|pingdom|bot|yahoo|slurp|java|fetch|spider|url|crawl|oneriot|abby|commentreader|twiceler
ignored_ip_addresses: []                    # List of IP

asset_path: gen                             # Path for compressed assets to live. This path is RELATIVE to sixpack/static
secret_key: '<your secret key here>'        # Random key (any string is valid, required for sixpack-web to run)
```

**Make sure to update the `<your secret key here>` with a custom random string key.**

#### Step 3: Start Sixpack Server
You should now be able to start the Sixpack server:

```shell
$ SIXPACK_CONFIG=/etc/sixpack/config.yml sixpack &
```

And also start the Sixpack web experience dashboard:

```shell
$ sudo SIXPACK_CONFIG=/etc/sixpack/config.yml sixpack-web &
```

_Note: the `&` at the end of each command, just tells the shell to run the command in the background._

