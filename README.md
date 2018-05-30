# Stellar Docker Compose

This is an alternate way to spin up [`stellar-core`](https://github.com/stellar/stellar-core) and [`horizon`](https://github.com/stellar/go/tree/master/services/horizon) with their associated databases. It was inspired by the gist example from [Satoshi Labs](https://gist.github.com/andrenarchy/bf56cfbded42da48e2e255a11de7cb49). The images build and run from this directory. There are prebuilt images for some versions of `horizon` and `stellar-core` in the following quay repositories:

- [`quay.io/jackzampolin/horizon`](https://quay.io/repository/jackzampolin/horizon?tab=tags)
- [`quay.io/jackzampolin/stellar-core`](https://quay.io/repository/jackzampolin/stellar-core?tab=tags)

### Dependancies

This getting started depends on `docker` and `docker-compose`. They must be installed and running:

- [Install `docker`](https://docs.docker.com/install/)
- [Install `docker-compose`](https://docs.docker.com/compose/install/)

### Running

To run this just pull down the repository (`git clone github.com/jackzampolin/stellar-docker-compose`) and run `docker-compose up`. That will pull down the prebuilt images and run them. Data will be stored in the `./data/testnet` directory. Once all the services are up you should see logs streaming and `horizon` will be available at `localhost:8000`.

The configuration file for `stellar-core` is stored in the `./stellar-core` directory. Add additional configurations there (e.g. for `mainnet`). To change the configuration run when starting the system change the following line in the `docker-compose.yaml`:

```yaml
...
stellar-core:
...
  volumes:
    # Configuration file
    - "/path/to/my/stellar-core.cfg:/stellar-core.cfg"
...
```

### Building

To build the images from scratch just pull down this repo and uncomment the following sections in the `docker-compose.yaml` file:

```yaml
...
stellar-core:
  build:
   context: ./stellar-core
   args:
     STELLAR_CORE_VERSION: $STELLAR_CORE_VERSION
...
horizon:
  build:
    context: ./horizon
    args:
      HORIZON_VERSION: $HORIZON_VERSION
...
```

If you would like to change the version of either `stellar-core` or `horizon` you can do so in the `./.env` file. That file also controls the location of the data directory. After making your desired changes, run `docker-compose up --build` to build `stellar-core` and `horizon` containers locally.
