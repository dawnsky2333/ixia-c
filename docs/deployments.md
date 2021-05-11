# Deployment Guide

- [Table of Contents](readme.md)
  - Deployment Guide
    * [Overview](#overview)
    * [Bootstrap](#bootstrap)
    * [Deployment Parameters](#deployment-parameters)
    * [Test Suite](#test-suite)
    * [One-arm Scenario](#one-arm-scenario)
    * [Two-arm Scenario](#two-arm-scenario)
    * [Three-arm Mesh Scenario](#three-arm-mesh-scenario)

### Overview

Ixia-C is distributed / deployed as a multi-container application consisting of following services:

* `controller` - Serves API request from clients and manages workflow across one or more traffic engines.
* `traffic-engine` - Generates, captures and processes traffic from one or more network interfaces (on linux-based OS).
* `app-usage-reporter` - (Optional) Collects usage report from controller and uploads it to Keysight Cloud, minimizing impact on host resources.

All these services are available as docker image on [docker-hub](https://hub.docker.com/). To pull in specific versions of these images, please check [Ixia-C Releases](releases.md).  

### Bootstrap

Ixia-C services can either all be deployed on same host or each on separate hosts (as long as they're mutually reachable over network). There's no boot-time dependency between them, which allows for **horizantal scalibility** without interrupting existing services.

Following outlines how connectivity is established between the services:

* `controller` <-> `traffic-engine` - when client pushes a traffic configuration to controller containing address of traffic engine as **port location**.
* `controller` <-> `app-usage-reporter` - controller periodically attempts connection with app-usage-reporter until successful; the location of app-usage-reporter can be overridden by passing an argument to controller upon boot-up.

> The location of traffic-engine or app-usage-reporter must be reachable from controller, although they don't need to be reachable from client scripts.

Deploying these services manually (along with required parameters) may not be desired in some scenarios and hence, for convenience [deployments](../deployments) directory consists of `docker-compose` files pertaining to some commonly used scenarios, requiring only a single command for spawning all required services.

All the scenarios mentioned in upcoming sections describes both manual and automated (requiring docker-compose) steps.

### Deployment Parameters

* Deploying Controller

  | Controller Parameters       | Optional  | Default                 | Description                                                     |
  |-----------------------------|-----------|-------------------------|-----------------------------------------------------------------|
  | --debug                     |   Yes     | false                   | Enables high volume logs with debug info for better diagnostics.|
  | --disable-app-usage-reporter|   Yes     | false                   | Disables sending usage data to app-usage-reporter.              |
  | --http-port                 |   Yes     | 443                     | TCP port for HTTP server.                                       |
  | --log-out                   |   Yes     | false                   | Enables streaming log output to stdout.                         |
  | --aur-host                  |   Yes     | https://localhost:5600  | Overrides location of app-usage-reporter.                       |


  | Docker Parameters           | Optional  | Default                 | Description                                                     |
  |-----------------------------|-----------|-------------------------|-----------------------------------------------------------------|
  | --net=host                  |   Yes     | bridge                  | Use host's network stack, allowing traffic-engine containers to be addressed using *localhost* instead of *Container IP*, when deployed on same host.                                                    |
  | -d                          |   Yes     | NA                      | Start container in background.                                  |


  To check logs,
  
  ```sh
  docker exec <container-id> cat logs/controller.log
  ```

  Example:

  ```bash
  docker run --net=host -d ixiacom/ixia-c-controller --debug --http-port 5050
  ```

* Deploying Traffic Engine

> TBD: Fix descriptions and add Diagnostics

  | Environment Variable | Mandatory | Default | Example | Comments |
  |----------------------|-----------|---------|----------|--------|
  | ARG_IFACE_LIST       | Yes | None | "virtual@af_packet@veth1" | FIXME |
  | OPT_LISTEN_PORT      | No | 5555 | 5556 | Port on which the `traffic-engine` will listen to for communication with the `controller` |
  | OPT_NO_HUGEPAGES     | No | "Yes" | FIXME | FIXME |

  Example:

  ```bash
  docker run --net=host --privileged -d         \
    -e OPT_LISTEN_PORT="5555"                   \
    -e ARG_IFACE_LIST="virtual@af_packet,eth1"  \
    -e OPT_NO_HUGEPAGES="Yes"                   \
    --cpuset-cpus="0,1,2"                       \
    ixiacom/ixia-c-traffic-engine
  ```

  > `--net=host` is required here when binding to network interfaces that are visible to host OS and not the traffic engine container

### Test Suite

### One-arm Scenario

> TODO: diagram

* Automated

  ```bash
  docker-compose -f deployments/raw-one-arm.yml up -d
  # optionally stop and remove services deployed
  docker-compose -f deployments/raw-one-arm.yml down
  ```

* Manual

  ```bash
  # start controller and app usage reporter
  docker run --net=host -d ixiacom/ixia-c-controller
  docker run --net=host -d ixiacom/ixia-c-app-usage-reporter

  # start traffic engine on network interface eth1, TCP port 5555 and cpu cores 0, 1, 2
  docker run --net=host --privileged -d         \
    -e OPT_LISTEN_PORT="5555"                   \
    -e ARG_IFACE_LIST="virtual@af_packet,eth1"  \
    -e OPT_NO_HUGEPAGES="Yes"                   \
    --cpuset-cpus="0,1,2"                       \
    ixiacom/ixia-c-traffic-engine
  ```

### Two-arm Scenario

> TODO: diagram

* Automated

  ```bash
  docker-compose -f deployments/raw-two-arm.yml up -d
  # optionally stop and remove services deployed
  docker-compose -f deployments/raw-two-arm.yml down
  ```

* Manual

  ```bash
  # start controller and app usage reporter
  docker run --net=host -d ixiacom/ixia-c-controller
  docker run --net=host -d ixiacom/ixia-c-app-usage-reporter

  # start traffic engine on network interface eth1, TCP port 5555 and cpu cores 0, 1, 2
  docker run --net=host --privileged -d         \
    -e OPT_LISTEN_PORT="5555"                   \
    -e ARG_IFACE_LIST="virtual@af_packet,eth1"  \
    -e OPT_NO_HUGEPAGES="Yes"                   \
    --cpuset-cpus="0,1,2"                       \
    ixiacom/ixia-c-traffic-engine

  # start traffic engine on network interface eth2, TCP port 5556 and cpu cores 0, 3, 4
  docker run --net=host --privileged -d         \
    -e OPT_LISTEN_PORT="5556"                   \
    -e ARG_IFACE_LIST="virtual@af_packet,eth2"  \
    -e OPT_NO_HUGEPAGES="Yes"                   \
    --cpuset-cpus="0,3,4"                       \
    ixiacom/ixia-c-traffic-engine
  ```

### Three-arm Mesh Scenario

This scenario binds traffic engine to management network interface belonging to the container which in turn is part of docker0 network.

> TODO: diagram

* Automated

  ```bash
  docker-compose -f deployments/raw-three-arm-mesh.yml up -d
  # optionally stop and remove services deployed
  docker-compose -f deployments/raw-three-arm-mesh.yml down
  ```

* Manual

  ```bash
  # start controller and app usage reporter
  docker run --net=host -d ixiacom/ixia-c-controller
  docker run --net=host -d ixiacom/ixia-c-app-usage-reporter

  # start traffic engine on network interface eth0, TCP port 5555 and cpu cores 0, 1, 2
  docker run --privileged -d                    \
    -e OPT_LISTEN_PORT="5555"                   \
    -e ARG_IFACE_LIST="virtual@af_packet,eth0"  \
    -e OPT_NO_HUGEPAGES="Yes"                   \
    -p 5555:5555                                \
    --cpuset-cpus="0,1,2"                       \
    ixiacom/ixia-c-traffic-engine

  # start traffic engine on network interface eth0, TCP port 5556 and cpu cores 0, 3, 4
  docker run --privileged -d                    \
    -e OPT_LISTEN_PORT="5555"                   \
    -e ARG_IFACE_LIST="virtual@af_packet,eth0"  \
    -e OPT_NO_HUGEPAGES="Yes"                   \
    -p 5556:5555                                \
    --cpuset-cpus="0,3,4"                       \
    ixiacom/ixia-c-traffic-engine

  # start traffic engine on network interface eth0, TCP port 5557 and cpu cores 0, 5, 6
  docker run --privileged -d                    \
    -e OPT_LISTEN_PORT="5555"                   \
    -e ARG_IFACE_LIST="virtual@af_packet,eth0"  \
    -e OPT_NO_HUGEPAGES="Yes"                   \
    -p 5557:5555                                \
    --cpuset-cpus="0,5,6"                       \
    ixiacom/ixia-c-traffic-engine
  ```

### TODO: Multi-port per TE container

## Tests

### Clone Tests

  ```sh
    # Git clone sanppi-tests repo from github
    # This repository consists of end-to-end test scripts written in [snappi](https://github.com/open-traffic-generator/snappi).
    git clone https://github.com/open-traffic-generator/snappi-tests.git
    cd snappi-tests
  ```

### Setup Tests

  Please make sure that the client setup meets [Python Prerequisites](#test-prerequisites).

* **Install `snappi`.**

    ```sh
      python -m pip install --upgrade "snappi"
    ```

* **Install test dependencies**

    ```sh
      python -m pip install --upgrade -r requirements.txt
    ```

* **Ensure a `sample test` script executes successfully. Please see [test details](#test-details) for more info.**

    ```sh
      # provide intended API Server and port addresses
      vi tests/settings.json
      # run a test
      python -m pytest tests/raw/test_tcp_unidir_flows.py
    ```

## Test Details

The test scripts are based on `snappi client SDK` (auto-generated from [Open Traffic Generator Data Model](https://github.com/open-traffic-generator/models)) and have been written using `pytest`.  

Open Traffic Generator Data Model can be accessed from any browser by hitting this url (https://<controller-host-ip>/docs/) and start scripting.

The test directory structure is as follows:

* `snappi-tests/tests/settings.json` - global test configuration, includes `controller` host, `traffic-engine` host and `speed` settings.
* `snappi-tests/configs/` - contains pre-defined traffic configurations in JSON, which can be loaded by test scripts.
* `snappi-tests/tests` - contains end-to-end test scripts covering most common use-cases.
* `snappi-tests/tests/utils/` - contains most commonly needed helpers, used throughout test scripts.
* `snappi-tests/tests/env/bin/python` - python executable (inside virtual environment) to be used for test execution.

Most test scripts follow the format of following sample scripts:

* `snappi-tests/tests/raw/test_tcp_unidir_flows.py` - for unidirectional flow use case.
* `snappi-tests/tests/raw/test_tcp_bidir_flows.py` - for using pre-defined JSON traffic config & bidirectional flow use case.
* `snappi-tests/tests/raw/test_basic_flow_stats.py` - for basic flow statistics validation use case.
* `<test to validate capture>` - for validating capture. TODO
* `<test to cover multiple protocol fields for different variation like fixed, list, counter>` - some example from gtpv2 [ethernet - ipv4 - udp - gtpv2 - ipv6] TODO
* `<test script for one arm testing>` - for one arm scenario TODO

To execute batch tests marked as `sanity`:

```sh
tests/env/bin/python -m pytest tests/py -m "sanity"
```

**NOTE on tests/settings.json**:

* When `controller` and `traffic-engine`s are located on separate systems (remote)

  ```json
  {
   "controller": "https://<controller-ip>",
    "ports": [
        "<traffic-engine-ip>:5555",
        "<traffic-engine-ip>:5556"
    ]
  }
  ```

* When `controller` and `traffic-engine`s are located on same system (local - raw sockets)

  ```json
  {
   "controller": "https://localhost",
    "ports": [
        "localhost:5555",
        "localhost:5556"
    ]
  }
  ```
