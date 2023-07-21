WIS2-GlobalCache-Monitoring
============================

The code in this repository is to provide an example of a WIS2 Sensor Centre for Global Cache Monitoring

## What is here ?

This is the source code of the Sensor Center to monitor the WIS2 Global Cache.
It runs using docker and relies on two containers running on the same bridged network.
The containers are:
1. Node-Red for all the processing part
2. Redis to store in memory (and saved on disk) key/value pairs 

All the required source is available in flow.json and al.

## What does it do ?

1. Listen to the topic (see MQTT_TOPIC env variable - `cache/a/wis2`or `origin/a/wis2` will be added in front of the topic - ) from Global Caches (at the moment, DWD and JMA) and Global Broker
2. Using `data_id` as the key compute the time difference between the message annoucing the availability of the data from the origin (the data producer) and the caches. 
3. Four different thresholds (time in seconds) are defined in the `Config` node using the DEALY env variables. Depending on the delay between `orig` and `cache` the matching label in the metrics will be used.
4. It then provides prometheus metrics available at http://@IP:1880/metrics (This URL is not password protected. It can be protected, eg. using traefik to proxy NodeRed and add authentication and secured (HTTPS) access.)

## How to start ?

1. Download 
- a. docker-compose.yml
- b. flow.json, settings.js, flow_cred.json, package.json
2. Create a directory within your $HOME eg. WIS2GlobalCacheMonitoring
3. In this directory create additional directories: data, redis
4. Copy the files from b. above in the data directory
5. Check the docker-compose.yml to make sure that the ports exposed are available on your system. Adjust the volume accordingly.
6. Configure all ENV variable required (Connection to MQTT brokers, delays,..) in the compose file
7. Start the docker stack using docker-compose.yml with `docker compose up -d`
   
You can connect to the system where you started the container using http://@IP_Address:1880. It is possible to update the flows to tweak this to your needs. Eg. logging on file the messages for data not cached... 
The node `flogger` is installed but not currently used. It can be configured to log into the 
This web interface is not password protected. It can be protected, eg. using traefik to proxy NodeRed and add authentication and secured (HTTPS) access.

