# GitFlow

Ce document définit le workflow Git utilisé par l'équipe pour le développement, la préparation des releases et la gestion des correctifs de production.

## 📑 Table des matières

- #-branches
  - #rôle-des-branches
- #-règles-générales
- [🚀évelopper-une-fonctionnalité
  - [1-mettre-à-jour-develop
  - [2. Créerla-branche
  - #3-développer-et-commiter
  - [4. Publierla-branche
  - [5. Créer une Pull Request](#5-créer-une-pull-request)
- [🐛 Corriger un bug horslease
- [🧪 Valider une release](#-livrer-une-release
  - #merge-vers-main
  - #synchronisation-avec-develop
  - [Checklist de livraison](#checklistger un problème critique en production - [1. Créer le hotfix](#1-créer-le-hotfix)
 rection
  - [3. Livrer le hotfix](#3-livrer-le-hotfix)
- es
  - #main
  - #develop
- #-cicd
- #-résumé-du-workflow
  - #feature
  - #bugfix
  - #release
  - #hotfix
- [⭐ Les 5 règles à retenir](#-les-5-règles-à-retenir) une Pull Request](#-checklistranches

Le projet utilise les branches suivantes :

```text
main
develop

feature/<nom>
bugfix/<nom>
release/<version>
hotfix/<version-ou-description>
```

### Rôle des branches

- `main` : contient le code livré en **production**.
- `develop` : contient les développements destinés à la **prochaine release**.
- `feature/*` : développement d'une nouvelle fonctionnalité.
- `bugfix/*` : correction d'un bug hors production.
- `release/*` : préparation et stabilisation d'une nouvelle version.
- `hotfix/*` : correction urgente d'un problème en production.

---

## 📋 Règles générales

- [ ] Ne jamais développer directement sur `main`.
- [ ] Ne jamais développer directement sur `develop`.
- [ ] Aucun push direct sur `main`.
- [ ] Les modifications sont intégrées via Pull Request / Merge Request.
- [ ] La CI doit être verte avant tout merge.
- [ ] Les tests doivent passer avant tout merge.
- [ ] Les Pull Requests doivent être validées par au moins un reviewer.
- [ ] Les branches terminées doivent être supprimées après leur merge.

---

## 🚀 Développer une fonctionnalité

Une nouvelle fonctionnalité part toujours de `develop`.

### 1. Mettre à jour `develop`

```bash
git switch develop
git pull origin develop
```

### 2. Créer la branche

```bash
git switch -c feature/user-authentication
```

Convention :

```text
feature/<nom-de-la-feature>
```

Exemples :

```text
feature/user-authentication
feature/payment-history
feature/export-pdf
```

### 3. Développer et commiter

```bash
git add .
git commit -m "feat: add user authentication"
```

Pendant le développement :

- [ ] Faire des commits petits et cohérents.
- [ ] Utiliser des messages de commit explicites.
- [ ] Ne jamais commiter de secrets.
- [ ] Ajouter ou mettre à jour les tests si nécessaire.
- [ ] Vérifier régulièrement que la branche reste compatible avec `develop`.

### 4. Publier la branche

```bash
git push -u origin feature/user-authentication
```

### 5. Créer une Pull Request

```text
feature/user-authentication
             │
             │ Pull Request
             ▼
          develop
```

Avant le merge :

- [ ] La PR cible `develop`.
- [ ] Le ticket associé est référencé.
- [ ] La description explique les changements.
- [ ] Aucun changement hors périmètre n'est présent.
- [ ] La CI est verte.
- [ ] Les tests passent.
- [ ] La review est validée.
- [ ] Les commentaires bloquants sont résolus.
- [ ] Aucun conflit Git n'est présent.

Après le merge :

- [ ] Supprimer la branche `feature/*`.

---

## 🐛 Corriger un bug hors production

Pour un bug constaté sur la prochaine version, utiliser une branche `bugfix/*` créée depuis `develop`.

```bash
git switch develop
git pull origin develop

git switch -c bugfix/incorrect-total
```

Une fois la correction terminée :

```text
bugfix/incorrect-total
          │
          │ Pull Request
          ▼
       develop
```

Avant le merge :

- [ ] La correction est limitée au bug concerné.
- [ ] Les tests existants passent.
- [ ] Un test couvrant le bug a été ajouté lorsque cela est pertinent.
- [ ] La CI est verte.
- [ ] La Pull Request est validée.

> **Important :** un problème critique déjà présent en production doit utiliser un `hotfix/*`, et non un `bugfix/*`.

---

## 📦 Préparer une release

Lorsqu'une version est prête à être stabilisée, créer une branche `release/*` depuis `develop`.

Exemple pour *a version `2.4.0` :

```bash
git s*itch develop
git pull origin devel*p

git switch -c release/2.4.0
git*push -u origin release/2.4.0
```

*e workflow devient :

```text
deve*op ───────────────────────→ procha*ne version
     \
      release/2.*.0 ───────────→ stabilisation
```
*### Checklist de création

- [ ] T*utes les features prévues sont dan* `develop`.
- [ ] La CI de `develo*` est verte.
- [ ] Les tests passe*t.
- [ ] Aucun ticket bloquant con*u n'est ouvert.
- [ ] Le numéro de*version est défini.
- [ ] `release*<version>` est créée depuis `devel*p`.

---

## 🧪 Valider une releas*

Pendant la stabilisation :

- [ * Vérifier le numéro de version.
- * ] Exécuter les tests unitaires.
-*[ ] Exécuter les tests d'intégrati*n.
- [ ] Exécuter les tests foncti*nnels.
- [ ] Tester les éventuelle* migrations.
- [ ] Déployer la rel*ase en recette/staging.
- [ ] Corr*ger les bugs bloquants directement*sur `release/*`.
- [ ] Préparer le* release notes.
- [ ] Obtenir la v*lidation QA/métier.

> ⚠️ **Aucune*nouvelle fonctionnalité ne doit êt*e ajoutée à une branche `release/**.**

Les nouvelles fonctionnalités*continuent leur développement à pa*tir de `develop` pour une prochain* release.

---

## 🏷️ Livrer une *elease

Une release validée doit ê*re mergée dans **`main` et `develo*`**.

```text
                 rel*ase/2.4.0
                    /   *   \
                   ▼         *
                 main     develop*                   │
             *     ▼
                v2.4.0
```
*### Merge vers `main`

Créer une P*ll Request :

```text
release/2.4.*
      ↓
     main
```

Une fois l* Pull Request validée et mergée, m*ttre `main` à jour :

```bash
git *witch main
git pull origin main
``*

Puis créer le tag correspondant * la version :

```bash
git tag -a *2.4.0 -m "Release 2.4.0"
git push *rigin v2.4.0
```

### Synchronisat*on avec `develop`

Créer également*une Pull Request :

```text
releas*/2.4.0
      ↓
   develop
```

Cel* permet de récupérer dans `develop* les éventuelles corrections réali*ées pendant la stabilisation.

###*Checklist de livraison

- [ ] `rel*ase/*` est mergée dans `main`.
- [*] La CI de `main` est verte.
- [ ]*Le tag `vX.Y.Z` est créé.
- [ ] La*release est déployée en production*
- [ ] `release/*` est également m*rgée dans `develop`.
- [ ] Les cor*ections de recette sont présentes *ans `develop`.
- [ ] Les release n*tes sont publiées.
- [ ] La branch* `release/*` est supprimée.

---

*# 🚨 Corriger un problème critique*en production

Un `hotfix` est uti*isé pour corriger rapidement un pr*blème critique déjà présent en pro*uction.

Un hotfix doit toujours p*rtir de `main`.

### 1. Créer le h*tfix

Mettre `main` à jour :

```b*sh
git switch main
git pull origin*main
```

Créer ensuite la branche*:

```bash
git switch -c hotfix/2.*.1
```

### 2. Effectuer la correc*ion

```bash
git add .
git commit *m "fix: correct production issue"
*git push -u origin hotfix/2.4.1
``*

Checklist :

- [ ] Limiter la mo*ification au strict nécessaire.
- * ] Ajouter ou corriger les tests.
* [ ] Vérifier les régressions pote*tielles.
- [ ] Faire une code revi*w.
- [ ] Vérifier la CI.
- [ ] Val*der le correctif avant la mise en *roduction.

### 3. Livrer le hotfi*

Le hotfix doit être intégré dans*`main` **et** `develop`.

```text
*                      hotfix/2.4.1*                       /          \
                      ▼            ▼
                    main        develop
                      │
                      ▼
                   v2.4.1
```

Checklist :

- [ ] Merger `hotfix/*` dans `main`.
- [ ] Vérifier que la CI de `main` est verte.
- [ ] Créer le nouveau tag.

Exemple :

```bash
git switch main
git pull origin main

git tag -a v2.4.1 -m "Release 2.4.1"
git push origin v2.4.1
```

Puis :

- [ ] Déployer le correctif en production.
- [ ] Merger `hotfix/*` dans `develop`.
- [ ] Vérifier que `develop` contient bien le correctif.
- [ ] Supprimer la branche `hotfix/*`.

> **Important :** ne jamais o*blier le merge vers `develop`. San* celui-ci, le correctif de product*on pourrait disparaître lors d'une*future release.

---

## 🔐 Protec*ion des branches

### `main`

La b*anche `main` doit être protégée :
*- [ ] Push direct interdit.
- [ ] *ull Request obligatoire.
- [ ] CI *bligatoire.
- [ ] Tests obligatoir*s.
- [ ] Review obligatoire.
- [ ] Conversations bloquantes résolues avant merge.
- [ ] Branche protégée contre la suppression.

### `develop`

La branche `develop` doit idéalement appliquer les mêmes protections :

- [ ] Push direct interdit.
- [ ] Pull Request obligatoire.
- [ ] CI obligatoire.
- [ ] Tests obligatoires.
- [ ] Review obligatoire.
- [ ] Conversations bloquantes résolues avant merge.

---

## 🤖 CI/CD

Le pipeline CI/CD recommandé suit les branches GitFlow :

```text
feature/* / bugfix/*
        │
        ▼
  Build + Tests
  Quality checks


develop
   │
   ▼
Build + Tests
   │
   ▼
  DEV


release/*
   │
   ▼
Build + Tests
   │
   ▼
RECETTE / STAGING


main + tag
   │
   ▼
Build / Release
   │
   ▼
PRODUCTION
```

### Comportement attendu

#### `feature/*` et `bugfix/*`

```text
Commit / Push
     ↓
Build
     ↓
Tests
     ↓
Quality checks
```

Ces branches ne déclenchent pas de déploiement en production.

#### `develop`

```text
Merge
  ↓
Build
  ↓
Tests
  ↓
Déploiement DEV
```

#### `release/*`

```text
Merge / Push
     ↓
   Build
     ↓
   Tests
     ↓
Déploiement RECETTE / STAGING
```

#### `main`

```text
Merge release/* ou hotfix/*
             ↓
           Build
             ↓
           Tests
             ↓
             Tag
             ↓
         PRODUCTION
```

---

## 🧭 Résumé du workflow

### Feature

```text
develop
   ↓
feature/*
   ↓
Pull Request
   ↓
develop
```

### Bugfix

```text
develop
   ↓
bugfix/*
   ↓
Pull Request
   ↓
develop
```

### Release

```text
develop
   ↓
release/*
   ├────────→ main → tag → PRODUCTION
   │
   └────────→ develop
```

### Hotfix

```text
main
 ↓
hotfix/*
 ├────────→ main → tag → PRODUCTION
 │
 └────────→ develop
```

### Vue globale

```text
                       feature/*
                      /         \
                     /           \
develop ────────────●─────────────●────────●─────────────→
                                          \
                                           \
                                        release/*
                                         /      \
                                        ▼        \
                                      main        \
                                        │          \
                                        ▼           ▼
                                      vX.Y.Z     develop
                                        │
                                        │
                                  production issue
                                        │
                                        ▼
                                    hotfix/*
                                     /     \
                                    ▼       ▼
                                  main   develop
                                    │
                                    ▼
                                 vX.Y.Z+1
```

---

## ⭐ Les 5 règles à retenir

> **1.** Une feature part toujours de `develop`.
>
> **2.** Une feature retourne toujours dans `develop`.
>
> **3.** Une release part de `develop` et termine dans `main` **et** `develop`.
>
> **4.** Un hotfix part de `main` et termine dans `main` **et** `develop`.
>
> **5.** `main` représente la production et chaque release est identifiée par un tag `vX.Y.Z`.

En résumé :

```text
Nouvelle fonctionnalité
develop → feature/* → develop


Bug hors production
develop → bugfix/* → develop


Nouvelle version
develop → release/* → main
                    ↘ develop


Bug critique en production
main → hotfix/* → main
               ↘ develop
```

---

## ✅ Checklist rapide avant une Pull Request

Avant de demander une review :

- [ ] Mon code compile.
- [ ] Mes tests passent.
- [ ] J'ai ajouté les tests nécessaires.
- [ ] Je n'ai pas commité de secrets ou de fichiers temporaires.
- [ ] Ma branche est à jour.
- [ ] Ma PR cible la bonne branche.
- [ ] Mon ticket est référencé.
- [ ] Le périmètre de ma PR reste limité.
- [ ] La CI est verte.
- [ ] La description de la PR permet au reviewer de comprendre les changements.
- [ ] J'ai vérifié les éventuelles régressions.
- [ ] Aucun conflit Git n'est présent.

---

## 📌 Aide-mémoire

| Type | Branche source | Branche cible | Usage |
|---|---|---|---|
| `feature/*` | `develop` | `develop` | Nouvelle fonctionnalité |
| `bugfix/*` | `develop` | `develop` | Bug hors production |
| `release/*` | `develop` | `main` + `develop` | Préparation d'une release |
| `hotfix/*` | `main` | `main` + `develop` | Correction urgente en production |

**Règle fondamentale : `main` doit toujours représenter l'état de la production.**
