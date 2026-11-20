# RemotiveTopology quickstart

A minimal RemotiveTopology project: a small sample vehicle described by two DBC files, with three of its ECUs running
as mocks and RemotiveBrokers. It is the finished project of the tutorial
[Your first topology](https://developer.remotivelabs.com/docs/get-started/first-topology), which explains every file.

```
databases/
├── body_can.dbc            # BodyCan: BCM sends TurnLightControl
└── driver_can.dbc          # DriverCan: SCCM sends HazardLightButton
topology/
├── vehicle.platform.yaml   # the platform: names the two CAN channels
├── main.instance.yaml      # the instance: SCCM and BCM as mocks, FLCM as a RemotiveBroker
└── udp.instance.yaml       # use UDP instead of SocketCAN (macOS, or without RemotiveBus)
remotive.yaml               # the RemotiveTopology workspace
```

## Prerequisites

- RemotiveCLI and RemotiveTopology, see [Install RemotiveTopology](https://developer.remotivelabs.com/docs/get-started/install)
- An active subscription: `remotive topology subscription status`
- Docker

## Run it

Use this repository as a template ("Use this template" on GitHub), or clone it, and run all commands in its root.
The workspace is already initialized (`remotive.yaml`), so skip `remotive topology workspace init`.

Build the topology. On macOS, or if you haven't installed RemotiveBus, add `-f topology/udp.instance.yaml`:

```sh
remotive topology build -f topology/main.instance.yaml --name my-vehicle output
```

Start it and keep it running:

```sh
docker compose -f output/my-vehicle/docker-compose.yml up
```

In a new terminal, subscribe to the turn light requests that `FLCM` receives from the `BCM` mock:

```sh
remotive broker signals subscribe \
  --signal FLCM-BodyCan:TurnLightControl.LeftTurnLightRequest \
  --signal FLCM-BodyCan:TurnLightControl.RightTurnLightRequest
```

Or open the project and the running topology in RemotiveStudio:

```sh
remotive studio .
```

Stop the topology with Ctrl-C, then remove its containers and networks:

```sh
docker compose -f output/my-vehicle/docker-compose.yml down
```

## Next steps

- Give the `BCM` behavior with a [behavioral model](https://developer.remotivelabs.com/docs/remotive-topology/usage/ecus/behavioral-models)
  and [write tests](https://developer.remotivelabs.com/docs/remotive-topology/usage/test).
- The [`getting_started`](https://github.com/remotivelabs/remotivelabs-topology-examples/tree/main/getting_started) example
  in [remotivelabs-topology-examples](https://github.com/remotivelabs/remotivelabs-topology-examples) adds a behavioral model and a test.
- [RemotiveCar](https://github.com/remotivelabs/remotivelabs-remotive-car) is a larger example with 14 ECUs, Android
  and a 3D car.

## License

The files in this repository are licensed under the [MIT License](./LICENSE), so you are free to copy, modify and reuse
them in your own projects.

Running the topology, however, requires a RemotiveLabs account. The RemotiveLabs software it depends on (such as
RemotiveBroker, RemotiveTopology and RemotiveCLI) is not covered by the MIT License; it is provided under the
RemotiveLabs Terms of Use at https://www.remotivelabs.com/license.

This repository is generated from the
[RemotiveLabs documentation](https://developer.remotivelabs.com/docs/get-started/first-topology); changes made here
are overwritten.
