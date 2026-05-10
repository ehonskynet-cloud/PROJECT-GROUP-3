# 🍽️ Module Gestion des Produits — Guide d'intégration
## Projet ENSPY 2025-2026 · Restaurant Manager Django

---

## 📦 Fichiers fournis

| Fichier | Rôle |
|---------|------|
| `models_produits.py` | Modèles Django : `Produit`, `Ingredient`, `Composition_Produit`, `Employe` |
| `forms_produits.py` | Formulaires : `ProduitForm`, `CompositionFormSet`, `FiltreProduitForm` |
| `views_produits.py` | 6 vues : catalogue, détail, créer, modifier, supprimer, API JSON |
| `urls_produits.py` | 6 routes URL |
| `admin_produits.py` | Configuration interface d'administration Django |
| `templates/restaurant_app/produits/` | 4 templates HTML |

---

## ⚙️ Étapes d'intégration

### 1. Modèles → `restaurant_app/models.py`

Copiez le contenu de `models_produits.py` dans votre `models.py` existant.

> ⚠️ Si `Employe` et `Ingredient` sont déjà définis, **ne les recopiez pas**.
> Vérifiez seulement que `managed = False` est présent (tables déjà créées via SQL).

**Important** : le champ `Prix` n'existe pas encore dans la table SQL.
Exécutez cette requête MySQL pour l'ajouter :

```sql
ALTER TABLE Produit ADD COLUMN Prix INT DEFAULT 0 AFTER Nombre_Personnes;
```

### 2. Formulaires → `restaurant_app/forms_produits.py`

Créez ce nouveau fichier (ou fusionnez dans `forms.py` existant).

### 3. Vues → `restaurant_app/views_produits.py`

Créez ce nouveau fichier (ou fusionnez dans `views.py` existant).

> Le décorateur `@role_requis` utilise `request.session['role']` et
> `request.session['employe_id']` — vérifiez que votre `views.py` existant
> stocke bien ces clés lors de la connexion.

### 4. URLs → `restaurant_app/urls.py`

Ajoutez à la fin de votre `urls.py` existant :

```python
# Importer les URLs produits
from .views_produits import (
    catalogue_produits, detail_produit,
    creer_produit, modifier_produit,
    supprimer_produit, api_ingredients,
)

# Ajouter dans urlpatterns :
urlpatterns += [
    path('produits/',                    catalogue_produits,  name='catalogue_produits'),
    path('produits/<int:pk>/',           detail_produit,      name='detail_produit'),
    path('produits/nouveau/',            creer_produit,       name='creer_produit'),
    path('produits/<int:pk>/modifier/',  modifier_produit,    name='modifier_produit'),
    path('produits/<int:pk>/supprimer/', supprimer_produit,   name='supprimer_produit'),
    path('api/ingredients/',             api_ingredients,     name='api_ingredients'),
]
```

### 5. Admin → `restaurant_app/admin.py`

Ajoutez en haut de votre `admin.py` :

```python
from .admin_produits import *
```

### 6. Templates → `templates/restaurant_app/produits/`

Créez le dossier `produits/` dans votre dossier `templates/restaurant_app/`
et copiez-y les 4 fichiers HTML fournis.

### 7. Lien dans le Dashboard

Dans votre `dashboard.html`, ajoutez un lien vers le catalogue :

```html
<a href="{% url 'catalogue_produits' %}">🍽️ Produits</a>
```

---

## 🗄️ Schéma BD concerné

```
Produit (id_Produit, id_Createur→Employe, Nom, Description,
         Duree_Cuisson, Nombre_Personnes, Prix*)
         * colonne à ajouter via ALTER TABLE

Composition_Produit (id_Produit→Produit, id_Ingredient→Ingredient,
                     Quantite_Utilisee)   ← PK composite

Ingredient (id_Ingredient, Nom, Unite_de_Mesure, Type)
```

---

## 🔐 Permissions par rôle

| Action | Rôles autorisés |
|--------|----------------|
| Voir le catalogue | Tous (connectés) |
| Voir le détail | Tous (connectés) |
| Créer un produit | `administrateur`, `directeur`, `chef_cuisinier` |
| Modifier un produit | `administrateur`, `directeur`, `chef_cuisinier` |
| Supprimer un produit | `administrateur`, `directeur` |

---

## ✅ Fonctionnalités du Cahier des Charges couvertes

- [x] **§5.3a** — Catalogue plats (liste avec cartes visuelles)
- [x] **§5.3b** — Catégorisation (Plat / Boisson / Dessert / Entrée)
- [x] **§5.3c** — Prix dynamique (champ Prix éditable)
- [x] **§5.2a/b/c/d/e** — CRUD recettes avec ingrédients, quantités, durée, chef
- [x] **§2.2** — Permissions par rôle
- [x] **§7** — CSRF activé, ORM Django (pas de SQL brut)
