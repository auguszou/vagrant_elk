### tgz
1. curl -Lo jdk-8u152-ea-bin-b05-linux-x64-20_jun_2017.tar.gz http://download.java.net/java/jdk8u152/archive/b05/binaries/jdk-8u152-ea-bin-b05-linux-x64-20_jun_2017.tar.gz (using openjdk8)
1. curl -Lo elasticsearch-5.6.1.tar.gz https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.6.1.tar.gz
1. curl -Lo logstash-5.6.1 https://artifacts.elastic.co/downloads/logstash/logstash-5.6.1.tar.gz
1. curl -Lo kibana-5.6.1-linux-x86_64.tar.gz https://artifacts.elastic.co/downloads/kibana/kibana-5.6.1-linux-x86_64.tar.gz

### setup (done in vagrant up)
1. setup java sdk (using openjdk8)
1. untar all packages
1. cp  nginx.conf
1. update `cluster.name`, `node.name`, `network.host`, `http.port` in config/elasticsearch.yml
1. update `elasticsearch.url: "http://localhost:9200"` in config/kibana.conf
1. cp config/logstash_agent.conf && config/logstash_indexer.conf

### services (done in vagrant up)
1. sudo /etc/init.d/nginx start
1. sudo /etc/init.d/redis start
1. ./bin/elasticsearch
1. ./bin/loadstash -f config/loadstash_agent.conf
1. ./bin/kibana

### test
1. curl http://localhost:8080 # nginx
1. curl http://locahost:9200  # es
1. curl http://locahost:9002  # logstash
1. curl http://localhost:55601 # kibana