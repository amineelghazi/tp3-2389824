# Services Obligatoire
-Portainer

# Autres services
-jellyfin & Blogverse(TP2), proxy(traefik)

# Commandes
git clone https://github.com/amineelghazi/tp3-2389824.git

cd tp3-2389824/
cp .env.example .env
# modification du .env avec des variables valides (token,email etc..)
nano .env
sudo docker compose up -d
# Verification que les services run
sudo docker ps
# Portainer
Lorsque le service reste ouvert trop longtemps il faut le relancer

sudo docker compose down portainer
sudo docker compose up portainer






