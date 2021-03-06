# maps.bikeottawa.ca-backend
Server side for maps.bikeottawa.ca running OSRM and isochrone servers. Tested with Node v6

## Installation
Clone and install dependencies: 
```
git clone https://github.com/zzptichka/maps.bikeottawa.ca-backend
cd maps.bikeottawa.ca-backend/
npm install
```

## Setup
- Copy LTS OSRM files generated by backend.bikeottawa.ca to data directory. You should have 4 directories there: lts1, lts2, lts3, lts4. Scripts on backend.bikeottawa.ca can do that automatically
- Open port 4000 on your firewall for isochrone server
- Configure your web server to forward requests based on lts# in url to corresponding OSRM server port(lts1->5001, lts2->5002, lts3->5003, lts4->5004). 
Your Nginx config could look like this:
```
server {
        listen 80 default_server;
        listen [::]:80 default_server ipv6only=on;
        root /var/www/html
        # catch all v1 routes and send them to OSRM
        location /route/v1/lts1   { proxy_pass http://localhost:5001; }
        location /route/v1/lts2   { proxy_pass http://localhost:5002; }
        location /route/v1/lts3   { proxy_pass http://localhost:5003; }
        location /route/v1/lts4   { proxy_pass http://localhost:5004; }
}
```


## Start
```
./start-osrm-server.sh
node iso-server.js
```
