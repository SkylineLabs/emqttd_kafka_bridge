
emqttd_kafka_plugin
===================

This is a plugin for the EMQ broker that sends all messages received by the broker to kafka.

Build the EMQ broker
-------------

1. clone emq-relx project
```	
git clone https://github.com/emqtt/emq-relx.git
```
2. Add DEPS of the plugin in the Makefile
```
DEPS += emqttd_kafka_bridge
dep_emqttd_kafka_bridge = git https://github.com/SkylineLabs/emqttd_kafka_bridge.git master
```
3. Add load plugin in relx.config
```
{emqttd_kafka_bridge, load},
 ```
4. Build
```
cd emq-relx && make
```  
Configuration
----------------------
You will have to edit the configurations of the bridge to set the kafka Ip address and port.

Edit the file emq-relx/deps/emqttd_kafka_bridge/etc/emqttd_kafka_bridge.config
```
[
  {emqttd_kafka_bridge, [{values, [
	  %%edit this to address and port on which kafka is running
      {bootstrap_broker, {"172.31.29.115", 9092} },
	  %% partition strategies can be strict_round_robin or random
      {partition_strategy, strict_round_robin},
      %% Change the topic to produce to kafka. Default topic is "Kafka". It is on this topic that the messages will be sent from the broker to a kafka consumer
	  {kafka_producer_topic, <<"kafka">>}
    ]}]}
].
```

Start the EMQ broker and load the plugin 
-----------------
1) cd emq-relx/_rel/emqttd
2) ./bin/emqttd start
3) ./bin/emqttd_ctl plugins load emqttd_kafka_bridge

Test
-----------------
Send an MQTT message on a random topic from an MQTT client to you EMQ broker.

The following should be received by your kafka consumer :

  {"topic":"yourtopic", "message":[yourmessage]}
This is the format in which kafka will receive the MQTT messages
  
License
-------

Apache License Version 2.0


**This project is a sub-module of ConnectX IoT platform.**
**Refer https://github.com/SkylineLabs/ConnectX-IoT-platform for more details**

