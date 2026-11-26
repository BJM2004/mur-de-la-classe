# Cloner le projet et l'envoyer sur SON propre GitHub

Objectif : récupérer le projet du prof sur ton ordinateur, puis l'envoyer dans **ton propre dépôt GitHub** (une copie qui t'appartient).

> Dans toutes les commandes, remplace `PSEUDO` par **ton pseudo GitHub**.
> Le projet du prof : `https://github.com/AYMERIC96/mur-de-la-classe`

---

## Avant de commencer

Il te faut :
- un compte GitHub
- Git installé
- VS Code (ou un autre éditeur)

Vérifie que Git est installé :
```bash
git --version
```

Configure ton identité (une seule fois sur l'ordinateur) :
```bash
git config --global user.name "Prénom Nom"
git config --global user.email "email-de-ton-compte-github@exemple.com"
```
*Pourquoi ?* Git signe chaque commit avec ce nom et cet email. L'email doit être celui de ton compte GitHub.

---

## Étape 1 : Créer ton dépôt VIDE sur GitHub

1. Va sur https://github.com et connecte-toi.
2. Clique sur **+** (en haut à droite), puis **New repository**.
3. **Repository name** : `mur-de-la-classe`
4. Visibilité : **Public**
5. **Important : ne coche RIEN** (ni README, ni .gitignore, ni licence).
6. Clique sur **Create repository**.

*Pourquoi vide ?* Si GitHub crée un README, ton dépôt aura un historique différent de celui du prof et l'envoi sera refusé.

Tu obtiens l'adresse de ton dépôt : `https://github.com/PSEUDO/mur-de-la-classe.git`

---

## Étape 2 : Choisir un bon dossier sur ton ordinateur

```bash
cd Documents
```

Vérifie que ce dossier n'est **pas déjà un projet Git** :
```bash
git status
```
- `fatal: not a git repository` : parfait, continue.
- `On branch ...` : tu es déjà dans un projet Git. Choisis un autre dossier, sinon tu auras l'erreur "embedded git repository".

---

## Étape 3 : Cloner le projet du prof

```bash
git clone https://github.com/AYMERIC96/mur-de-la-classe.git
cd mur-de-la-classe
```

*Explication :*
- `git clone` télécharge tout le projet et son historique.
- `cd mur-de-la-classe` te place dans le dossier du projet. **Toutes les commandes suivantes se tapent ici.**

Vérifie :
```bash
git remote -v
```
Tu vois `origin` qui pointe vers le dépôt du prof (`AYMERIC96`).

---

## Étape 4 : Renommer `origin` en `upstream`

```bash
git remote rename origin upstream
```

*Explication :* un "remote" est une adresse GitHub enregistrée dans ton projet. Pour l'instant, `origin` pointe vers le dépôt du prof. On le renomme `upstream` (= "la source") pour libérer le nom `origin`, qu'on réserve à **ton** dépôt.

Vérifie :
```bash
git remote -v
```
Tu vois maintenant `upstream` vers `AYMERIC96`.

---

## Étape 5 : Ajouter TON dépôt comme `origin`

```bash
git remote add origin https://github.com/PSEUDO/mur-de-la-classe.git
git remote -v
```

*Explication :* tu as maintenant deux adresses :

| Nom | Pointe vers | Rôle |
|---|---|---|
| `origin` | **ton** dépôt | là où tu envoies ton travail |
| `upstream` | le dépôt du prof | là où tu récupères les nouveautés |

Vérifie que ton pseudo apparaît bien dans l'URL de `origin`.

---

## Étape 6 : Vérifier ta branche

```bash
git branch
```
La branche avec l'étoile `*` est ta branche actuelle (en général `main`).

Si elle s'appelle autrement (ex. `master`), renomme-la :
```bash
git branch -M main
```

---

## Étape 7 : Envoyer le projet sur ton GitHub

```bash
git push -u origin main
```

*Explication :*
- `git push` envoie ton projet vers GitHub.
- `origin` = ton dépôt.
- `main` = la branche envoyée.
- `-u` mémorise ce choix : ensuite, un simple `git push` suffira.

**Connexion :** une fenêtre de navigateur peut s'ouvrir pour te connecter à GitHub. Accepte. Si on te demande un mot de passe dans le terminal, ce n'est **pas** ton mot de passe GitHub mais un **token d'accès personnel** (GitHub → Settings → Developer settings → Personal access tokens).

---

## Étape 8 : Vérifier sur GitHub

1. Ouvre `https://github.com/PSEUDO/mur-de-la-classe`.
2. Actualise la page (`F5`).
3. Tu dois voir les fichiers du projet (`index.html`, `README.md`...).

Le projet est maintenant **dans ton compte**.

---

## Étape 9 : Récupérer les mises à jour du prof (plus tard)

```bash
git pull upstream main
git push origin main
```

*Explication :*
- `git pull upstream main` récupère les nouveautés du prof.
- `git push origin main` les envoie dans ton dépôt.

---

## Aide-mémoire

| Je veux... | Commande |
|---|---|
| Cloner le projet du prof | `git clone https://github.com/AYMERIC96/mur-de-la-classe.git` |
| Voir mes adresses | `git remote -v` |
| Renommer origin en upstream | `git remote rename origin upstream` |
| Ajouter mon dépôt | `git remote add origin https://github.com/PSEUDO/mur-de-la-classe.git` |
| Renommer ma branche en main | `git branch -M main` |
| Envoyer sur mon GitHub | `git push -u origin main` |
| Récupérer les nouveautés du prof | `git pull upstream main` |

---

## Erreurs fréquentes

| Message | Cause | Solution |
|---|---|---|
| `remote origin already exists` | Étape 4 oubliée | Faire `git remote rename origin upstream`, puis `git remote add origin ...` |
| `Repository not found` | Pseudo mal écrit ou dépôt non créé | Vérifier l'URL avec `git remote -v` |
| `rejected ... fetch first` | Ton dépôt n'était pas vide (README coché) | Supprimer le dépôt sur GitHub, le recréer vide, refaire l'étape 7 |
| `src refspec main does not match any` | Ta branche ne s'appelle pas `main` | `git branch -M main` puis refaire le push |
| `Permission denied` / `403` | Tu pousses vers le dépôt du prof | Vérifier que `origin` contient **ton** pseudo |
| `Author identity unknown` | Identité Git non configurée | Refaire les `git config --global` du début |
| `embedded git repository` | Clone dans un dossier déjà géré par Git | Cloner dans un autre dossier |

---

## Bon à savoir

Ce dépôt est une **copie indépendante**, pas un fork. Tu peux y travailler librement, mais tu ne pourras pas ouvrir de Pull Request vers le dépôt du prof depuis cette copie. Pour proposer une modification au prof, il faudra passer par un **Fork** (étape suivante du cours).