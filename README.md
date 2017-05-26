# LogStash-conf
Configuration of LogStash for Analytics treatment.

## Configuration files
That's preconfigured files to be used like tat.
Lot of filters are ready and just you need to adapt your input (connectors) according to your systems.

### Input
* 001-syslog-input.conf	--> UDP/514
* 002-beats-input.conf	--> TCP/5044

### Filter
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

### Output
Elasticsearch only at this time... to be continued
* 300-elasticsearch-output.conf


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

**Example**
```bash
service logstash stop
mv /etc/logstash /etc/logstash.bkp
git clone https://github.com/guillain/LogStash-conf
mv LogStash-conf /etc/logstash
cd /etc/logstash/
curl -O "http://geolite.maxmind.com/download/geoip/database/GeoLiteCity.dat.gz"
gunzip GeoLiteCity.dat.gz
service logstash start
```

## Configuration validated
### Input
* syslog
* filebeat
* logstash
### Filter
* syslog
* apache
* audit
* login
### Ouput
* logstash
* elasticsearch
* redis

## Have fun :)
