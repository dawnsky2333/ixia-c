# Ixia-c Release Notes and Version Compatibility


## Release v0.0.1-2330 (Latest)
> 29th September, 2021

#### About

This build contains performance improvement in SetConfig and more correct fps vaules in scenarios involving multiple start or stop traffic.

#### Build Details

| Component                     | Version       |
|-------------------------------|---------------|
| Open Traffic Generator API    | [0.6.5](https://redocly.github.io/redoc/?url=https://github.com/open-traffic-generator/models/releases/download/v0.6.5/openapi.yaml)         |
| snappi                        | [0.6.5](https://pypi.org/project/snappi/0.6.5)        |
| ixia-c-controller             | [0.0.1-2330](https://hub.docker.com/r/ixiacom/ixia-c-controller/tags)    |
| ixia-c-traffic-engine         | [1.4.0.13](https://hub.docker.com/r/ixiacom/ixia-c-traffic-engine/tags)       |
| ixia-c-app-usage-reporter     | [0.0.1-37](https://hub.docker.com/r/ixiacom/ixia-c-app-usage-reporter/tags)      |
| ixia-c-protocol-engine        | 1.00.0.83      | 
| otg-gnmi-server               | [0.6.6](https://hub.docker.com/r/otgservices/otg-gnmi-server/tags)         |
| otg-grpc-server               | [0.6.6](https://hub.docker.com/r/otgservices/otg-grpc-server/tags)         |


#### New Feature(s)

* SetConfig Performance is improved through concurrent operations.
* FPS shows more correct values in scenarios involving multiple start or stop traffic in same test.

#### Known Issues

* The metric `loss` in flow metrics is currently not supported.

## Release v0.0.1-2289
> 29th September, 2021

#### About

This build contains support for performance optimisation through concurrent port operations.

#### Build Details

| Component                     | Version       |
|-------------------------------|---------------|
| Open Traffic Generator API    | [0.6.1](https://redocly.github.io/redoc/?url=https://github.com/open-traffic-generator/models/releases/download/v0.6.1/openapi.yaml)         |
| snappi                        | [0.6.1](https://pypi.org/project/snappi/0.6.1)        |
| ixia-c-controller             | [0.0.1-2289](https://hub.docker.com/r/ixiacom/ixia-c-controller/tags)    |
| ixia-c-traffic-engine         | [1.4.0.13](https://hub.docker.com/r/ixiacom/ixia-c-traffic-engine/tags)       |
| ixia-c-app-usage-reporter     | [0.0.1-37](https://hub.docker.com/r/ixiacom/ixia-c-app-usage-reporter/tags)      |
| ixia-c-protocol-engine        | 1.00.0.70      | 
| otg-gnmi-server               | [0.6.1](https://hub.docker.com/r/otgservices/otg-gnmi-server/tags)         |
| otg-grpc-server               | [0.6.1](https://hub.docker.com/r/otgservices/otg-grpc-server/tags)         |


#### New Feature(s)

* Performance is optimised through concurrent port operations.

#### Known Issues

* The metric `loss` in flow metrics is currently not supported.


## Release v0.0.1-2185 
> 8th September, 2021

#### About

This build contains support for updating flow rate without disrupting transmit state.

#### Build Details

| Component                     | Version       |
|-------------------------------|---------------|
| Open Traffic Generator API    | [0.5.4](https://redocly.github.io/redoc/?url=https://github.com/open-traffic-generator/models/releases/download/v0.5.4/openapi.yaml)         |
| snappi                        | [0.5.3](https://pypi.org/project/snappi/0.5.3)        |
| ixia-c-controller             | [0.0.1-2185](https://hub.docker.com/r/ixiacom/ixia-c-controller/tags)    |
| ixia-c-traffic-engine         | [1.4.0.11](https://hub.docker.com/r/ixiacom/ixia-c-traffic-engine/tags)       |
| ixia-c-app-usage-reporter     | [0.0.1-37](https://hub.docker.com/r/ixiacom/ixia-c-app-usage-reporter/tags)      |
| ixia-c-protocol-engine        | 1.00.0.56     | 
| otg-gnmi-server               | [0.5.2](https://hub.docker.com/r/otgservices/otg-gnmi-server/tags)         |
| otg-grpc-server               | [0.5.3](https://hub.docker.com/r/otgservices/otg-grpc-server/tags)         |


#### New Feature(s)

* Updating flow rate without disrupting transmit state is now supported. Rate of multiple flows can be updated simultaneously through `update_flows` api without stopping the traffic. 

#### Known Issues

* The metric `loss` in flow metrics is currently not supported.


## Release v0.0.1-2120 
> 27th August, 2021

#### About

This build contains support for capture filter, setting GRE checksum flag, redirecting Ixia-c controller log to stdout and some bug fixes.

#### Build Details

| Component                     | Version       |
|-------------------------------|---------------|
| Open Traffic Generator API    | [0.4.12](https://redocly.github.io/redoc/?url=https://github.com/open-traffic-generator/models/releases/download/v0.4.12/openapi.yaml)         |
| snappi                        | [0.4.25](https://pypi.org/project/snappi/0.4.25)        |
| ixia-c-controller             | [0.0.1-2120](https://hub.docker.com/r/ixiacom/ixia-c-controller/tags)    |
| ixia-c-traffic-engine         | [1.4.0.9](https://hub.docker.com/r/ixiacom/ixia-c-traffic-engine/tags)       |
| ixia-c-app-usage-reporter     | [0.0.1-37](https://hub.docker.com/r/ixiacom/ixia-c-app-usage-reporter/tags)      |
| ixia-c-protocol-engine        | 1.00.0.50     | 
| otg-gnmi-server               | [0.4.4](https://hub.docker.com/r/otgservices/otg-gnmi-server/tags)         |
| otg-grpc-server               | [0.0.9](https://hub.docker.com/r/otgservices/otg-grpc-server/tags)         |


#### New Feature(s)

* Capture filters are now supported. Multiple patterns can be specified in the configuration.
* Controller log is now redirected to stdout. `docker logs` can now be used to access Ixia-c controller logs.
* Checksum field in `GRE` header now can be set.

#### Bug Fixes

* All patterns of IPv6 value now can be set for `increment` and `decrement` properties in flow header fields.
* Default value of step for `decrement` properties in flow header fields is now set correctly.

#### Known Issues

* The metric `loss` in flow metrics is currently not supported.


## Release v0.0.1-1622
> 25th June, 2021

#### About

This build contains support for protocols GRE and VXLAN (RFC 2784), enabling/disabling flow metrics and some bug fixes.

#### Build Details

| Component                     | Version       |
|-------------------------------|---------------|
| Open Traffic Generator API    | [0.4.0](https://redocly.github.io/redoc/?url=https://github.com/open-traffic-generator/models/releases/download/v0.4.0/openapi.yaml) |
| snappi                        | 0.4.0         |
| ixia-c-controller             | 0.0.1-1622    |
| ixia-c-traffic-engine         | 1.4.0.1       |
| ixia-c-app-usage-reporter     | 0.0.1-36      |

#### New Feature(s)

* Flow header configuration for protocols `GRE` and `VXLAN (RFC 2784)` are now supported.
* Flow metrics is now disabled by default to allow transmitting packets with `unaltered payload `(i.e. without any timestamps and instrumentation bytes embedded in it).
* Flow metrics (including metrics that are its sub-properties, e.g. `latency` and `timestamp`) can now be explicitly enabled on per-flow basis.

#### Bug Fixes

* `ixia-c-controller` can now safely serve multiple parallel requests from different clients preventing any undefined behavior.
* Port metrics can now be fetched for ports which are not part of flow configuration.
* Providing port locations for `ixia-c-traffic-engine` running in unsupported mode will now throw a user-friendly error.
* Default values for `increment` and `decrement` properties in flow header fields are now aligned per Open Traffic Generator API.

#### Known Issues

* Checksum field in `GRE` header currently cannot be set.
* The metric `loss` in flow metrics is currently not supported.


## Release v0.0.1-1388
> 31st May, 2021

#### About

This build contains support for flow delay and some bug fixes.

#### Build Details

| Component                     | Version       |
|-------------------------------|---------------|
| Open Traffic Generator API    | [0.3.10](https://redocly.github.io/redoc/?url=https://github.com/open-traffic-generator/models/releases/download/v0.3.10/openapi.yaml) |
| snappi                        | 0.3.20        |
| ixia-c-controller             | 0.0.1-1388    |
| ixia-c-traffic-engine         | 1.2.0.12      |
| ixia-c-app-usage-reporter     | 0.0.1-36      |

#### New Feature(s)

* Ixia-c now supports `delay` parameter in flow configuration.  Refer to [v0.3.10](https://redocly.github.io/redoc/?url=https://github.com/open-traffic-generator/models/releases/download/v0.3.10/openapi.yaml) of the Open Traffic Generator API specification for more details.

#### Bug Fixes

* The flow configuration parameter `inter_burst_gap` when specified in nanoseconds can now be set to a value larger than 4.2 seconds.
* Invalid values can now be set for the `phb` (per hob behavior) field in the DSCP bits in the IPv4 header.
* The `set_config` method will return an error when flows are over subscribed.
* Fixed an error in calculation for packet counts when `duration` is set in terms of fixed_seconds.

#### Known Issues

* The metrics `frames_rx_rate` and `bytes_rx_rate` in port statistics are not calculated correctly and are always zero.
* The metric `min_latency_ns` in flow statistics is not calculated correctly and is always zero.
