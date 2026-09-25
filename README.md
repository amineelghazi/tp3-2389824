# TP3 – 2389824

Infrastructure de services conteneurisés déployée avec Docker Compose, orchestrée derrière un reverse proxy Traefik avec HTTPS automatique (Let's Encrypt via DuckDNS).

## Architecture

| Service | Description | Sous-domaine |
|---|---|---|
| Traefik | Reverse proxy / gestion des certificats HTTPS | `${MY_DOMAIN}` (dashboard) |
| Portainer | Interface de gestion Docker | `portainer.${MY_DOMAIN}` |
| Jellyfin | Serveur multimédia | `jellyfin.${MY_DOMAIN}` |
| Blogverse Backend | API (TP2) | `backend.${MY_DOMAIN}` |
| Blogverse Frontend | Interface web (TP2) | `blogverse.${MY_DOMAIN}` |
| MySQL | Base de données de Blogverse | interne uniquement (non exposée) |

Tout le trafic externe passe par Traefik (ports 80/443), qui redirige le HTTP vers HTTPS et route chaque requête vers le bon service selon le sous-domaine.

## Prérequis

- Docker et Docker Compose installés
- Accès `sudo` sur la machine hôte
- Un nom de domaine pointant vers le serveur, avec les sous-domaines (`*.${MY_DOMAIN}`)
- Un compte DuckDNS avec un token valide (challenge DNS pour Let's Encrypt)
- Dossiers média présents sur l'hôte pour Jellyfin : `/mnt/media/Movies` et `/mnt/media/TV`

## Variables d'environnement

À définir dans `.env` (voir `.env.example`) :

| Variable | Description |
|---|---|
| `MY_DOMAIN` | Domaine principal (ex : `example.com`) |
| `MY_EMAIL` | Email utilisé pour Let's Encrypt |
| `DUCKDNS_TOKEN` | Token DuckDNS pour le challenge DNS |
| `MYSQL_ROOT_PASSWORD` | Mot de passe root MySQL |
| `MYSQL_DATABASE` | Nom de la base de données |
| `MYSQL_USER` | Utilisateur MySQL |
| `MYSQL_PASSWORD` | Mot de passe de l'utilisateur MySQL |
| `BACKEND_PORT` | Port interne du backend (défaut : `3001`) |
| `JWT_SECRET` | Clé secrète pour la signature des JWT |
| `JWT_EXPIRES_IN` | Durée de validité des tokens JWT (défaut : `7d`) |
| `VITE_API_URL` | URL de l'API utilisée par le frontend au build |

## Installation

1. Cloner le dépôt
   ```bash
   git clone https://github.com/amineelghazi/tp3-2389824.git
   cd tp3-2389824/
   ```

2. Configurer les variables d'environnement
   ```bash
   cp .env.example .env
   nano .env
   ```

3. Démarrer les services
   ```bash
   sudo docker compose up -d
   ```

4. Vérifier que les conteneurs tournent
   ```bash
   sudo docker ps
   ```

## Accès aux services

Une fois démarrés, les services sont accessibles en HTTPS (certificat généré automatiquement) :

- Portainer : `https://portainer.${MY_DOMAIN}`
- Jellyfin : `https://jellyfin.${MY_DOMAIN}`
- Blogverse (frontend) : `https://blogverse.${MY_DOMAIN}`
- API Blogverse (backend) : `https://backend.${MY_DOMAIN}`
- Dashboard Traefik : `https://${MY_DOMAIN}`

## Maintenance

### Portainer

Si le service reste ouvert trop longtemps, il peut devenir instable et nécessiter un redémarrage :

```bash
sudo docker compose down portainer
sudo docker compose up -d portainer
```

## Notes

- `db` (MySQL) n'est accessible que sur le réseau interne `blogverse-network` et n'est jamais exposée publiquement.
- `backend` attend que `db` soit en bonne santé (healthcheck) avant de démarrer.
- `frontend` dépend du démarrage de `backend`.
- Traefik gère automatiquement l'émission et le renouvellement des certificats HTTPS via le challenge DNS DuckDNS.
