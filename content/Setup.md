Install Elastic Search and Kibana locally for testing or you can using Elasic cloud trial (30 days).

```docker
1. docker network create elastic
2. docker run --name elasticsearch --net elastic -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -e "xpack.security.enabled=false" elasticsearch:8.11.0
3. docker run --name kibana --net elastic -p 5601:5601 -e "ELASTICSEARCH_HOSTS=http://elasticsearch:9200" kibana:8.11.0
```

Explains:

1. When running multiple containers that need to communicate with each other, it’s recommended to create a user-defined network, this allows for easier container-to-container communication and better network management.
2. Elasticsearch
    - `--name elasticsearch` this sets the name of the container to `elasticsearch`
    - `--net elastic` this attaches the container to a Docker network created from `1`, containers on this network can communication with each other directly.
    - `-p 9200:9200` this option expose and maps port `9200` on the container to port `9200` on the host.
        
        We can only specify only the container port by `-p 9200` and the Docker will choose an available port on the host and map it to the port 9200 of the container. We can find out which port was assigned by using:
        
        ```docker
        docker port elasticsearch 9200
        ```
        
    - `-p 9300:9300` `9300` is the default port for internal cluster communication.
    - `-e discovery.type=single-node` it bypasses the need for cluster coordination settings, with this configuration, we can omit the option `-p 9300:9300`
    - `-e "xpack.security.enable=false` this enviroment variable disables X-Pack security features in Elasticsearch.
    - `elasticsearch:8.11.0` specifics the version of Elasticsearch to use, in this case, version `8.11.0`
3. Kibana
    - `-e "ELASTICSEARCH_HOSTS=http://elasticsearch:9200"` `ELASTICSEARCH_HOSTS` is a Kibana configuration parameter that specifies the URL(s) of the Elasticsearch instances to which Kibana should connect, in this case, it set to elasticsearch container at `2`

### Verify

Now, we can access to Elasticsearch by http://localhost:9200/ and Kibana by [http://localhost:5601/](http://0.0.0.0:5601/)
