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
