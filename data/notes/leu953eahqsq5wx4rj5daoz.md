
Kibana is a user interface that provides search and data visualization for data indexed in Elasticsearch, as well as management tools for an Elasticsearch cluster.

## Installation

For a local Docker installation, we use the same docker-compose as Elasticsearch:

![[database.elasticsearch#disabling-elasticsearch-security-settings,1:#*]]

However, we add a service for Kibana. The benefit is that services in the same docker-compose file use the same virtual network, so they should discover and able to speak to each other out of the box. (No need to worry about the enrollment token and all that.)

The Kibana service looks like this:

``` yml
kibana:
    container_name: kibana_8.1
    image: docker.elastic.co/kibana/kibana:8.1.0
    ports:
      - 5601:5601
```

## Using Kibana

### Index Management

Useful for checking index status and configs.

<http://localhost:5601/app/management/data/index_management/indices>

### Dev tools

Useful for sending custom queries.

<http://localhost:5601/app/dev_tools#/console>