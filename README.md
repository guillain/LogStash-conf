# LogStash-conf
Configuration of LogStash for Analytics treatment.


Provide set of configuration files for:
* Logstash data collect and treatment
* Filebeat template to transfert your logs to logstash

## Design 
Best Pratice for _real usage_

Repartition by Virtual Machine
0/ Source
* _Install:_ filebeat, syslog (UDP), JSON/TCP
* _What:_ Dedicated VM where the data source is/come from

1/ Data collection
* _Install:_ logstash
* _What:_ Collect all source, index and push it to elasticsearch

2/ Storage
* _Install:_ elasticsearch
* _What:_ Store and index all data

3/ Interface
* _Install:_ kibana
* _What:_ Provide web interface for analytics reports

## Configuration files
That's preconfigured files to be used like that. 
No change is ok for run but some configuration adaption are require for production usage.

### LogStash
Lot of filters are ready and just you need to adapt your input (connectors) according to your systems.

#### Input
* 001-syslog-input.conf	--> UDP/5000
* 002-beats-input.conf	--> TCP/5044
* 004-bot-input.conf    --> TCP/5055

#### Filter
Most of them with based on the patterns included in the logstash-patterns-core distribution:
* 001-syslog-input.conf
* 002-beats-input.conf
* 100-syslog-filter.conf
* 101-apache-filter.conf
* 102-audit-filter.conf
* 103-login-filter.conf
* 104-bot-filter.conf
* 105-redis-filter.conf
* 106-aws-filter.conf
* 107-cisco-filter.conf
* 108-mongodb-filter.conf
* 109-nagios-filter.conf
* 110-java-filter.conf
* 111-haproxy-filter.conf
* 112-ruby-filter.conf
* 113-netscreen-filter.conf
* 114-sharewall-filter.conf
* 115-postgresql-filter.conf
* 116-rails-filter.conf
* 117-bro-filter.conf
* 118-cucmcdr-filter.conf

#### Output
Elasticsearch only at this time... to be continued
* 300-elasticsearch-output.conf
* 399-debug-output.conf

### FileBeat
Provide log template to quickly integarte on your host where the data should come.
* syslog
* audit
* redis
* apache
* apache-other-vhost
* apache-error
* dpkg
* cucm-cdr
* cucm-cmr
* bot: based on JSON and dynamic index

## Cisco CDR
Thanks to [Damienetwork](https://damienetwork.wordpress.com) and its [website](https://damienetwork.wordpress.com/2015/10/09/elk-setup-for-cucm-cdr/)

I've not integrated the mecanism to get the CDR/CMR from the CUCM, up to you to add your choice.

### ToDo list
* Install logstash-filter-translate in Logstash
* Import JSON in elasticsearch and kibana (to find in the Damien's site)

## GeoIP
Thanks to:[manicas](https://www.digitalocean.com/community/users/manicas) and its [article](https://www.digitalocean.com/community/tutorials/how-to-map-user-location-with-geoip-and-elk-elasticsearch-logstash-and-kibana)

[GeoIP](http://geolite.maxmind.com/download/geoip/database/GeoLiteCity.dat.gz) is included in apache and syslog conf...
* To continue in other configuration
* Or can declared as generic field under 'clientip' name

## HowTo
Principle:
* Dearchive the package
* Copy the content in your logstash configuration folder
* Adapt the conf to your elasticseaerch server in the 3xx output 
* Download the GeoIP database
* Install the CUCM CDR/CDR features

### Example on Logstash server
```bash
git clone https://github.com/guillain/LogStash-conf
cp LogStash-conf/logtash/* /etc/logstash/conf.d/
cd /etc/logstash/
curl -O "http://geolite.maxmind.com/download/geoip/database/GeoLiteCity.dat.gz"
gunzip GeoLiteCity.dat.gz
/opt/logstash/bin/plugin install logstash-filter-translate
service logstash restart
```

### Example on CentOS server to collect with Filebeat
```bash
yum install filebeat
git clone https://github.com/guillain/LogStash-conf
mv /etc/filebeat/filebeat.yml /etc/filebeat/filebeat.yml.orig
cp LogStash-conf/filebeat/filebeat.yml /etc/filebeat/filebeat.yml
service filebeat restart
```

## Configuration validated
### Input
* syslog
* filebeat
* * syslog
* * auditlog
* * apache
* * CUCM CDR/CRM
* logstash
* NodeJS bot
* Python bot
### Filter
* syslog
* apache
* audit
* login
* bot
### Ouput
* logstash
* elasticsearch
* redis

## Have fun :)
