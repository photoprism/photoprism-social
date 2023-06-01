# photoprism-social

## Mastodon bootstrap
```sh
# 1. Fill out mastodon.prod.env
# 2.
sudo docker compose up -d
# 3.
sudo docker compose -f /opt/mastodon/docker-compose.yml run --rm tootctl bundle exec rake db:setup
# 4.
sudo docker compose -f /opt/mastodon/docker-compose.yml run --rm tootctl bundle exec rake db:migrate

# Need to attach to the web container for these last few.
sudo docker exec -it mastodon-web /bin/bash
bin/tootctl accounts create jahanson --email joe@veri.dev --confirmed --role Owner
bin/tootctl settings registrations close

# Reverse Proxy - Nginx
# Generate yourself a custom dhparam
sudo openssl dhparam -dsaparam -out /etc/ssl/certs/dhparam.pem 4096
# Place the mastodon.nginx.conf in either the nginx /etc/nginx/conf.d folder or available-sites depending on which distro you're on and start nginx up!
```