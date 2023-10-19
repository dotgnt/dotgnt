## Nostr relay

- Run a private relay on your local network to backup all your [Nostr](https://nostr.com/) activity.
- The [nostr-rs-relay](https://github.com/scsibug/nostr-rs-relay) is written in Rust and saves data with SQLite.
- Use service like [Tailscale](https://tailscale.com/) to access your private relay remotely.

**Requirements**

- A system running GNU/Linux
- [Docker](https://docs.docker.com/engine/install/) and [Docker Compose](https://docs.docker.com/compose/install/)
- Nginx Proxy Manager ([NPM](https://github.com/NginxProxyManager/nginx-proxy-manager))

Note: Create the directories wherever you want and edit the files using your favorite editor.

**Docker**

Create a new directory to store data and a `config.toml` configuration file.

```console
$ mkdir -p ~/docker/nostr-rs-relay/data && cd ~/docker/nostr-rs-relay/data
$ touch config.toml
$ nano config.toml
```

Copy, paste and edit the latest [config.toml](https://github.com/scsibug/nostr-rs-relay/blob/master/config.toml) configuration file.

**Docker Compose**

Create a `docker-compose.yml` file.

```console
$ cd ~/docker/nostr-rs-relay
$ touch docker-compose.yml
$ nano docker-compose.yml
```

Copy and paste the following contents.

```yaml
version: "3.7"
services:
  nostr-relay:
    image: scsibug/nostr-rs-relay
    user: 1000:1000
    container_name: nostr-rs-relay
    volumes:
      - /path/to/data/config.toml:/usr/src/app/config.toml
      - /path/to/data:/usr/src/app/db
    ports:
      - 7000:8080
    restart: on-failure
```

Note: Edit the file to match your configuration.

**Deploy the container**

To build and start the container run the `docker-compose` command.

```console
$ cd ~/docker/nostr-rs-relay
$ sudo docker-compose up -d
```

**Nginx Proxy Manager**

Configure Nginx Proxy Manager ([NPM](https://github.com/NginxProxyManager/nginx-proxy-manager)) as a reverse proxy to provide TLS termination.

Note: Use [Pi-hole](https://github.com/pi-hole/pi-hole) as your local DNS server.

**Connect to the relay**

All done! Just connect to the relay using `wss://domain-name.local` from your nostr client.
