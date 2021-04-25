# Installation
Clone the repo
```
cd /opt
sudo git clone https://github.com/fapgomes/owl2mqtt.git
```
Copy the sample config file, and put your own configurations
```
cd /opt/owl2mqtt/
sudo cp owl2mqtt.conf-sample owl2mqtt.conf
```
Create the system file
```sudo vi /etc/systemd/system/owl2mqtt.service```
And add the following to this file:
```
[Unit]
Description=owl2mqtt
After=network.target

[Service]
ExecStart=/usr/bin/python3 /opt/owl2mqtt/owl.py
WorkingDirectory=/opt/owl2mqtt
StandardOutput=inherit
StandardError=inherit
Restart=always
User=openhab

[Install]
WantedBy=multi-user.target
```
Reload systemd daemon
```
sudo systemctl daemon-reload
```
Start the service
```
sudo systemctl start owl2mqtt
```
# openhab mqtt config example
owl.things
```
Bridge mqtt:broker:rabbitmq "MQTT Broker: RabbitMQ"
[
    host="localhost",
    port=1883,
    secure="AUTO",
    username="openhab",
    password="testpassword"
]
{
    // owl2mqtt
    Thing topic owl2mqtt "OWL" @ "Piso 1" {
    Channels:
        Type datetime   : timestamp             "OWL Timestamp"                 [ stateTopic="owl/electricity/timestamp" ]
        Type number     : battery               "OWL Battery"                   [ stateTopic="owl/electricity/battery" ]
        Type number     : rssi                  "OWL RSSI"                      [ stateTopic="owl/electricity/rssi" ]
        Type number     : lqi                   "OWL LQI"                       [ stateTopic="owl/electricity/lqi" ]
        Type number     : channel0              "OWL Channel 0"                 [ stateTopic="owl/electricity/channel0" ]
        Type number     : channel1              "OWL Channel 1"                 [ stateTopic="owl/electricity/channel1" ]
        Type number     : channel2              "OWL Channel 2"                 [ stateTopic="owl/electricity/channel2" ]
        Type number     : daychannel0           "OWL Day Channel 0"             [ stateTopic="owl/electricity/daychannel0" ]
        Type number     : daychannel1           "OWL Day Channel 1"             [ stateTopic="owl/electricity/daychannel1" ]
        Type number     : daychannel2           "OWL Day Channel 2"             [ stateTopic="owl/electricity/daychannel2" ]
    }
 }
 ```
 owl.items
 ```
 Group                   gOwl                        "Owl"       <energy>
Group:Number:SUM        gBombaCalorEnergy           "Consumo Energético Bomba Calor [%d w]"     <bomba_calor>
Group:Number:SUM        gBombaCalorEnergyDay        "Consumo Energético Bomba Calor Dia [%d w]" <bomba_calor>
DateTime                owlTimeStamp                "Time [%1$tY-%1$tm-%1$td  %1$tH:%1$tM]"     <time>                  (gOwl)                      { channel="mqtt:topic:rabbitmq:owl2mqtt:timestamp" }
Number                  owlBattery                  "Battery [%.1f %%]"                         <battery>               (gOwl)                      { channel="mqtt:topic:rabbitmq:owl2mqtt:battery" }
Number                  owlFase1                    "Fase 1 [%.1f w]"                           <energy>                (gOwl,gBombaCalorEnergy)    { channel="mqtt:topic:rabbitmq:owl2mqtt:channel0" }
Number                  owlFase1Day                 "Fase 1 dia [%.1f wh]"                      <energy>                (gOwl,gBombaCalorEnergyDay) { channel="mqtt:topic:rabbitmq:owl2mqtt:daychannel0" }
Number                  owlFase2                    "Fase 2 [%.1f w]"                           <energy>                (gOwl,gBombaCalorEnergy)    { channel="mqtt:topic:rabbitmq:owl2mqtt:channel1" }
Number                  owlFase2Day                 "Fase 2 dia [%.1f wh]"                      <energy>                (gOwl,gBombaCalorEnergyDay) { channel="mqtt:topic:rabbitmq:owl2mqtt:daychannel1" }
Number                  owlFase3                    "Fase 3 [%.1f w]"                           <energy>                (gOwl,gBombaCalorEnergy)    { channel="mqtt:topic:rabbitmq:owl2mqtt:channel2" }
Number                  owlFase3Day                 "Fase 3 dia [%.1f wh]"                      <energy>                (gOwl,gBombaCalorEnergyDay) { channel="mqtt:topic:rabbitmq:owl2mqtt:daychannel2" }
Number                  owlrssi                     "RSSI [%d]"                                 <qualityofservice>      (gOwl)                      { channel="mqtt:topic:rabbitmq:owl2mqtt:rssi" }
Number                  owllqi                      "lqi [%d]"                                  <qualityofservice>      (gOwl)                      { channel="mqtt:topic:rabbitmq:owl2mqtt:lqi" }
```
![Screenshot_20210323_160727](https://user-images.githubusercontent.com/39247306/112178710-e5941b00-8bf1-11eb-8791-71f7d7615a22.png)  
