netflowipfix

https://www.iana.org/assignments/enterprise-numbers/enterprise-numbers

https://www.plixer.com/blog/netflow/ipfix-information-elements-vs-netflow-elements/

https://www.plixer.com/blog/cisco-netflow/exporting-netflow-or-ipfix/

import https://www.juniper.net/documentation/en_US/junos/topics/concept/services-exporting-ipfix-flow-data-collector.html

pattern findter
https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=8&cad=rja&uact=8&ved=2ahUKEwiU4J7p15veAhWYc3AKHUoFCDoQFjAHegQIARAC&url=https%3A%2F%2Fis.muni.cz%2Fth%2Fp7gwp%2Ftext_prace.pdf&usg=AOvVaw30ISRVAWA5urhXH9-LpONm



tcpreplay tcprewrite
https://jaceklaskowski.gitbooks.io/apache-kafka/kafka-properties-bootstrap-servers.html

https://serverfault.com/questions/744142/how-to-send-captured-packets-to-a-different-destination
https://nemea.liberouter.org/

https://www.google.com/search?client=ubuntu&channel=fs&q=tcprewrite%2Btcpreplay&ie=utf-8&oe=utf-8
http://staff.cesnet.cz/~krejci/ipfixcol/doc/files.html


ipfix format:
https://www.iana.org/assignments/ipfix/ipfix.xhtml

1.start spark
---------------------
export SPARK_MASTER_HOST=192.168.56.12

export SPARK_LOCAL_HOST=192.168.56.101

export SPARK_WORKER_WEBUI_PORT="8080"

export SPARK_LOCAL_IP=192.168.56.101

opt/spark/spark-bin/sbin/start-slave.sh spark://192.168.56.12:7077 -m 2000M

2.sparkMaster
-------------------------
/opt/kafka/bin/kafka-topics.sh --list --zookeeper localhost:2181
-----------host----------------
./run-application.sh statistics/hosts_statistics/spark/application_template.py -iz producer:2181 -it ipfix.entry -oz producer:9092
 -ot results.output -n 0.10.0.0/0 -wd 10 -ws 10 -c 10

./run-application.sh ./statistics/hosts_statistics/spark/host_stats.py -iz producer:2181 -it ipfix.entry -oz producer:9092 -ot results.output -ln "0.0.0.0/0" -w 10 -m 10

--------------------------dns_statistic---------------------
/home/spark/applications/run-application.sh /home/spark/applications/statistics/dns_statistics/spark/dns_statistics.py -iz producer:2181 -it ipfix.entry -oz producer:9092 -ot results.output -ln 0.0.0.0/0
--------------------------detect-ddos--------
/home/spark/applications/run-application.sh /home/spark/applications/detection/ddos/spark/detection_ddos.py -iz producer:2181 -it ipfix.entry -oz producer:9092 -ot results.output -nf "192\.168\..+"

192\.168\.2\.\d+

\d+
--------------------------extenal dns
/home/spark/applications/run-application.sh /home/spark/applications/detection/dns_external_resolvers/spark/dns_external_resolvers.py -iz producer:2181 -it ipfix.entry -oz producer:9092 -ot results.output -ln 0.0.0.0/0
--------------------------detect ssh-auth
/home/ubuntu/applications/run-application.sh /home/ubuntu/applications/detection/ssh_auth_simple/spark/ssh_auth_simple.py -iz producer:2181 -it ipfix.entry -oz producer:9092 -ot results.output
--------------------------parten filter


pip install netaddr

./run-application.sh ./statistics/hosts_statistics/spark/host_stats.py -iz producer:2181 -it ipfix.entry -oz producer:9092 -ot results.output -ln "3.0.0.0/24" -w 10 -m 10

3.producer
---------------------
nano /opt/kafka/config/server.properties
listeners=PLAINTEXT://producer:9092
ipfixsend -i test-data.ipfix -d 192.168.56.11 -p 4739 -t UDP -n 10

/opt/kafka/bin/kafka-topics.sh --list --zookeeper localhost:2181

__consumer_offsets

ipfix.entry

results.output


/opt/kafka/bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic ipfix.entry --from-beginning

{"@type": "ipfix.entry", "ipfix.octetDeltaCount": 59, "ipfix.packetDeltaCount": 1, "ipfix.protocolIdentifier": 17, "ipfix.ipClassOfService": 0, "ipfix.sourceTransportPort": 8278, "ipfix.sourceIPv4Address": "3.0.0.0", "ipfix.ingressInterface": 0, "ipfix.destinationTransportPort": 53, "ipfix.destinationIPv4Address": "8.8.8.8", "ipfix.egressInterface": 0, "ipfix.samplingInterval": 0, "ipfix.samplingAlgorithm": 0, "ipfix.ipVersion": 4, "ipfix.flowStartMilliseconds": 1489141087955, "ipfix.flowEndMilliseconds": 1489141087955}


/opt/kafka/bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic results.output --from-beginning

{"src_ip":"3.0.0.0","stats":{"total":{"packets":40,"bytes":5600,"flow":40},"avg_flow_duration":0.0,"dport_count":2,"peer_number":2},"@type":"host_stats"}

{"@type": "protocols_statistics", "protocol": "udp", "flows": 1, "packets": 2, "bytes": 248}

{"@type": "protocols_statistics", "protocol": "other", "flows": 20, "packets": 20, "bytes": 1680}

{"@type": "protocols_statistics", "protocol": "tcp", "flows": 60, "packets": 60, "bytes": 360480}




