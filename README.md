# EKGC-F Logging Stack

Components Used: 
* Elasticsearch (6.8)
* Kibana (6.8)
* Graylog (3.1.3)
* Curator (5.8)
* Filebeat (6.8)
* NGINX

## Elasticsearch Configuration

```diff
- Graylog will only support ES version 6.8.x with graylog 3.x.x
```

You will need plugins in elasticsearch to work with snapshots. Dockerfiles are in elasticsearch/config folder for some cloud vendors you can build and use that image to have that plugin installed you can install more plugins as you required like this.

You can also use filesystem path as snapshot storage that does not require additional plugin. attach a data disk to VM and change that path into `path.repo` variable. you can hit this api changing location in ES or kibana dev tools to create snapshot storage on filesystem 

```diff
+ make sure filesystem path is writable by elasticsearch user otherwise you will face errors
```

```
PUT /_snapshot/my_fs_backup
{
    "type": "fs",
    "settings": {
        "location": "/mnt/backup/snapshot",
        "compress": true
    }
}
```

## Graylog Configuration

Graylog UI is avilable at `http://IP:9000`
Launch a new input in graylog with selecting Beats and port 5044 as inbound. make sure firewall allows port 5044 and it is also exposed in docker-compose.yaml.

You can add basic extractors to parse JSON or remove sensetive data from logs with the help of regex and many more. You can also add pipelines to filter more logs remove unncessary fields clean some logs filter out messages and many more.

This way we can launch many inputs and based on fileds we can divert it into diffrent index with stream rules make sure to check index rotation settings as per your requirements

## Filebeat Configuration

On you log originating machine copy this two files and change graylog ip in filebeat.docker.yml and run `sudo bash command`

this will create a filebeat container which will harvest docker container logs and send it to graylog for processing and searching. You can add other metadata into logs to diffrentaite logs based on fields.

## Curator

In Curator configuration make sure you type correct ES snapshot repository name and also change index snapshot and deletion inteval as per your need.

we can add multiple index to run rule against, in pattern regex





