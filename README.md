Compose-Services
===

Docker-compose setup for experimental commons, small commons, or local development of the Gen3 stack. Production use should use [cloud-automation](https://github.com/uc-cdis/cloud-automation).

This setup uses Docker containers for the [Gen3 microservices](https://github.com/uc-cdis/) and nginx. The microservices and nginx images are pulled from quay.io (master), while Postgres (9.5) images are pulled from Docker Hub. Nginx is used as a reverse proxy to each of the services. 

In the following pages you will find information about [migrating existing](docs/release_history.md) and [setting up](docs/setup.md) new compose services, [dev tips](docs/dev_tips.md), basic information about [using the data commons](docs/using_the_commons.md), and [useful links](docs/useful_links.md) contributed by our community. 

You can quickly find commonly used commands in our [cheat sheet](./docs/cheat_sheet.md). Config file formats were copied from [cloud-automation](https://github.com/uc-cdis/cloud-automation) and stored in the `Secrets` directory and modified for local use with Docker Compose. Setup scripts for some of the containers are kept in the `scripts` directory.


# Key Documentation

* [Database Information](docs/database_information.md)
* [Release History and Migration Instructions](docs/release_history.md)
* [Setup](docs/setup.md)
* [Dev Tips](docs/dev_tips.md)
* [Using the Data Commons](docs/using_the_commons.md)
* [Useful links](docs/useful_links.md)


DEPLOY ON EC2
https://stackoverflow.com/questions/35868976/nginx-job-for-nginx-service-failed-because-the-control-process-exited/58332311
https://certbot.eff.org/lets-encrypt/ubuntufocal-nginx

Go to https://certbot.eff.org/lets-encrypt/ubuntubionic-nginx, and follow the instructions to install certbot
make sure port 80 is open, docker isn’t running portal/revproxy, and run sudo certbot certonly --nginx and that a local not docker nginx instance is running
copy the files created, most likely something like

1)open firewall

if renewing:
Run `docker stop revproxy-service` and run `sudo certbot renew`

# Certificate is saved at: /etc/letsencrypt/live/gearbox-dev.pedscommons.org/fullchain.pem
# Key is saved at:         /etc/letsencrypt/live/gearbox-dev.pedscommons.org/privkey.pem
sudo cp /etc/letsencrypt/live/gearbox-dev.pedscommons.org/privkey.pem ~/compose-services/Secrets/TLS/service.key
sudo cp /etc/letsencrypt/live/gearbox-dev.pedscommons.org/fullchain.pem ~/compose-services/Secrets/TLS/service.crt

maybe sudo systemctl stop nginx.service
sudo lsof -i -P -n | grep 80 and kill
sudo kill -9
docker-compose up -d