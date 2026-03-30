# Déploiement Nextcloud avec Docker Compose

Ce projet déploie une instance **Nextcloud** complète avec :
- Base de données **PostgreSQL**
- Cache **Redis**
- Éditeur de documents **Collabora Online**
- **Cron job** pour les tâches automatiques
- **Nginx** comme reverse proxy

## URL de démonstration

> 🌐 **https://unirritative-concavely-arlean.ngrok-free.dev/**
>
> Identifiants de connexion :
>- **Utilisateur** : `admin`
>- **Mot de passe** :  `admin_secret`
>
> L'application est exposée via ngrok depuis ma machine locale.





## Prérequis

- [Docker](https://docs.docker.com/get-docker/) (≥ 24)
- Docker Compose (≥ 2.20, inclus avec Docker Desktop)



## Déploiement local

### 1. Cloner le projet

```bash
git clone https://github.com/TON_USERNAME/nextcloud-docker.git
cd mon-nextcloud
```


### 3. Lancer l'application

```bash
docker compose up -d
```

### 4. Vérifier que tout tourne

```bash
docker compose ps
```


### 5. Accéder à Nextcloud en local

```
https://localhost
```

Identifiants par défaut :
- **Utilisateur** : `admin`
- **Mot de passe** :  `admin_secret` 

---

## Exposition publique avec ngrok

Pour rendre l'application accessible depuis internet sans serveur dédié,
on utilise [ngrok](https://ngrok.com) qui crée un tunnel sécurisé depuis
la machine locale.

### 1. Installer ngrok

```bash
# Windows
winget install ngrok

# Ou télécharger sur https://ngrok.com/download
```

### 2. Configurer le token d'authentification

Créer un compte gratuit sur [dashboard.ngrok.com](https://dashboard.ngrok.com),
récupérer le token et l'enregistrer :

```bash
ngrok config add-authtoken TON_TOKEN
```

### 3. Lancer le tunnel

```bash
ngrok http 80
```

ngrok affiche une URL publique du type :
```
Forwarding   https://xxxx-xxxx.ngrok-free.app -> http://localhost:80
```

### 4. Autoriser le domaine ngrok dans Nextcloud

Nextcloud bloque par sécurité les domaines non déclarés.
Il faut ajouter l'URL ngrok dans la liste des domaines de confiance :

```bash
docker compose exec -u 33 nextcloud php occ config:system:set \
  trusted_domains 2 --value="xxxx-xxxx.ngrok-free.app"
```


L'application est ensuite accessible sur l'URL ngrok depuis n'importe quel navigateur.


