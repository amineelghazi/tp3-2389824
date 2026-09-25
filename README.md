# TP3 – 2389824

Déploiement de services conteneurisés avec Docker Compose, gérés via Portainer.

## Services

**Obligatoire**
- Portainer

**Autres**
- Jellyfin
- Blogverse (TP2)
- Proxy (Traefik)

## Prérequis

- Docker et Docker Compose installés
- Accès `sudo` sur la machine hôte

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
   Renseigner les variables requises (token, email, etc.) avant de continuer.

3. Démarrer les services
```bash
   sudo docker compose up -d
```

4. Vérifier que les conteneurs tournent
```bash
   sudo docker ps
```

## Maintenance

### Portainer

Si le service reste ouvert trop longtemps, il peut devenir instable et nécessiter un redémarrage :

```bash
sudo docker compose down portainer
sudo docker compose up portainer
```
