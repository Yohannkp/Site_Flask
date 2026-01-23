Application : https://sqlite-tp-flask-8.onrender.com/login

# 🎫 Système de Gestion de Tickets IT

Une application web complète de gestion de tickets IT développée avec **Flask**, offrant un système d'authentification sécurisé, une distinction de rôles (Admin/User) et des fonctionnalités de communication en temps réel.

## 📋 Vue d'ensemble

Cette application permet aux utilisateurs de :
- ✅ S'inscrire et se connecter de manière sécurisée
- 🎯 Créer et gérer leurs tickets de support IT
- 💬 Communiquer avec les administrateurs via des messages intégrés
- 📊 Consulter l'état de leurs tickets (ouvert, en cours, fermé, en attente)
- 👥 Disposer d'un tableau de bord avec gestion de tâches personnelles

Les administrateurs peuvent :
- 👨‍💼 Consulter tous les tickets du système
- 📈 Accéder aux statistiques globales
- 🔧 Gérer les tickets des autres utilisateurs
- 💭 Répondre aux tickets avec statut de modérateur

## 🏗️ Architecture du Projet

```
Site_Flask/
├── site_flask/
│   ├── app.py                 # Application principale Flask
│   ├── migrations/            # Migrations de base de données (Alembic)
│   ├── static/
│   │   └── css/
│   │       └── style.css      # Feuilles de style
│   ├── templates/             # Modèles HTML Jinja2
│   │   ├── index.html         # Page d'accueil/tableau de bord
│   │   ├── login.html         # Page de connexion
│   │   ├── register.html      # Page d'inscription
│   │   ├── dashboard.html     # Tableau de bord utilisateur
│   │   ├── tickets.html       # Gestion des tickets utilisateur
│   │   ├── gestion_tickets.html # Gestion tickets (admin)
│   │   ├── edit_ticket.html   # Édition de ticket
│   │   ├── ticket_view.html   # Détail d'un ticket
│   │   ├── user_tickets.html  # Tickets d'un utilisateur spécifique
│   │   ├── users.html         # Gestion utilisateurs
│   │   └── mdp.html           # Récupération de mot de passe
│   ├── instance/              # Configuration locale (non versionné)
│   └── __pycache__/           # Fichiers Python compilés
└── README.md
```

## 🗄️ Modèles de Données

### User
```
- id: Identifiant unique (PK)
- username: Nom d'utilisateur (unique)
- password: Mot de passe hashé (bcrypt)
- role: Rôle utilisateur ("admin" ou "user")
- tasks: Relation vers Task
- tickets: Relation vers Ticket
```

### Ticket
```
- id: Identifiant unique (PK)
- user_id: Référence à l'utilisateur (FK)
- title: Titre du ticket
- description: Description détaillée
- status: État du ticket ("ouvert", "en cours", "fermé", "en attente")
- messages: Relation vers Message
```

### Message
```
- id: Identifiant unique (PK)
- ticket_id: Référence au ticket (FK)
- user_id: Référence à l'utilisateur créateur (FK)
- admin_id: Référence à l'administrateur répondant (FK, nullable)
- content: Contenu du message
- timestamp: Date/heure du message
```

### Task
```
- id: Identifiant unique (PK)
- content: Description de la tâche
- user_id: Référence à l'utilisateur (FK)
```

## 🚀 Fonctionnalités Principales

### 🔐 Authentification
- Inscription avec validation de formulaire (WTForms)
- Connexion sécurisée avec hashage bcrypt
- Gestion des sessions Flask
- Récupération de mot de passe oublié
- Intégration Flask-Login pour gestion d'authentification

### 🎫 Gestion des Tickets
- **Création** : Les utilisateurs créent des tickets avec titre et description
- **Lecture** : Consultation des détails d'un ticket
- **Mise à jour** : Édition des tickets par leurs créateurs
- **Suppression** : Suppression de tickets personnels
- **États multiples** : Suivi de l'avancement (ouvert, en cours, fermé, en attente)

### 💬 Système de Messagerie
- Messages en temps réel via **Flask-SocketIO**
- Distinction entre messages utilisateurs et réponses administrateurs
- Historique complet des communications par ticket
- Horodatage automatique des messages

### 📊 Tableau de Bord Admin
- Vue d'ensemble des statistiques :
  - Nombre total d'utilisateurs
  - Nombre total de tickets
  - Nombre de tickets par état
- Consultation de tous les tickets du système
- Gestion des utilisateurs et de leurs tickets

### ✅ Gestion des Tâches
- Création de tâches personnelles pour chaque utilisateur
- Suppression de tâches complétées
- Affichage dans le tableau de bord utilisateur

## 🛠️ Technologies Utilisées

| Technologie | Version | Utilisation |
|---|---|---|
| Flask | 3.x | Framework web principal |
| Flask-SQLAlchemy | - | ORM pour la base de données |
| Flask-Migrate | - | Gestion des migrations (Alembic) |
| Flask-Login | - | Gestion de l'authentification |
| Flask-WTF | - | Gestion des formulaires web |
| Flask-Bcrypt | - | Hashage sécurisé des mots de passe |
| Flask-SocketIO | - | Communication en temps réel WebSocket |
| Flask-MySQLdb | - | Connecteur MySQL |
| MySQL | 8.x | Base de données relationnelle |
| Jinja2 | - | Templating HTML |
| Bootstrap/CSS | - | Interface utilisateur |

## 📋 Configuration Requise

### Prérequis
- Python 3.8+
- MySQL Server 8.0+
- pip (gestionnaire de paquets Python)

### Variables d'Environnement
```bash
FLASK_APP=site_flask/app.py
FLASK_ENV=development
SECRET_KEY=ma_cle_secrete
SQLALCHEMY_DATABASE_URI=mysql+mysqlconnector://root:@localhost/gestion_tickets_it
```

## 🔧 Installation et Configuration

### 1. Clonage et Navigation
```bash
git clone <repository-url>
cd Site_Flask
```

### 2. Création d'un Environnement Virtuel
```bash
python -m venv venv
source venv/bin/activate  # Sur Windows: venv\Scripts\activate
```

### 3. Installation des Dépendances
```bash
pip install -r requirements.txt
```

### 4. Configuration de la Base de Données
```bash
# Créer la base de données MySQL
mysql -u root -p
> CREATE DATABASE gestion_tickets_it;
> EXIT;
```

### 5. Initialisation des Migrations
```bash
flask db init
flask db migrate -m "Initial migration"
flask db upgrade
```

### 6. Lancement de l'Application
```bash
python site_flask/app.py
```

L'application sera accessible à `http://10.74.3.216:8080`

## 📝 Utilisation

### Flux d'Utilisateur Standard
1. **Inscription** : Accès à `/register` pour créer un compte (rôle User par défaut)
2. **Connexion** : `/login` avec identifiants
3. **Tableau de Bord** : `/dashboard` pour gérer les tâches
4. **Créer un Ticket** : `/tickets` pour signaler un problème
5. **Communiquer** : `/ticket/<id>` pour discuter avec l'admin

### Flux d'Administrateur
1. **Inscription** : Créer un compte avec rôle Admin
2. **Connexion** : Accès au `/index` avec vue d'ensemble
3. **Gestion Globale** : Consultation de tous les tickets
4. **Réponse aux Tickets** : Messagerie avec les utilisateurs

### Endpoints Principaux

| Endpoint | Méthode | Description |
|---|---|---|
| `/` | GET | Page d'accueil/dashboard |
| `/register` | GET, POST | Inscription utilisateur |
| `/login` | GET, POST | Connexion utilisateur |
| `/logout` | GET | Déconnexion |
| `/dashboard` | GET, POST | Tableau de bord personnel |
| `/tickets` | GET, POST | Gestion des tickets |
| `/ticket/<id>` | GET, POST | Détail d'un ticket |
| `/edit_ticket/<id>` | GET, POST | Édition d'un ticket |
| `/delete_ticket/<id>` | GET, POST | Suppression d'un ticket |
| `/user_tickets/<id>` | GET | Tickets d'un utilisateur |
| `/admin/tickets` | GET | Tous les tickets (Admin) |
| `/send_message` | POST | Envoi de message |
| `/forget_password` | GET, POST | Récupération du mot de passe |

## 🔒 Sécurité

- ✅ Mots de passe hashés avec **bcrypt** (5 rounds)
- ✅ Validation de session pour les routes protégées
- ✅ Vérification des permissions (Admin/User)
- ✅ Protection CSRF via Flask-WTF
- ✅ Clé secrète pour les sessions (à modifier en production)
- ✅ Requêtes SQL paramétrées contre les injections SQL

## ⚠️ Notes Importantes

### Pour la Production
- [ ] Modifier `SECRET_KEY` dans `app.py`
- [ ] Utiliser une clé secrète forte (générer avec `secrets.token_hex(32)`)
- [ ] Configurer `SQLALCHEMY_ECHO = False`
- [ ] Utiliser des variables d'environnement pour les données sensibles
- [ ] Mettre en place HTTPS/SSL
- [ ] Configurer un serveur WSGI (Gunicorn, uWSGI)
- [ ] Mettre en place les logs d'application

### Améliorations Futures
- [ ] Ajouter des priorités de tickets (basse, moyenne, haute, critique)
- [ ] Implémentation d'assignation de tickets à des administrateurs
- [ ] Système de notations/satisfaction client
- [ ] Export des données (PDF, Excel)
- [ ] Intégration email pour les notifications
- [ ] Deux facteurs d'authentification (2FA)
- [ ] Dashboard analytic amélioré avec graphiques
- [ ] Recherche et filtrage avancés
- [ ] Pagination des listes

## 🐛 Dépannage

**Erreur de connexion MySQL**
```
Vérifier que MySQL est démarré et que les identifiants sont corrects dans app.py
```

**Erreur de migration**
```bash
flask db stamp head  # Réinitialiser les migrations
flask db upgrade     # Appliquer les migrations
```

**SocketIO non fonctionnel**
```
Vérifier que python-socketio et python-engineio sont installés
pip install python-socketio python-engineio
```

## 📜 Licence

À définir selon vos besoins.

## 👨‍💻 Auteur

Développé comme projet de gestion de tickets IT avec Flask.

---

**Lien de Déploiement** : https://sqlite-tp-flask-8.onrender.com
