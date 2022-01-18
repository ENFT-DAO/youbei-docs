AWS Resources:

*************************************************************************************************************
postgresql-cluster

dbmasteruser
4ORoMtqF7hR`5Lw^pi6{R6O1DM#{DRD9

ip endpoint: ls-1fbc6e4229157c0e4f9efb07b95a591984a5a936.cyui9j4mutm1.us-east-2.rds.amazonaws.com:5432


*************************************************************************************************************
elrond-observer - This is the server for the elrond node

Probably use this next time https://github.com/ElrondNetwork/elrond-go-scripts-mainnet


Public IP: 3.142.241.4  
Private IP: 172.26.0.248

Install go:
wget -c https://dl.google.com/go/go1.17.6.linux-amd64.tar.gz -O - | sudo tar -xz -C /usr/local
Add go path to global server profile
sudo nano /etc/profile
export PATH=$PATH:/usr/local/go/bin

Install gcc Build Tools
sudo apt-get install build-essential

Install elrond-go
cd ~/
git clone https://github.com/ElrondNetwork/elrond-go
cd elrond-go && git checkout master
GO111MODULE=on go mod vendor
cd cmd/node && go build
cd ~/elrond-go/cmd/keygenerator
go build
./keygenerator    
Pem key is in ~/elrond-go/cmd/keygenerator 
Binary (node) is in /home/elrond-go/cmd/node - yes it is /home/elrond-go/cmd/node/node





Configure your observer: 

https://docs.elrond.com/validators/

Configs download:

https://github.com/ElrondNetwork/elrond-config-devnet
https://github.com/ElrondNetwork/elrond-config-testnet
https://github.com/ElrondNetwork/elrond-config-mainnet

Create a folder with the current version of the validator https://devnet-explorer.elrond.com/nodes

///devnet///
mkdir ~/validator-D1.2.38.0
mkdir ~/validator-D1.2.38.0/config

cp ~/elrond-go/cmd/node/node ~/validator-D1.2.38.0/node
cp ~/elrond-go/cmd/keygenerator/validatorKey.pem ~/validator-D1.2.38.0/config
cp -r ~/elrond-config-devnet/* ~/validator-D1.2.38.0/config
///devnet///

///mainnet///
mkdir ~/mainnet-observer
mkdir ~/mainnet-observer/config

cp ~/elrond-go/cmd/node/node ~/mainnet-observer/node
cp ~/elrond-go/cmd/keygenerator/validatorKey.pem ~/mainnet-observer/config
cp -r ~/elrond-config-mainnet/* ~/mainnet-observer/config
///mainnet///

Edit the configs

a few things worth mentioning for observer setup:
- correct destination shard (if scs are on shard 1 the observer should target that shard) 

https://github.com/ElrondNetwork/elrond-config-mainnet/blob/


/config/prefs.toml

- enabling the notifier driver so that events are dispatched -> https://github.com/ElrondNetwork/elrond-config-mainnet/blob/master/external.toml#L18-L34
- the auth between node process[notifier] -> api can be left as disabled if it's being routed on localhost 

DestinationShardAsObserver = "metachain"
FullArchive = true


config/external.toml

[ElasticSearchConnector]
    Enabled           = true 

[EventNotifierConnector]
    Enabled = true 
    ProxyUrl = "http://172.26.6.231:5000"




Create the validator service

sudo echo "[Unit]
  Description=Elrond Observer
  After=network-online.target
  
  [Service]
  User=$USER

  WorkingDirectory=/home/ubuntu/mainnet-observer
  ExecStart=/home/ubuntu/mainnet-observer/node "--log-level=*:DEBUG"
  StandardOutput=journal
  StandardError=journal
  Restart=always
  RestartSec=3
  LimitNOFILE=65535
  
  [Install]
  WantedBy=multi-user.target" > elrond-observer.service

sudo mv elrond-observer.service /etc/systemd/system/
sudo systemctl enable elrond-observer.service
sudo systemctl start elrond-observer.service

*************************************************************************************************************


*************************************************************************************************************

web-services 

This installs API, NGINX, REDIS

Gitgub Access Token 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 

sudo apt-get install redis-server nginx

NGINX:

sudo nano /etc/nginx/nginx.conf

nginx.cong : http :

limit_req_zone $binary_remote_addr zone=ip:10m rate=3r/s;

sudo nano /etc/nginx/sites-available/default 

server {
        listen 80;
                listen [::]:80;

                server_name dev.doshswap.com;

        root /home/ubuntu/youbei-webapp/build;
                index index.html;

                location / {
                        try_files $uri $uri/ =404;
                }
}

	
server {
listen 80;
        	location / {
            	limit_req zone=ip burst=6 delay=4;
            	proxy_pass http://localhost:5000;
}
    
? sudo ufw allow proto tcp from	10.5.96.3 port 5432 to 10.5.96.3 port 5432
 
sudo nano /etc/sysctl.conf
fs.inotify.max_user_watches=524288
sudo sysctl -p

curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash -
sudo apt install -y nodejs
sudo npm i -g node-sass typescript

Install Webapp

git clone https://github.com/shawnbure/youbei-webapp.git

nano ~/youbei-webapp/src/webapp/src/configs/dapConfig.tsx
nano ~/youbei-webapp/src/constants/api.ts BASE_URL_API: string = 'https://erdsea.com/';

npm i
npm run build

“Test Run” is npm start

Install API

Install go:
wget -c https://dl.google.com/go/go1.17.6.linux-amd64.tar.gz -O - | sudo tar -xz -C /usr/local
Add go path to global server profile
sudo nano /etc/profile
export PATH=$PATH:/usr/local/go/bin

Install gcc Build Tools
sudo apt-get install build-essential
git clone https://github.com/shawnbure/youbei-api.git

Add PEM File from local host

scp dev-wallet-owner.pem ubuntu@3.145.15.251:/home/ubuntu/youbei-api/config

api - config.toml - This controls the address and port of the api

[ConnectorApi]
    Address = "localhost:5000"

sudo ufw allow in on eth0 from 172.26.0.248 to 172.26.6.231 proto tcp port 5000
sudo ufw allow ssh

sudo ufw allow http
sudo ufw enable
    

Build go
 go build cmd/main.go






Create the API Service

sudo echo "[Unit]
  Description=Youbei API
  After=network-online.target
  
  [Service]
  User=ubuntu

  WorkingDirectory=/home/ubuntu/youbei-api
  ExecStart=/home/ubuntu/youbei-api/main "--log-level=*:DEBUG"
  StandardOutput=journal
  StandardError=journal
  Restart=always
  RestartSec=3
  LimitNOFILE=65535
  
  [Install]
  WantedBy=multi-user.target" > youbei-api.service

sudo mv youbei-api.service /etc/systemd/system/
sudo systemctl enable youbei-api.service
sudo systemctl start youbei-api.service






Deploy Contract - 

erd1qqqqqqqqqqqqqpgqhz4g5t6n7qkdupykngtp69ynw7ghcccry4wsx6423a

Look up Shard:

 few things worth mentioning for observer setup:
- correct destination shard (if scs are on shard 1 the observer should target that shard) 

https://github.com/ElrondNetwork/elrond-config-mainnet/blob/master/prefs.toml#L5
- enabling the notifier driver so that events are dispatched -> 

https://github.com/ElrondNetwork/elrond-config-mainnet/blob/master/external.toml#L18-L34
- the auth between node process[notifier] -> api can be left as disabled if it's being routed on 

cp /home/erdsea-api/config/config-example.toml /home/erdsea-api/config/config.toml 
Copy wallet pem file into /home/erdsea-api/config

Change contract address, deployer address and database specifics, pem, proxy, chain id

Make erdsea_db, give permissions to user


