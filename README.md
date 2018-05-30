# Stellar Docker Compose

This is an alternate way to spin up `stellar-core` and `horizon` with their associated databases. It was inspired by the gist example from [Satoshi Labs](https://gist.github.com/andrenarchy/bf56cfbded42da48e2e255a11de7cb49). The images build and run from this directory. There are prebuild images for some versions of `horizon` and `stellar-core` in the following quay repositories:

- `quay.io/jackzampolin/horizon`
- `quay.io/jackzampolin/stellar-core`

### Running

To run this just pull down the repository (`git clone github.com/jackzampolin/stellar-docker-compose`) and run `docker-compose up`. That will pull down the prebuilt images and run them. Data will be stored in the `./data/testnet` directory. Once all the services are up you should see logs streaming and `horizon` will be available at `localhost:8000`

### Building

To build the images from scratch just pull down this repo and uncommment the following sections in the `docker-compose.yaml` file:

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
```
If you would like to change the version of either `stellar-core` or `horizon` you can do so in the `./.env` file. That file also controls the location of the data directory. 
