### tgz
1. curl http://download.java.net/java/jdk8u152/archive/b05/binaries/jdk-8u152-ea-bin-b05-linux-x64-20_jun_2017.tar.gz
1. curl https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.6.1.tar.gz
1. curl https://artifacts.elastic.co/downloads/kibana/kibana-5.6.1-linux-x86_64.tar.gz
1. curl https://artifacts.elastic.co/downloads/kibana/kibana-5.6.1-linux-x86_64.tar.gz

### setup (done in vagrant up)
1. setup java sdk
1. untar all packages
1. cp  nginx.conf
1. update `cluster.name`, `node.name`, `network.host`, `http.port` in config/elasticsearch.yml
1. update `elasticsearch.url: "http://localhost:9200"` in config/kibana.conf
1. cp config/logstash_agent.conf && config/logstash_indexer.conf

### services
1. sudo /etc/init.d/nginx start # done in vagrant up
1. sudo /etc/init.d/redis start # done in vagrant up
1. ./bin/elasticsearch
1. ./bin/loadstash -f config/loadstash_agent.conf
1. ./bin/kibana

### test
1. curl://localhost:8080 # nginx
1. curl://locahost:9002  # es
1. curl://localhost:55601 # kibana