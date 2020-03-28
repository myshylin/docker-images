# Ubuntu based docker image with nightly dprep

## Content
 * azureml-core
 * azureml-dataprep
 * numpy
 * pandas
 * pyarrow
 * fuse package installed on ubuntu to allow mount
 * jupyter notebook open to all connections on port `3056`

## Arguments
 * PYTHON_VERSION - 3.7 by default
 * DPREP_INDEX_URL - dprep package wheel url (pypi by default)

## Usage

Starts single instance of docker container. Exposes internal port `3056` with jupyter as host port `3056`
Mounts folders (on windows [driver sharing](https://rominirani.com/docker-on-windows-mounting-host-directories-d96f3f056a2c) should be enabled):
| host          | container         |
| ------------- | ----------------- |
| `./src`       | `/home/src`       |
| `./testfiles` | `/home/testfiles` |

Build: `docker-compose build`

Rebuild from scratch: `docker-compose build --no-cache --pull`

Start: `docker-compose up`

To use private index build uncomment `DPREP_INDEX_URL` arg declaration in `docker-compose.yaml` and set it to index url.