# 🍽️ Restaurant Manager — Guide de démarrage
## Projet ENSPY 2025-2026 · Python Django · MySQL

---

## ⚙️ Installation & Lancement

### 1. Cloner le projet et créer l'environnement virtuel
\`\`\`bash
git clone <votre-repo-github>
cd restaurant_project
python -m venv venv
source venv/bin/activate          # Linux/Mac
venv\Scripts\activate             # Windows
\`\`\`

### 2. Installer les dépendances
\`\`\`bash
pip install -r requirements.txt
\`\`\`

### 3. Configurer la base de données MySQL
- Ouvrir `restaurant_project/settings.py`
- Modifier les variables `USER` et `PASSWORD` selon votre config MySQL
- Importer la base de données SQL fournie :
\`\`\`bash
mysql -u root -p < Base_Restaurant.sql
\`\`\`

### 4. Appliquer les migrations Django
\`\`\`bash
python manage.py makemigrations
python manage.py migrate
\`\`\`

### 5. Créer un super-utilisateur (Administrateur)
\`\`\`bash
python manage.py createsuperuser
\`\`\`
> Lors de la création, choisissez le rôle `administrateur`

### 6. Lancer le serveur
\`\`\`bash
python manage.py runserver
\`\`\`
Accéder à : http://127.0.0.1:8000/

---

## 🗂️ Structure du projet

\`\`\`
restaurant_project/
├── manage.py
├── requirements.txt
├── restaurant_project/
│   ├── settings.py        ← Configuration Django + MySQL
│   ├── urls.py            ← URLs principales
│   └── wsgi.py
└── restaurant_app/
    ├── models.py          ← Modèles Django (schéma SQL converti)
    ├── views.py           ← Logique métier & authentification
    ├── forms.py           ← Formulaires de connexion/inscription
    ├── admin.py           ← Interface d'administration configurée
    ├── urls.py            ← URLs de l'application
    └── templates/
        └── restaurant_app/
            ├── base.html              ← Template parent (héritage)
            ├── accueil.html           ← Page d'accueil (choix Admin/Client)
            ├── connexion_admin.html   ← Login personnel du restaurant
            ├── connexion_client.html  ← Login client
            ├── inscription_client.html← Création de compte
            ├── dashboard.html         ← Tableau de bord (admin/employés)
            └── espace_client.html     ← Espace personnel client
\`\`\`

---

## 🔐 URLs disponibles

| URL | Description |
|-----|-------------|
| `/` | Page d'accueil — choix du portail |
| `/login/admin/` | Connexion Personnel/Administrateur |
| `/login/client/` | Connexion Client |
| `/inscription/` | Inscription Client |
| `/dashboard/` | Tableau de bord (admin/employés) |
| `/espace-client/` | Espace personnel client |
| `/logout/` | Déconnexion |
| `/admin/` | Interface d'administration Django |

---

## 👤 Rôles disponibles

| Rôle | Accès |
|------|-------|
| `administrateur` | Accès total + interface admin |
| `directeur` | Dashboard + tous les modules |
| `chef_cuisinier` | Dashboard + produits/recettes |
| `cuisinier` | Dashboard + commandes |
| `serveur` | Dashboard + commandes/réservations |
| `gestionnaire_stock` | Dashboard + stock |
| `client` | Espace client uniquement |

---

## 🔒 Sécurité implémentée

- ✅ Protection CSRF sur tous les formulaires (`{% csrf_token %}`)
- ✅ Hachage des mots de passe (Django `make_password`)
- ✅ Sessions sécurisées (expire après 1h ou fermeture navigateur)
- ✅ Vérification des rôles à chaque vue (`login_required`)
- ✅ Protection contre les injections SQL via Django ORM
- ✅ Séparation stricte des portails Admin / Client

---

## 👥 Contributeurs

| Nom | Rôle |
|-----|------|
| Requielleyeinko | Développeuse |
