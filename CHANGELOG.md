# Changelog - Docker LAMP Stack

## Version 2.0.0 - Refonte complète (2024)

### 🎯 Objectifs de la refonte
- Migration de Traefik vers une solution plus simple
- Support du développement local multi-projets
- Flexibilité d'accès (ports ou domaines)
- Amélioration de la gestion des permissions

### ✨ Nouvelles fonctionnalités

#### 0. Support Cross-Platform (Linux, macOS, Windows)
- **Support Windows natif** avec init.bat
- **Gestion automatique UID/GID** selon l'OS
- **Scripts bash** compatibles Linux/Mac/Windows (Git Bash/WSL2)
- **Documentation Windows** dédiée (WINDOWS.md)
- **Permissions automatiques** sur Windows via Docker Desktop

#### 1. Double mode d'accès
- **Mode direct (défaut)**: Accès via `http://localhost:PORT`
- **Mode reverse proxy**: Accès via `https://domain.local`
- Basculement simple via variable `USE_REVERSE_PROXY` dans `.env`

#### 2. Support multi-projets
- Chaque projet peut avoir ses propres ports
- Isolation complète via réseaux Docker séparés
- Noms de projets configurables via `COMPOSE_PROJECT_NAME`

#### 3. Reverse proxy Nginx intégré (optionnel)
- Conteneur Nginx par projet
- Génération automatique de certificats SSL
- Configuration via profiles Docker Compose

#### 4. Scripts d'automatisation
- `init.sh`: Initialisation automatique du projet
  - Détection automatique UID/GID
  - Création du `.env`
  - Configuration des répertoires
  
- `lamp.sh`: Gestion simplifiée
  - Démarrage/arrêt facile
  - Visualisation des logs
  - Accès aux shells
  - Informations de connexion
  
- `generate-ssl-certs.sh`: Génération SSL
  - Certificats auto-signés pour dev local
  - Support pour domaines personnalisés

#### 5. Amélioration des permissions
- Mapping automatique UID/GID de l'utilisateur host
- Édition directe des fichiers sans problèmes de permissions
- Configuration par défaut saine

#### 6. Configuration enrichie
- Fichier `.env` plus complet et documenté
- Variables pour tous les aspects configurables
- Valeurs par défaut sensibles
- Documentation inline

#### 7. Documentation améliorée
- `README.md` complet avec exemples
- `QUICKSTART.md` pour démarrage rapide
- Guide de troubleshooting détaillé
- Exemples de configuration multi-projets

### 🔧 Modifications techniques

#### Docker Compose
- Suppression du réseau externe `networks_frontend`
- Ajout d'un réseau interne par projet
- Support des profiles pour activer/désactiver le reverse proxy
- Ports configurables dynamiquement
- Amélioration des healthchecks

#### Configuration Apache
- Suppression des redirections HTTPS forcées (gérées par Nginx si besoin)
- Support des headers `X-Forwarded-*` pour reverse proxy
- Configuration plus flexible des VirtualHosts

#### Configuration Nginx
- Template de configuration pour reverse proxy
- Support SSL/TLS moderne
- Headers de sécurité
- Configuration optimisée pour phpMyAdmin

#### Gestion SSL
- Support certificats auto-signés (développement)
- Support mkcert (certificats de confiance)
- Génération automatisée
- Documentation claire

### 🗑️ Suppressions

#### Traefik
- Suppression complète de Traefik
- Plus de dépendance à un reverse proxy externe
- Plus de labels Docker spécifiques

#### Ansible
- Suppression du workflow Ansible
- Configuration directe via `.env`
- Simplification du déploiement

#### Réseau externe
- Suppression de la dépendance au réseau `networks_frontend`
- Chaque projet a son propre réseau isolé

### 📋 Fichiers ajoutés
- `init.sh`: Script d'initialisation (cross-platform)
- `init.bat`: Script d'initialisation Windows natif
- `lamp.sh`: Script de gestion
- `generate-ssl-certs.sh`: Générateur de certificats SSL
- `config/nginx/default.conf`: Configuration Nginx
- `QUICKSTART.md`: Guide de démarrage rapide
- `WINDOWS.md`: Guide spécifique Windows
- `CROSS-PLATFORM.md`: Notes techniques cross-platform
- `.vscode/launch.json`: Configuration Xdebug pour VS Code
- `CHANGELOG.md`: Ce fichier

### 📝 Fichiers modifiés
- `docker-compose.yml`: Refonte complète
- `sample.env`: Enrichissement avec nouvelles variables
- `README.md`: Documentation complète réécrite
- `.gitignore`: Ajout de nouveaux patterns
- `config/vhosts/default.conf`: Adaptation pour les deux modes

### 🔄 Migration depuis v1.x

Pour migrer depuis l'ancienne version :

1. **Sauvegardez vos données importantes**
   ```bash
   # Sauvegarde de la base de données
   docker compose exec database mysqldump -u root -p$MYSQL_ROOT_PASSWORD --all-databases > backup.sql
   
   # Sauvegarde du code
   cp -r www www.backup
   ```

2. **Arrêtez l'ancien stack**
   ```bash
   docker compose down
   ```

3. **Mettez à jour les fichiers**
   ```bash
   git pull origin main
   ```

4. **Recréez votre `.env`**
   ```bash
   # L'ancien .env n'est pas compatible
   mv .env .env.old
   ./init.sh
   # Reportez vos configurations personnalisées
   ```

5. **Redémarrez**
   ```bash
   ./lamp.sh start
   ```

### ⚠️ Breaking Changes

- Le réseau `networks_frontend` n'existe plus
- Les labels Traefik ne sont plus utilisés
- La structure du `.env` a changé
- Les ports doivent être explicitement configurés
- phpMyAdmin n'a plus de port dédié par défaut (accès via alias Apache)

### 🐛 Bugs connus
Aucun bug connu pour le moment.

### 🚀 Prochaines améliorations prévues
- [ ] Support de Mailhog pour tester les emails
- [ ] Support de Elasticsearch/Kibana (optionnel)
- [ ] Script de backup automatisé
- [ ] Support Docker Swarm (production)
- [ ] Interface Web de gestion (optionnel)

### 👥 Contributeurs
- Refonte complète basée sur le projet original docker-compose-lamp

### 📄 License
MIT License - Voir LICENSE file

---

## Version 1.x - Historique

Version originale avec Traefik et Ansible.
Voir l'historique Git pour plus de détails.
