# Creative Commons Catalog API

## Purpose

The Creative Commons Catalog API ('cccatalog-api') is a system that allows programmatic access to public domain digital media. It is our ambition to index and catalog [billions of Creative Commons works](https://stateof.creativecommons.org/), including articles, songs, videos, photographs, paintings, and more. Using this API, developers will be able to access the digital commons in their own applications.

This repository is primarily concerned with back end infrastructure like datastores, servers, and APIs. The pipeline that feeds data into this system can be found in the [cccatalog repository](https://github.com/creativecommons/cccatalog). A front end web application that interfaces with the API can be found at the [cccatalog-frontend repository](https://github.com/creativecommons/cccatalog).

## API Documentation

Browsable API documentation can be found [here](https://api.creativecommons.engineering).

## Running the server locally

Ensure that you have installed [Docker](https://docs.docker.com/install/) and that the [Docker daemon is running](https://docs.docker.com/config/daemon/).
```
git clone https://github.com/creativecommons/cccatalog-api.git
cd cccatalog-api
docker-compose up
```

After executing this, you will be running:
* A Django API server
* Two PostgreSQL (one simulates the upstream data source, the other serves as the application database)
* Elasticsearch
* Redis
* Ingestion Server, a microservice for bulk ingesting and indexing search data.

Once everything has initialized, load the sample data.

```
./load_sample_data.sh
```

You are now ready to start sending the API server requests. Hit the API with a request to make sure it is working:
`curl localhost:8000/image/search?q=honey`

## System Architecture
![System Architecture](https://raw.githubusercontent.com/creativecommons/cccatalog-api/master/system_architecture.png)

## Running the tests

### Running API live integration tests
You can check the health of a live deployment of the API by running the live integration tests.
```
cd cccatalog-api
virtualenv venv
pip install -r requirements.txt
source venv/bin/activate
cd test
export INTEGRATION_TEST_URL="http://api.creativecommons.engineering"
pytest -s
```

### Running Ingestion Server test
This end-to-end test ingests and indexes some dummy data using the Ingestion Server API.

```
cd ingestion_server
virtualenv venv
pip install -r requirements.txt
source venv/bin/activate
python3 test/integration_tests.py
```

## Deploying the API
The API infrastructure is orchestrated using Terraform hosted in creativecommons/ccsearch-infrastructure. More details can be found on the [this wiki page](https://wikijs.creativecommons.org/tech/cc-search/operations).
