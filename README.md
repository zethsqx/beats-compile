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
tar -xzf go1.16.6.linux-arm64.tar.gz
```

3. Git clone the Beats 
4. Checkout latest version
```
git clone https://github.com/elastic/beats.git
mkdir go_projects/src/github.com/elastic -p
mv beats/ go_projects/src/github.com/elastic/
cd go_projects/src/github.com/elastic/
cd beats/
git checkout v7.13.4
```

4. Compile 
```
cd heartbeat/
GOOS=linux GOARCH=arm GOARM=7 go get
```

5. Copy to folder and use
```
mkdir ~/beats
cp -R $GOPATH/src/github.com/elastic/beats/heartbeat ~/beats/heartbeat
cp $GOPATH/bin/heartbeat ~/beats/heartbeat
chmod go-w ~/beats/heartbeat/heartbeat.yml
```
