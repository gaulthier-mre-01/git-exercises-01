# Exercices Git

---

## Exercice 1 — Rebase

**Objectif :** Rebaser une branche feature sur main.

```bash
git checkout ex1-rebase/feature
git rebase ex1-rebase/main
```

**Résultat :** Les commits de la branche feature (`feat: ajouter page menu`, `feat: ajouter les plats du menu`) sont rejoués au-dessus des derniers commits de `main`, produisant un historique linéaire et propre.

```
* cdcaf84 feat: ajouter les plats du menu
* c152e32 feat: ajouter page menu
* 1cea8a2 feat: ajouter page contact
* 3c3fb42 fix: corriger les horaires d'ouverture
* c7a7860 feat: ajouter description du restaurant
* 93b9c5b init: page d'accueil
* 805ccf9 root: point de depart commun
```

---

## Exercice 2 — Rebase Interactif

**Objectif :** Nettoyer un historique de commits désordonné avec `rebase -i`, puis corriger un message de commit avec `--amend`.

```bash
git rebase -i HEAD~8
git commit --amend -m "feat: page d'accueil"
```

**Résultat :** Plusieurs commits WIP sont regroupés et réorganisés, et le message du commit final est corrigé.

```
cb30a41 feat: page d'accueil
5bf9683 add: style.css
b61738e init: README
805ccf9 root: point de depart commun
```

---

## Exercice 3 — Merge avec Résolution de Conflit

**Objectif :** Fusionner une branche feature dans main en résolvant manuellement un conflit de contenu.

```bash
git merge ex3-merge/feature
# Conflit dans menu.html → résolution manuelle
git checkout HEAD -- menu.html
```

**Résultat :** Le conflit sur `menu.html` (prix des pâtes) est résolu en choisissant la bonne version (16 EUR), et le commit de merge documente la résolution.

```
*   77a84ca Merge ex3-merge/feature: résolution conflit prix 16 EUR
|\
| * 31829cd feat: ajuster le prix des pates
| * 1081ac7 feat: ajouter section vegetarienne
* | 35c4c3f fix: mettre a jour le prix des pates
|/
* d2d8a68 init: page menu
```

---

## Exercice 4 — Squash

**Objectif :** Regrouper plusieurs commits WIP en un seul commit propre avant de fusionner dans main.

```bash
git checkout ex4-squash/feature
git rebase -i ex4-squash/main        # regrouper tous les commits WIP en un seul
git commit --amend -m "feat: ajouter le footer complet"
git checkout ex4-squash/main
git merge ex4-squash/feature          # fusion en fast-forward
```

**Résultat :** Cinq commits WIP du footer sont regroupés en un seul commit explicite, et la fusion dans main se fait en fast-forward proprement.

```
* eaeb040 feat: ajouter le footer complet
* 8d3b7bc feat: ajouter le header
* f8dc175 init: page index
* 805ccf9 root: point de depart commun
```

---

## Exercice 5 — Cherry-pick

**Objectif :** Appliquer uniquement certains commits d'une branche feature sur main, en ignorant les autres.

```bash
git checkout ex5-cherrypick/main
git cherry-pick 7545bd5   # hotfix: corriger le lien de navigation casse
git cherry-pick a9144ae   # fix: corriger la faille XSS dans le formulaire contact
```

Les commits suivants de la branche feature ont été **intentionnellement exclus** :
- `feat: mode sombre experimental (ne pas merger)`
- `WIP: refacto complete du CSS`

**Résultat :** Seuls les correctifs pertinents sont portés sur main, sans embarquer du travail inachevé ou expérimental.

```
* c550c99 fix: corriger la faille XSS dans le formulaire contact
* 3567b06 hotfix: corriger le lien de navigation casse
* 42e6052 feat: ajouter galerie photos
* e293ba5 init: page d'accueil
* 805ccf9 root: point de depart commun
```

---

## Récapitulatif

| Exercice | Concept | Commande principale |
|----------|---------|---------------------|
| 1 | Rebase sur main | `git rebase <branche>` |
| 2 | Rebase interactif + amend | `git rebase -i`, `git commit --amend` |
| 3 | Résolution de conflit de merge | `git merge`, résolution manuelle |
| 4 | Regroupement de commits (squash) | `git rebase -i` (squash), fusion fast-forward |
| 5 | Sélection de commits spécifiques | `git cherry-pick <hash>` |
