

## Installation

### Docker image

Elasticsearch can be easily installed using [[linux.docker]].

For ElasticSearch 8:

```bash
docker network create elastic
docker pull docker.elastic.co/elasticsearch/elasticsearch:8.1.0
docker run -d -p 9200:9200 -p 9300:9300 --name elasticsearch_8 \
  --net elastic -t docker.elastic.co/elasticsearch/elasticsearch:8.1.0
```

### Disabling Elasticsearch Security Settings

We must disable some security settings to easily connect to our container locally (we leave them on in prod). The settings to disable are: `xpack.security.enabled`, `xpack.security.http.ssl`, and `xpack.security.transport.ssl`.

I found the easiest method is to create a [[docker-compose|linux.docker#docker-compose,1:#*]] file and bind-mount a config file:

```yml
version: '3'
services:
  elasticsearch:
    container_name: elasticsearch_8.1
    image: docker.elastic.co/elasticsearch/elasticsearch:8.1.0
    ports:
      - 9200:9200
      - 9300:9300
    volumes:
      - ./elasticsearch.yml:/usr/share/elasticsearch/config/elasticsearch.yml
```

`elasticsearch.yml` has the following settings:

```yml
cluster.name: "docker-cluster"
node.name: node1
network.host: 0.0.0.0
xpack.security.enabled: false
xpack.security.enrollment.enabled: true
xpack.security.http.ssl:
  enabled: false
xpack.security.transport.ssl:
  enabled: false
cluster.initial_master_nodes:
  - node1

```

### Virtual Memory

I ran into a virtual memory problem:

> Elasticsearch: Max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]

Solution:
https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html

```bash
sysctl -w vm.max_map_count=262144
```

To modify this setting permanently, edit `/etc/sysctl.conf` and set `vm.max_map_count` to  262144.

### Connect to ES using CA certs

Install Docker image, but don't disable security settings.

Copy the CA certs to the host machine:

```
cp elasticsearch_8:/usr/share/elasticsearch/config/certs/http_ca.crt .

```

You can store the cert file in `/etc/ssl/certs` for system-wide access.

Connection string with simple authentication:

```
curl --cacert http_ca.crt -u elastic:<password> https://localhost:9200
```

Create an API key:

```
curl -X POST --cacert http_ca.crt -u elastic:<password> \
  https://localhost:9200/_security/api_key \
  -H 'Content-Type: application/json' -d'{"name": "my_api_key"}'
```
The API key is the 'encoded' value in the JSON response.

To connect with an API key:

```
curl --cacert http_ca.crt -H \
  "Authorization: ApiKey <encoded_api_key>" https://localhost:9200
```