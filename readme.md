# Configuring Splunk tile
Download the tile from Ecsteam aws bucket `s3://pcf-custom-tiles/splunk-ecs-1.0.0.pivotal` and upload it to ops manager.  


### Firehose Properties
Create firehose user in PCF UAA

```
//spunk user configuration
$ uaac target https://uaa.<SYSTEM_DOMAIN>
$ uaac token client get admin -s <ADMIN_SECRET> (UAA -> Admin Client Credentials)
$ uaac -t user add splunk.cf.user --password < RANDOM_PWD > --emails <EMAIL_ID>
$ uaac -t member add cloud_controller.admin splunk.cf.user
$ uaac -t member add doppler.firehose splunk.cf.user
```  

Configure splunk tile

```
cf_api_endpoint: https://api.<SYSTEM_DOMAIN>  
cf_api_user: splunk.cf.user  
cf_api_password: <RANDOM_PWD>  
firehose_subscription_id: splunk-firehose-subscription <subscription id used to tap into firehose>
splunk_token: <SPLUNK_TOKEN> - Get it from splunk team
splunk_index: <SPLUNK_INDEX> - Get it from splunk team
splunk_servers: <SPLUNK_SERVERS> - get it from splunk team
splunk_ssl: <if checked use ssl to communicate with splunk servers>
splunk_ssl_password: Get it from splunk team
splunk_ssl_root_ca: Get it from splunk team
splunk_ssl_common_name: Get it from splunk team
```


# Building the tile

This tile is uses code following community repos. By Default, the [splunk-firehose-nozzle](https://github.com/cloudfoundry-community/splunk-firehose-nozzle) 
##### Get the code

Clone https://github.com/cloudfoundry-community/splunk-firehose-nozzle-release  

##### Create bosh realease
Create bosh release and download the release.

```
$ cd splunk-firehose-nozzle-release
$ bosh create release --force --with-tarball
```  
##### Build the tile
1. clone this repo  
2. copy the release tarball from `splunk-firehose-nozzle-release/dev_releases/splunk-firehose-nozzle` directory to `resources` directory. (change the tile.yml to reference the tarball if required
3. `tile build` 
