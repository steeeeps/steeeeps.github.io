---
title: logstash 使用配置纪录
description: logstash是Elastic stack中最主要的数据获取、转换工作。主要记录一下日常使用中的配置信息。
date: 2017-12-26 22:02:27
categories:
- 大数据
tags:
- ELK
- 大数据
---

logstash是Elastic stack中最主要的数据获取、转换工作。主要记录一下日常使用中的配置信息。


### 导入带有地理位置的 csv文件到 ES 中

``` bash
input
{
      file
        {
           path => "/home/steeeeps/workspace/es_datas/csv/sicuan.csv"
           # type => "testeFile_lite"
           start_position => "beginning"
           sincedb_path => "/home/steeeeps/workspace/es_datas/logs"
        }
}
filter
{
    csv{
           columns => ['poiid', 'title', 'address','lon','lat','city','category_name','checkun_num','photo_num']
           separator => ","
      }

     mutate {
         add_field => { "[location][lat]" => "%{lat}" }
         add_field => { "[location][lon]" => "%{lon}" }
     }

     mutate {
         convert => {
           "[location][lat]" => "float"
           "[location][lon]" => "float"
        }
     }

}
output
{
    elasticsearch
    {
       # action => "index"
       hosts => ["http://172.16.167.188:9200"]
       index => "weibo"
       document_type => "checkin"

       # cluster => "SIC_UTAD"
    }

    stdout {}
}
```

### 导入多边形Json数据到 ES 中

``` bash
input
{
      file
        {
           path => "/home/steeeeps/workspace/es_datas/json/sheetpolygon_10000_0.json"
           start_position => "beginning"
           sincedb_path => "/home/steeeeps/workspace/es_datas/json_logsssss"
        }
}
filter
{
    json {
        source => "message"
    }

    mutate {
        rename => { "location_group" => "location" }
    }
 
    mutate {
        remove_field => [ "message" ]
    }

	

}
output
{
   elasticsearch
    {
       #action => "index"
      hosts => ["http://172.16.167.188:9200"]
       index => "geoshape"
       document_type => "polygons"

       #cluster => "SIC_UTAD"
    }

    stdout {}
}
```

### 导入 kafka topic 中的数据到 ES 和 Mongodb 中

``` bash
input{

  kafka {
    bootstrap_servers => "localhost:9092"
    group_id => "test-consumer-group"
    topics => ["logstashData","logstashData2"]
    decorate_events => true
  }
}
filter
{
   if [kafka][topic] == "logstashData" {
      mutate {
         add_field => {"[@metadata][index]" => "logstashdata"}
      }
   } else {
      mutate {
         add_field => {"[@metadata][index]" => "logstashdata2"}
      }
   }
   # remove the field containing the decorations, unless you want them to land into ES
   mutate {
      remove_field => ["kafka"]
   }
   
	json {
        source => "message"
    }
  
	mutate {
        remove_field => [ "message" ]
    }

}
output{
  stdout{}
  elasticsearch {
    action => "index"    
    hosts  => "192.168.80.3:9200"   
    index  => "kafka-%{[@metadata][index]}"
    flush_size => 50000
    idle_flush_time => 10
  }
  mongodb {
      bulk => true
      bulk_interval => 2
      collection => "%{[@metadata][index]}"
      database => "kafkaconnect"
      # generateId => true
    codec => "json" 
    uri => "mongodb://192.168.90.61:27017"
  }
}
```



### 基于 kafka 的 topic 名称构建 es 索引

``` bash
input{

  kafka {
    bootstrap_servers => "localhost:9092"
    group_id => "test-consumer-group"
    topics => ["topic_monitor_sys"]
    decorate_events => true
  }
}
filter
{
   
   
   mutate {
        add_field => {"[@metadata][index]" => "%{[kafka][topic]}"}
   }
   
   mutate {
    gsub => [
     
      "[@metadata][index]", "topic", "index"
    ]
  }
   # remove the field containing the decorations, unless you want them to land into ES
   mutate {
      remove_field => ["kafka"]
   }
   
	json {
        source => "message"
    }
  
	mutate {
        remove_field => [ "message" ]
    }

}
output{
  stdout{}
  elasticsearch {
    action => "index"    
    hosts  => "192.168.80.3:9200"   
    index => "%{[@metadata][index]}_%{+YYYY.MM.dd}"
    flush_size => 50000
    idle_flush_time => 10
  }
  # mongodb {
  #    bulk => true
  #    bulk_interval => 2
  #    collection => "%{[@metadata][index]}"
  #    database => "kafkaconnect"
  #    # generateId => true
  #  codec => "json" 
  #  uri => "mongodb://192.168.90.61:27017"
  #}
}
```



