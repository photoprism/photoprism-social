# Docker Hosting Environment for Mastodon

## Setup Instructions

```sh
# 1. Fill out mastodon.prod.env
# 2. Increase system mmap count for better elastic search performance.
echo "vm.max_map_count=262144" | sudo tee /etc/sysctl.d/90-max_map_count.conf
sudo sysctl --system
# 3. Create the directories that need special permissions.
sudo mkdir -p /opt/mastodon/database/elasticsearch
sudo mkdir -p /opt/mastodon/web/system
sudo chown 1000 /opt/mastodon/database/elasticsearch
sudo chown 991:991 /opt/mastodon/web/system
# 4.
sudo docker compose up -d
# 5.
sudo docker compose -f /opt/mastodon/docker-compose.yml run --rm tootctl bundle exec rake db:setup
# 6.
sudo docker compose -f /opt/mastodon/docker-compose.yml run --rm tootctl bundle exec rake db:migrate

# Need to attach to the web container for these last few.
sudo docker exec -it mastodon-web /bin/bash
bin/tootctl accounts create jahanson --email joe@veri.dev --confirmed --role Owner
bin/tootctl settings registrations close

```
