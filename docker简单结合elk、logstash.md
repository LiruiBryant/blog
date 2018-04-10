---
title: docker简单结合elk、logstash
date: 2018-02-13 10:40:28
tags: logstash
---

## 本文记录docker搭建elk，并简单结合logstash日志输出
### filebeat
> docker pull docker.elastic.co/beats/filebeat:6.2.1

- filebeat.yml
**enabled要改为true，filebeat默认为false**
```env
filebeat.prospectors:
- type: log
  enabled: true
  paths:
    - D:\workspace\baidu\baiyi\baidu\dsp\dsp-main-server\log\*.log
output:
    logstash:
          hosts: ["192.168.99.100:5044"]
```
> docker run -it --name filebeat -v /data/filebeat.yml::/usr/share/filebeat/filebeat.yml

### logstash
改写/opt/logstash/logstash/config/logstash.yml文件
- logstash.yml

```env
input{
    beats {
        port => 5044
    }
}
output{
    stdout{ codec => rubydebug }
    elasticsearch {
        hosts => "192.168.99.100:9200"
        index =>  "t-server-%{+YYYY.MM.dd}"
        document_type => "log4j_type"
    }
}
```
