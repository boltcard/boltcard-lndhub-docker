# Bolt Card Hub docker container

### install Docker

- [Docker engine download &
   install](https://docs.docker.com/engine/install/)
   
### pre setup
```
$ git clone https://github.com/boltcard/boltcard-lndhub-docker bolthub
$ cd bolthub
$ git clone https://github.com/boltcard/boltcard-lnhhub-transferred.git BoltCardHub
$ git clone https://github.com/boltcard/boltcard.git
```
- Rename the .env.example file to .env and change the variable values
- Edit `Caddyfile` change the two domain names to your bolt card hub domain
- Edit `settings.sql` [Bolt card system settings](https://github.com/boltcard/boltcard/blob/main/docs/SETTINGS.md)
- Add `admin.macaroon` and `tls.cert` files in the /bolthub/ project directory

### service bring-up and running
```
$ sudo groupadd docker
$ sudo usermod -aG docker ${USER}
(log out & in again)
$ docker volume create caddy_data
$ docker volume create boltcard_hub_lnd
// add -d option for detached mode
$ docker compose up
```
- Go to `https://your.domain.com:8080` to access your Bolt Card Hub GUI 
- Either scan the QR from yout Bolt Card Hub GUI in the top right or
- enter the same URL into your Bolt Card Wallet to connect to your Bolt Card Hub
