# Popeye - Application de Vote en Temps Réel

## 📋 Description

Popeye est une application de vote distribuée en temps réel qui permet aux utilisateurs de voter pour leur outil DevOps préféré. L'application est composée de plusieurs microservices containerisés avec Docker et utilise une architecture moderne avec Flask, Node.js, Java, Redis et PostgreSQL.

## 🏗️ Architecture

L'application suit une architecture microservices avec les composants suivants :

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│    Poll     │    │   Result    │    │   Worker    │    │   Database  │
│  (Flask)    │    │ (Node.js)   │    │   (Java)    │    │(PostgreSQL) │
│             │    │             │    │             │    │             │
│   Port:     │    │   Port:     │    │             │    │             │
│   5000      │    │   5001      │    │             │    │             │
└─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘
       │                   │                   │                   │
       └───────────────────┼───────────────────┼───────────────────┘
                           │                   │
                    ┌─────────────┐           │
                    │    Redis    │           │
                    │             │           │
                    │   Port:     │           │
                    │   6379      │           │
                    └─────────────┘───────────┘
```

### Composants

1. **Poll Service** (Python/Flask) - Interface de vote
2. **Result Service** (Node.js/Express) - Affichage des résultats en temps réel
3. **Worker Service** (Java) - Traitement des votes en arrière-plan
4. **Redis** - Queue des messages et cache
5. **PostgreSQL** - Base de données pour la persistance des votes

## 🚀 Démarrage Rapide

### Prérequis

- Docker
- Docker Compose

### Installation et Lancement

1. **Cloner le repository**
   ```bash
   git clone git@github.com:Amayyas/Popeye.git
   cd Popeye
   ```

2. **Lancer l'application**
   ```bash
   docker-compose up --build
   ```

3. **Accéder aux services**
   - Interface de vote : http://localhost:5000
   - Résultats en temps réel : http://localhost:5001

## 🔧 Services Détaillés

### Poll Service (Port 5000)
- **Technologie** : Python 3.9 + Flask
- **Fonction** : Interface utilisateur pour voter
- **Fonctionnalités** :
  - Interface web responsive
  - Gestion des cookies pour identifier les votants
  - Envoi des votes vers Redis
  - Options configurables via variables d'environnement

### Result Service (Port 5001)
- **Technologie** : Node.js 20 + Express + Socket.IO
- **Fonction** : Affichage des résultats en temps réel
- **Fonctionnalités** :
  - Mise à jour en temps réel via WebSockets
  - Visualisation graphique des résultats
  - Interface moderne et responsive

### Worker Service
- **Technologie** : Java 21 + Maven
- **Fonction** : Traitement asynchrone des votes
- **Fonctionnalités** :
  - Consommation de la queue Redis
  - Insertion/mise à jour des votes en base
  - Gestion de la reconnexion automatique

### Base de Données
- **Technologie** : PostgreSQL 16
- **Schéma** :
  ```sql
  CREATE TABLE votes (
    id text PRIMARY KEY,
    vote text NOT NULL
  );
  ```

## 🌐 Variables d'Environnement

### Options de Vote (Poll Service)
- `OPTION_A` : Option A (défaut: "Ansible")
- `OPTION_B` : Option B (défaut: "Chef")
- `OPTION_C` : Option C (défaut: "Puppet")
- `OPTION_D` : Option D (défaut: "SaltStack")

### Configuration Base de Données
- `POSTGRES_HOST` : Hôte PostgreSQL
- `POSTGRES_PORT` : Port PostgreSQL (défaut: 5432)
- `POSTGRES_DB` : Nom de la base de données
- `POSTGRES_USER` : Utilisateur PostgreSQL
- `POSTGRES_PASSWORD` : Mot de passe PostgreSQL

### Configuration Redis
- `REDIS_HOST` : Hôte Redis

## 🔄 Flux de Données

1. **Vote** : L'utilisateur vote via l'interface web (Poll Service)
2. **Queue** : Le vote est ajouté à la queue Redis
3. **Traitement** : Le Worker consomme le vote de la queue
4. **Persistance** : Le vote est sauvegardé en base PostgreSQL
5. **Affichage** : Les résultats sont mis à jour en temps réel (Result Service)

## 🏃‍♂️ Développement

### Structure du Projet
```
Popeye/
├── docker-compose.yml          # Configuration Docker Compose
├── schema.sql                  # Schéma de base de données
├── poll/                       # Service de vote (Python/Flask)
│   ├── app.py
│   ├── Dockerfile
│   ├── requirements.txt
│   └── templates/
├── result/                     # Service de résultats (Node.js)
│   ├── server.js
│   ├── package.json
│   ├── Dockerfile
│   └── views/
├── worker/                     # Service de traitement (Java)
│   ├── Dockerfile
│   ├── pom.xml
│   └── src/main/java/worker/
└── README.md
```

### Commandes Utiles

**Arrêter les services**
```bash
docker-compose down
```

**Voir les logs**
```bash
docker-compose logs -f [service-name]
```

**Reconstruire un service spécifique**
```bash
docker-compose up --build [service-name]
```

**Accéder à la base de données**
```bash
docker-compose exec db psql -U poll_user -d poll_db
```

## 🐛 Dépannage

### Problèmes Courants

1. **Erreur de connexion Redis**
   - Vérifier que Redis est démarré
   - Vérifier la configuration réseau Docker

2. **Erreur de connexion PostgreSQL**
   - Attendre que la base soit initialisée
   - Vérifier les variables d'environnement

3. **Les votes ne s'affichent pas**
   - Vérifier que le Worker fonctionne
   - Vérifier les logs : `docker-compose logs worker`

## 📊 Monitoring

Pour surveiller l'état de l'application :

```bash
# État des conteneurs
docker-compose ps

# Logs en temps réel
docker-compose logs -f

# Utilisation des ressources
docker stats
```

## 🔗 Technologies Utilisées

- **Frontend** : HTML5, CSS3, JavaScript, Socket.IO
- **Backend** : Python (Flask), Node.js (Express), Java 21
- **Base de données** : PostgreSQL 16
- **Cache/Queue** : Redis 7
- **Containerisation** : Docker, Docker Compose
- **Build Tools** : Maven (Java), npm (Node.js), pip (Python)
