# Bolt Card Hub docker container

There is a walkthough video on the [Bolt Card youtube channel](https://www.youtube.com/@boltcard)  

### install docker

- [Docker engine download & install](https://docs.docker.com/engine/install/)
   
### pre setup
```
$ git clone https://github.com/boltcard/boltcard-lndhub-docker bolthub
$ cd bolthub
$ git clone --depth 1 --branch v0.2.0 https://github.com/boltcard/boltcard-lndhub BoltCardHub
$ git clone --depth 1 --branch v0.4.0 https://github.com/boltcard/boltcard.git
$ git clone https://github.com/boltcard/boltcard-groundcontrol.git GroundControl
```
- Rename the .env.example file to .env and change the variable values
- Edit `Caddyfile` change the two domain names to your bolt card hub domain
   - If you are using `docker-compose-groundcontrol.yml`, edit `CaddyfileGroundControl`. Change the three domain names to your bolt card hub domain
- Edit `settings.sql` [Bolt card system settings](https://github.com/boltcard/boltcard/blob/main/docs/SETTINGS.md)
- Add `admin.macaroon` and `tls.cert` files in the /bolthub/ project directory

### service bring-up and running
```
$ sudo groupadd docker
$ sudo usermod -aG docker ${USER}
(log out & in again)
$ docker volume create caddy_data
$ docker volume create boltcard_hub_lnd
$ docker volume create boltcard_redis
$ cd bolthub
// add -d option for detached mode
$ docker compose up
```
- Go to `https://your.domain.com:8080` to access your Bolt Card Hub GUI 
- Either scan the QR from your Bolt Card Hub GUI in the top right or
- enter the same URL into your Bolt Card Wallet to connect to your Bolt Card Hub


#### running your own groundcontrol at the same time
- If you want to run your own ground control server, use `docker-compose-groundcontrol.yml`. `docker compose -f docker-compose-groundcontrol.yml up`
- Make sure to change the ground control url in the app as well.



### Upgrading bolt hub and bolt service to the latest version

```
$ cd bolthub
$ cd BoltCardHub
$ git fetch origin tag v0.2.0 --no-tags
$ git checkout v0.2.0
$ cd ../boltcard
$ git fetch origin tag v0.4.0 --no-tags
$ git checkout v0.4.0
$ cd ../
//this will take down your containers so make sure you are not using the hub
$ docker compose down
//removing container images to build a new ones
//your image names might be different.
//you can check them by running docker images.
//delete the ones with *-boltcard and *-web at the end.
$ docker image rm bolthub-boltcard
$ docker image rm bolthub-web
$ docker compose up -d
```


#### When upgrading to boltcard service v0.4.0 and boltcard hub v0.2.0
If you are upgrading your boltcard service container to enable PIN functionality you have to run the commands below to update the database table

```
$ docker exec -it boltcard_db /bin/sh
$ psql -d card_db -U cardapp
$ ALTER TABLE cards ADD COLUMN pin_enable CHAR(1) NOT NULL DEFAULT 'N';
$ ALTER TABLE cards ADD COLUMN pin_number CHAR(4) NOT NULL DEFAULT '0000';
$ ALTER TABLE cards ADD COLUMN pin_limit_sats INT NOT NULL DEFAULT 0;
```


### reference documents

[Ground Control](https://github.com/BlueWallet/GroundControl)
