# beats-compile

## Compile for non-supported architecture (arm6, arm7, arm8, arm64, armhf)

1. Clear all existing folder
```
rm -rf go
rm -rf go*linux*.tar.gz
rm -rf go_projects
``` 

2. Download latest go of supported architecture
(eg. tested arm6 on armhf)
```
wget https://dl.google.com/go/go1.16.6.linux-arm64.tar.gz
tar -C /usr/local go1.17.5.linux-armv6l.tar.gz
```

3. Configure GO
```
vi ~/.profile
PATH=$PATH:/usr/local/go/bin
GOPATH=$HOME/go
source ~/.profile
```

4. Git clone the Beats 
```
git clone https://github.com/elastic/beats.git

mkdir -p go_projects/src/github.com/elastic
mv beats/ go_projects/src/github.com/elastic/
cd go_projects/src/github.com/elastic/beats/
```

5. Checkout latest version
```
git checkout v7.13.4
```

6. Compile 
```
cd heartbeat/
GOOS=linux GOARCH=arm GOARM=7 go get
```

7. Copy to the beats folder and beats binary to folder
8. Change permission for yml
```
mkdir ~/beats
cp -R /root/go_projects/src/github.com/elastic/beats/heartbeat ~/beats/
cp /root/go/bin/heartbeat ~/beats/heartbeat/
chmod go-w ~/beats/heartbeat/heartbeat.yml
```

9. (Optional) Create systemd unit
```
vi /etc/systemd/system/heartbeat.service
[Unit]
Description=Elasticsearch Heartbeat Agent
After=network.online.target
Wants=network.online.target

[Service]
Type=simple
User=root
ExecStart=/root/beats/heartbeat/heartbeat -c /root/beats/heartbeat/heartbeat.yml

[Install]
WantedBy=multi-user.target

systemctl enable heartbeat.service
systemctl restart heartbeat.service
systemctl status heartbeat.service
```
