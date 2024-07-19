#### Build Details

| Component                     | Version       |
|-------------------------------|---------------|
| Open Traffic Generator API    | [1.6.2](https://redocly.github.io/redoc/?url=https://raw.githubusercontent.com/open-traffic-generator/models/v1.6.2/artifacts/openapi.yaml)         |
| snappi                        | [1.6.2](https://pypi.org/project/snappi/1.6.2)        |
| gosnappi                      | [1.6.2](https://pkg.go.dev/github.com/open-traffic-generator/snappi/gosnappi@v1.6.2)        |
| keng-controller               | [1.6.2-13](https://github.com/orgs/open-traffic-generator/packages/container/package/keng-controller)    |
| ixia-c-traffic-engine         | [1.8.0.12](https://github.com/orgs/open-traffic-generator/packages/container/package/ixia-c-traffic-engine)       |
| keng-app-usage-reporter       | [0.0.1-52](https://github.com/orgs/open-traffic-generator/packages/container/package/keng-app-usage-reporter)      |
| ixia-c-protocol-engine        | [1.00.0.390](https://github.com/orgs/open-traffic-generator/packages/container/package/ixia-c-protocol-engine)    | 
| keng-layer23-hw-server        | [1.6.2-4](https://github.com/orgs/open-traffic-generator/packages/container/package/keng-layer23-hw-server)    |
| keng-operator                 | [0.3.30](https://github.com/orgs/open-traffic-generator/packages/container/package/keng-operator)        | 
| otg-gnmi-server               | [1.14.6](https://github.com/orgs/open-traffic-generator/packages/container/package/otg-gnmi-server)         |
| ixia-c-one                    | [1.6.2-13](https://github.com/orgs/open-traffic-generator/packages/container/package/ixia-c-one/)         |
| UHD400                        | [1.3.3](https://downloads.ixiacom.com/support/downloads_and_updates/public/UHD400/1.3/1.3.3/artifacts.tar)         |


# Release Features(s)
* <b><i>Ixia-C, Ixia Chassis & Appliances(Novus, AresOne)</i></b>: gNMI support added for `first-timestamp` and `last-timestamp` in flow metrics. [details](https://github.com/open-traffic-generator/models-yang/blob/main/artifacts/open-traffic-generator-flow.txt)
  - User needs to set `flows[i].metrics.timestamps=true` to fetch these new fields.

  ```gNMI
    module: open-traffic-generator-flow​
    +--rw flows​
      +--ro flow* [name]​
        +--ro name              -> ../state/name​
        +--ro state​
          |  +--ro name?              string​
          .........
          |  +--ro first-timestamp?   decimal64​
          |  +--ro last-timestamp?    decimal64​
  ``` 

### Bug Fix(s)
* <b><i>Ixia Chassis & Appliances(Novus, AresOne)</i></b>: Issue where `devices[i].rsvp.ipv4_interfaces[j].summary_refresh_interval` was not setting correctly, is fixed.

* <b><i>Ixia Chassis & Appliances(Novus, AresOne)</i></b>: Issue where destination mac address was not getting resolved properly for traffic over ISIS IPv6 routes, is fixed.


#### Known Issues
* <b><i>Ixia Chassis & Appliances(Novus, AresOne)</i></b>: If `keng-layer23-hw-server` version is upgraded/downgraded, the ports which will be used from this container must be rebooted once before running the tests.
* <b><i>Ixia Chassis & Appliances(Novus, AresOne)</i></b>: `StartProtocols`/`set_control_state.protocol.all.start` can get stuck till the time all DHPCv4 clients receive the leased IPv4 addresses from the DHCPv4 server/relay agent. This may result in getting `"context deadline exceeded"` error in the test program.
* <b><i>UHD400</i></b>: Packets will not be transmitted if `flows[i].rate.pps` is less than 50.
* <b><i>UHD400</i></b>: `values` for fields in flow packet headers can be created with maximum length of 1000 values.
* <b><i>Ixia-C</i></b>: Flow Tx is incremented for flow with tx endpoints as LAG, even if no packets are sent on the wire when all active links of the LAG are down. 
* <b><i>Ixia-C</i></b>: Supported value for `flows[i].metrics.latency.mode` is `cut_through`.
* <b><i>Ixia-C</i></b>: The metric `loss` in flow metrics is currently not supported.
* <b><i>Ixia-C</i></b>: When flow transmit is started, transmission will be restarted on any existing flows already transmitting packets.