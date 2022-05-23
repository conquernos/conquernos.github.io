---
layout: post
title:  "Logstash timestamp seoul"
categories: Logstash
---

### pipeline 설정
``` json
input {
    tcp {
        port => 5000
        codec => "json"
    }
    }
    filter {
        mutate {
            add_field => { "[@metadata][domain]" => "%{domain}" }
        }
        mutate {
            remove_field => [ "domain" ]
        }
        date {
            match => ["timestamp" , "yyyy-MM-dd'T'HH:mm:ss.SSS"]
            target => "@timestamp"
        }
        mutate {
            remove_field => [ "timestamp" ]
        }
        if [@metadata][domain] == "search-root" {
            mutate {
                gsub => [
                "message", ":,", ":{},"
                ]
            }
            json {
                source => "message"
                skip_on_invalid_json => true
            }
            if [uri] == "/status/health" {
                drop { }
            }
            mutate {
                remove_field => [
                "headers"
                , "dd.span_id"
                , "dd.env"
                , "dd.env"
                , "dd.trace_id"
                , "dd.version"
                , "dd.service"
                , "tags"
                , "thread_name"
                , "level_value"
                , "@version"
                , "logger_name"
                ]
            }
        }
    }
    output {
        elasticsearch {
            hosts => ["elasticsearch-log-node:9200"]
            index => "%{[@metadata][domain]}-logs"
            user => "elastic"
            password => "0mlY6qB2JD59BYxb7LkS"
            action => "create"
        }
    }
}
``````