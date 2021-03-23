# Installation
Clone the repo
```
git clone https://github.com/fapgomes/owl2mqtt.git
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
