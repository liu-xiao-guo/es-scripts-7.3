POST twitter/_doc/1
{
  "user": "GB",
  "uid": 1,
  "city": "Shenzhen",
  "province": "Guangdong",
  "country": "China"
}

HEAD twitter/_doc/1

GET twitter/_doc/1

POST twitter/_doc/
{
  "user": "GB",
  "uid": 1,
  "city": "Wuhan",
  "province": "Hubei",
  "country": "China"
}

PUT twitter/_doc/1
{
   "user": "GB",
   "uid": 1,
   "city": "北京",
   "province": "北京",
   "country": "中国",
   "location":{
     "lat":"29.084661",
     "lon":"111.335210"
   }
}

POST twitter/_update/1
{
  "doc": {
    "city": "成都",
    "province": "四川"
  }
}

POST twitter/_search


POST twitter/_update_by_query
{
  "script": {
    "source": "ctx._source.city = params.city;ctx._source.province = params.province;ctx._source.country = params.country",
    "lang": "painless",
    "params": {
      "city": "上海",
      "province": "上海",
      "country": "中国"
    },
    "query": {
      "match": {
        "user": "GB"
      }
    }
  }
}

POST twitter/_update/1
{
  "script" : {
      "source": "ctx._source.city=params.city",
      "lang": "painless",
      "params": {
        "city": "长沙"
      }
  }
}

DELETE twitter

POST twitter/_search

POST _bulk
{ "index" : { "_index" : "twitter", "_id": 1} }
{"user":"双榆树-张三","message":"今儿天气不错啊，出去转转去","uid":2,"age":20,"city":"北京","province":"北京","country":"中国","address":"中国北京市海淀区","location":{"lat":"39.970718","lon":"116.325747"}}
{ "index" : { "_index" : "twitter", "_id": 2 }}
{"user":"东城区-老刘","message":"出发，下一站云南！","uid":3,"age":30,"city":"北京","province":"北京","country":"中国","address":"中国北京市东城区台基厂三条3号","location":{"lat":"39.904313","lon":"116.412754"}}
{ "index" : { "_index" : "twitter", "_id": 3} }
{"user":"东城区-李四","message":"happy birthday!","uid":4,"age":30,"city":"北京","province":"北京","country":"中国","address":"中国北京市东城区","location":{"lat":"39.893801","lon":"116.408986"}}
{ "index" : { "_index" : "twitter", "_id": 4} }
{"user":"朝阳区-老贾","message":"123,gogogo","uid":5,"age":35,"city":"北京","province":"北京","country":"中国","address":"中国北京市朝阳区建国门","location":{"lat":"39.718256","lon":"116.367910"}}
{ "index" : { "_index" : "twitter", "_id": 5} }
{"user":"朝阳区-老王","message":"Happy BirthDay My Friend!","uid":6,"age":50,"city":"北京","province":"北京","country":"中国","address":"中国北京市朝阳区国贸","location":{"lat":"39.918256","lon":"116.467910"}}
{ "index" : { "_index" : "twitter", "_id": 6} }
{"user":"虹桥-老吴","message":"好友来了都今天我生日，好友来了,什么 birthday happy 就成!","uid":7,"age":90,"city":"上海","province":"上海","country":"中国","address":"中国上海市闵行区","location":{"lat":"31.175927","lon":"121.383328"}}


POST _bulk
{ "delete" : { "_index" : "twitter", "_id": 1 }}

GET twitter/_doc/1

POST _bulk
{ "update" : { "_index" : "twitter", "_id": 2 }}
{"doc": { "city": "长沙"}}

GET twitter/_doc/2

GET twitter/_search?size=2&from=2

GET twitter/_search
{
  "size": 2,
  "from": 2, 
  "query": {
    "match_all": {}
  }
}

GET twitter/_settings


GET twitter/_mapping

DELETE twitter
PUT twitter
{
  "settings": {
    "number_of_shards": 1,
    "number_of_replicas": 1
  }
}

PUT twitter/_mapping
{
  "properties": {
    "address": {
      "type": "text",
      "fields": {
        "keyword": {
          "type": "keyword",
          "ignore_above": 256
        }
      }
    },
    "age": {
      "type": "long"
    },
    "city": {
      "type": "text",
      "fields": {
        "keyword": {
          "type": "keyword",
          "ignore_above": 256
        }
      }
    },
    "country": {
      "type": "text",
      "fields": {
        "keyword": {
          "type": "keyword",
          "ignore_above": 256
        }
      }
    },
    "location": {
      "type": "geo_point"
    },
    "message": {
      "type": "text",
      "fields": {
        "keyword": {
          "type": "keyword",
          "ignore_above": 256
        }
      }
    },
    "province": {
      "type": "text",
      "fields": {
        "keyword": {
          "type": "keyword",
          "ignore_above": 256
        }
      }
    },
    "uid": {
      "type": "long"
    },
    "user": {
      "type": "text",
      "fields": {
        "keyword": {
          "type": "keyword",
          "ignore_above": 256
        }
      }
    }
  }
}

GET twitter/_doc/1

GET twitter/_search
{
  "query": {
    "match": {
      "city": "北京"
    }
  }
}

GET twitter/_search
{
  "query": {
    "bool": {
      "filter": {
        "term": {
          "city.keyword": "北京"
        }
      }
    }
  }
}

GET twitter/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "city": "北京"
          }
        },
        {
          "match": {
            "age": "30"
          }
        }
      ]
    }
  }
}

GET twitter/_search
{
  "query": {
    "bool": {
      "must_not": [
        {
          "match": {
            "city": "北京"
          }
        }
      ]
    }
  }
}

GET twitter/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "address": "北京"
          }
        }
      ]
    }
  },
  "post_filter": {
    "geo_distance": {
      "distance": "3km",
      "location": {
        "lat": 39.920086,
        "lon": 116.454182
      }
    }
  }
}

GET twitter/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "address": "北京"
          }
        }
      ]
    }
  },
  "post_filter": {
    "geo_distance": {
      "distance": "5km",
      "location": {
        "lat": 39.920086,
        "lon": 116.454182
      }
    }
  },
  "sort": [
    {
      "_geo_distance": {
        "location": "39.920086,116.454182",
        "order": "asc",
        "unit": "km"
      }
    }
  ]
}

GET twitter/_search
{
  "query": {
    "range": {
      "age": {
        "gte": 30,
        "lte": 40
      }
    }
  }
}

GET twitter/_search
{
  "query": {
    "range": {
      "age": {
        "gte": 30,
        "lte": 40
      }
    }
  },"sort": [
    {
      "age": {
        "order": "desc"
      }
    }
  ]
}

GET twitter/_search
{
  "query": {
    "match": {
      "message": "happy birthday"
    }
  }
}

PUT twitter/_doc/5
{
  "user": "朝阳区-老王",
  "message": "Happy Good BirthDay My Friend!",
  "uid": 6,
  "age": 50,
  "city": "北京",
  "province": "北京",
  "country": "中国",
  "address": "中国北京市朝阳区国贸",
  "location": {
    "lat": "39.918256",
    "lon": "116.467910"
  }
}

GET twitter/_search
{
  "query": {
    "match_phrase": {
      "message": {
        "query": "Happy birthday",
        "slop": 1
      }
    }
  },
  "highlight": {
    "fields": {
      "message": {}
    }
  }
}


GET twitter/_search
{
  "size": 0, 
  "aggs": {
    "age": {
      "range": {
        "field": "age",
        "ranges": [
          {
            "from": 20,
            "to": 30
          },{
            "from": 30,
            "to": 40
          },{
            "from": 40,
            "to": 50
          }
        ]
      }
    }
  }
}

GET twitter/_search
{
  "size": 0,
  "aggs": {
    "average_age": {
      "avg": {
        "field": "age"
      }
    }
  }
}

GET twitter/_search
{
  "size": 0,
  "aggs": {
    "age_stats": {
      "stats": {
        "field": "age"
      }
    }
  }
}

GET twitter/_search
{
  "size": 0,
  "aggs": {
    "age_max": {
      "max": {
        "field": "age"
      }
    }
  }
}

GET twitter/_search
{
  "size": 0,
  "aggs": {
    "average_age_1.5": {
      "avg": {
        "field": "age",
        "script": {
          "source": "_value * params.correction",
          "params": {
            "correction": 1.5
          }
        }
      }
    }
  }
}

GET twitter/_search
{
  "size": 0,
  "aggs": {
    "average_2_times_age": {
      "avg": {
        "script": {
          "source": "doc['age'].value * params.times",
          "params": {
            "times": 2.0
          }
        }
      }
    }
  }
}

GET twitter/_search

DELETE twitter/_doc/1

PUT twitter/_doc/2?_ttl=1d
{
  "user": "东城区-老刘",
  "message": "出发，下一站云南！",
  "uid": 3,
  "age": 30,
  "city": "北京",
  "province": "北京",
  "country": "中国",
  "address": "中国北京市东城区台基厂三条3号",
  "location": {
    "lat": "39.904313",
    "lon": "116.412754"
  }
}

GET twitter/_search
{
  "size": 1,
  "aggs": {
    "age": {
      "range": {
        "field": "age",
        "ranges": [
          {
            "from": 20,
            "to": 30
          },
          {
            "from": 30,
            "to": 40
          },
          {
            "from": 40,
            "to": 50
          }
        ]
      }
    }
  }
}

GET twitter/_search
{
  "query": {
    "match": {
      "message": "happy birthday"
    }
  },
  "size": 0,
  "aggs": {
    "city": {
      "terms": {
        "field": "city.keyword",
        "size": 10
      }
    }
  }
}


GET twitter/_analyze
{
  "text": [
    "生日快乐"
  ],
  "analyzer": "standard"
}

GET twitter/_analyze
{
  "text": [
    "Happy.Birthday"
  ],
  "analyzer": "simple"
}

GET twitter/_analyze
{
  "text": ["Happy Birthday"],
  "tokenizer": "keyword",
  "filter": ["lowercase"]
}

GET twitter/_search


GET twitter/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "age": "30"
          }
        }
      ],
      "should": [
        {
          "match_phrase": {
            "message": "Happy birthday"
          }
        }
      ]
    }
  }
}

GET twitter/_search
{
  "query": {
    "bool": {
      "should": [
        {
          "match_phrase": {
            "message": "Happy birthday"
          }
        }
      ]
    }
  }
}







