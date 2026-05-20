#  Day 07 — Régression Linéaire sur des Prix de Maisons

> **Série : 30 Days of Python for Data** · Jour 7/30  
> Publié sur [LinkedIn]()

---

## 📌 Ce que tu vas apprendre

| Concept | Fonction | Utilité |
|---------|----------|---------|
| Générer des données | `np.random.seed()` | Données reproductibles à chaque exécution |
| Séparer train/test | `train_test_split()` | Ne jamais évaluer sur les données d'entraînement |
| Entraîner le modèle | `model.fit()` | Trouver les coefficients optimaux |
| Évaluer | `r2_score`, `MAE`, `RMSE` | Trois angles différents sur la précision |
| Visualiser | `matplotlib` | Réel vs Prédit, résidus, importance features |

---

## 📁 Structure du projet

```
day-07-regression-lineaire/
│
├── regression_lineaire.py   ← Script principal
└── README.md
```

---

## 🚀 Installation & Lancement

```bash
pip install pandas numpy scikit-learn matplotlib
python regression_lineaire.py
```

---

## 🧠 Explications détaillées

### `np.random.seed(42)`

Fixe le générateur aléatoire. Avec le même seed, `np.random.randint`
et `np.random.normal` produisent exactement les mêmes nombres à chaque
exécution. C'est indispensable pour un tutoriel reproductible — ton
audience doit obtenir les mêmes résultats que toi.

---

### La génération du prix

```python
prix = surface * 3500 + nb_pieces * 8000 - age * 500 + etage * 2000 + bruit
```

On simule une relation linéaire réelle : chaque m² vaut 3 500€, chaque
pièce ajoute 8 000€, chaque année d'âge enlève 500€, chaque étage
ajoute 2 000€.

Le `bruit` = `np.random.normal(0, 15000)` ajoute une variation
aléatoire réaliste — dans la vraie vie, deux maisons identiques n'ont
pas exactement le même prix.

---

### `train_test_split(X, y, test_size=0.2, random_state=42)`

Quatre paramètres à retenir :

- **`X`** — les features (ce qu'on donne au modèle)
- **`y`** — la cible (ce qu'on veut prédire)
- **`test_size=0.2`** — 20% des données vont en test, 80% en entraînement
- **`random_state=42`** — mélange les données de façon reproductible

Sans cette séparation, le modèle "triche" — il a déjà vu les données
sur lesquelles il est évalué. C'est comme donner les réponses avant
l'examen.

---

### `model.fit(X_train, y_train)`

C'est ici que la magie opère. L'algorithme cherche les coefficients
qui minimisent l'erreur entre les prix réels et les prix prédits.
Pour une régression linéaire c'est instantané — l'équation normale
a une solution analytique exacte.

---

### `model.coef_` et `model.intercept_`

- **`coef_`** — le poids de chaque feature. `surface = 3521` signifie
  que chaque m² supplémentaire ajoute 3 521€ au prix prédit.
- **`intercept_`** — la valeur de base quand toutes les features sont
  à 0. Rarement interprétable directement.

---

### Les 3 métriques

**R² (R-carré)**
Proportion de la variance expliquée par le modèle.
- `0` = le modèle ne sert à rien
- `1` = prédiction parfaite
- En pratique : > 0.8 c'est bon, > 0.95 c'est excellent

**MAE — Mean Absolute Error**
Erreur moyenne en valeur absolue, dans la même unité que `y`.
Ici 13 568€. C'est la métrique la plus intuitive à expliquer
à un non-technicien.

**RMSE — Root Mean Squared Error**
Comme le MAE mais les grandes erreurs sont pénalisées davantage
(on les élève au carré avant de faire la moyenne). Utile quand
une grosse erreur est bien pire que deux petites erreurs combinées.

---

### Le graphique Réel vs Prédit

Si le modèle est parfait, tous les points sont sur la diagonale `y = x`.

- Un point **au-dessus** → le modèle surestime
- Un point **en-dessous** → il sous-estime
- Un nuage **sans pattern** autour de la diagonale → le modèle est bon

---

### L'analyse des résidus

Un résidu = prix réel − prix prédit.

Si le modèle est correct, les résidus doivent être distribués
aléatoirement autour de zéro — pas de pattern, pas de tendance.
Si tu vois une courbe ou un entonnoir, il manque quelque chose
au modèle (une feature non linéaire, une interaction, etc.).

---

## 📊 Résultats obtenus

| Métrique | Valeur | Interprétation |
|----------|--------|----------------|
| **R²** | 0.993 | Le modèle explique 99.3% de la variance |
| **MAE** | ~13 500€ | Erreur moyenne de 13 500€ |
| **RMSE** | ~18 000€ | Grandes erreurs pénalisées |

### Coefficients appris

| Feature | Coefficient | Sens |
|---------|-------------|------|
| surface | +3 521 €/m² | Chaque m² ajoute 3 521€ |
| nb_pieces | +7 800 €/pièce | Chaque pièce ajoute 7 800€ |
| age | −513 €/an | Chaque année enlève 513€ |
| etage | +1 950 €/étage | Chaque étage ajoute 1 950€ |

---

## 💡 Pour aller plus loin

- [ ] Ajouter la variable `quartier` (encodage one-hot)
- [ ] Tester `Ridge` et `Lasso` pour la régularisation
- [ ] Comparer avec `RandomForestRegressor`
- [ ] Faire une validation croisée avec `cross_val_score`
- [ ] Connecter à un vrai dataset immobilier (Kaggle)

---

## 📚 Ressources

- [scikit-learn LinearRegression](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html)
- [train_test_split](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.train_test_split.html)
- [Dataset immobilier Kaggle](https://www.kaggle.com/c/house-prices-advanced-regression-techniques)

---


---


---

⭐ **Si ce projet t'aide, mets une étoile !**
