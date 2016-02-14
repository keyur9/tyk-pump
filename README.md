# Tyk Pump

Tyk Pump is a pluggable analytics purger to move Analytics generated by your Tyk nodes to any back-end. 

## Back ends currently supported: 

- MongoDB (to replace built-in purging)
- CSV (updated, now supports all fields)
- ElasticSearch (2.0+)

## Configuration:

Create a `pump.conf` file:

```
{
	"analytics_storage_type": "redis",
    "analytics_storage_config": {
        "type": "redis",
        "host": "localhost",
        "port": 6379,
        "hosts": null,
        "username": "",
        "password": "",
        "database": 0,
        "optimisation_max_idle": 100,
        "optimisation_max_active": 0,
        "enable_cluster": false
    },
    "purge_delay": 10,
    "pumps": {
    	"dummy": {
    		"name": "dummy",
    		"meta": {}
    	},
        "mongo": {
            "name": "mongo",
            "meta": {
                "collection_name": "tyk_analytics",
                "mongo_url": "mongodb://username:password@{hostname:port},{hostname:port}/{db_name}"
            }
        },
        "csv": {
            "name": "csv",
            "meta": {
                "csv_dir": "./"
            }
        },
        "elasticsearch": {
            "name": "elasticsearch",
            "meta": {
                "index_name": "tyk_analytics",
                "elasticsearch_url": "localhost:9200",
                "enable_sniffing": false,
                "document_type": "tyk_analytics",
                "rolling_index": false
            }
        }
    },
    "uptime_pump_config": {
        "collection_name": "tyk_uptime_analytics",
        "mongo_url": "mongodb://username:password@{hostname:port},{hostname:port}/{db_name}"
    },
    "dont_purge_uptime_data": false
}
```

Settings are the same as for the original `tyk.conf` for redis and for mongoDB.

### Elasticsearch Config

`"index_name"` - The name of the index that all the analytics data will be placed in. Defaults to "tyk_analytics"

`"elasticsearch_url"` - If sniffing is disabled, the URL that all data will be sent to. Defaults to "http://localhost:9200"

`"enable_sniffing"` - If sniffing is enabled, the "elasticsearch_url" will be used to make a request to get a list of all the nodes in the cluster, the returned addresses will then be used. Defaults to false

`"document_type"` - The type of the document that is created in ES. Defaults to "tyk_analytics"

`"rolling_index"` - Appends the date to the end of the index name, so each days data is split into a different index name. E.g. tyk_analytics-2016.02.28 Defaults to false