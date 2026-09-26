# 🎓 Exercice Git & GitHub — Guide complet pour débutant

Ce dépôt contient le support de l'atelier :

- **`index.html`** — la page interactive du guide (recherche, mode sombre, sommaire, boutons "copier"). Ouvre-la avec **VS Code + Live Server** pour la consulter pendant l'exercice.
- **`README.md`** — ce fichier : le même contenu que l'exercice, en texte simple, pour référence ou impression.

### 🎯 Ce que tu vas apprendre

```
Fichiers locaux → Git (suivi local) → GitHub (sauvegarde distante) → Collaboration
```

### 💻 Environnement

Cet exercice est prévu pour **Windows**, avec **Git Bash** (installé avec Git) ou le terminal intégré de **VS Code** (Terminal → New Terminal → profil "Git Bash"). Toutes les commandes ci-dessous (`echo`, `cat`, `ls`, `mkdir`, `rm`...) fonctionnent telles quelles dans ces deux terminaux.

---

## Partie 1 — Installation et configuration

### Étape 1 — Vérifie si Git est installé

```bash
git --version
```

Si Git est absent, installe-le avec `winget` (PowerShell) ou via l'installeur officiel sur [git-scm.com](https://git-scm.com/download/win) :

```powershell
winget install --id Git.Git -e --source winget
```

> Il n'y a pas de `apt`/`apt-get` sous Windows — c'est un gestionnaire de paquets Linux.

### Étape 2 — Configure ton identité (une seule fois, pour toujours)

```bash
git config --global user.name "Ton Nom"
git config --global user.email "ton.email@example.com"
```

### Étape 3 — Vérifie la configuration

```bash
git config --list
```

---

## Partie 2 — Créer ton premier dépôt Git (local)

### Étape 1 — Crée un dossier de travail

```bash
mkdir mon-premier-projet
cd mon-premier-projet
```

### Étape 2 — Initialise Git dans ce dossier

```bash
git init
```

👉 Ça crée un dossier caché `.git/` qui va suivre tout l'historique de tes fichiers. Vérifie :

```bash
ls -la
```

### Étape 3 — Crée un premier fichier

```bash
echo "# Mon premier projet" > README.md
```

### Étape 4 — Vérifie l'état de ton dépôt

```bash
git status
```

👉 Git te dit que `README.md` est **"untracked"** (non suivi) — il existe mais Git ne le surveille pas encore.

---

## Partie 3 — Le cycle de base : add → commit

### Étape 1 — Ajoute le fichier à la "zone de préparation" (staging area)

```bash
git add README.md
git status
```

👉 Le fichier passe en vert, "Changes to be committed" — il est prêt à être enregistré, mais **pas encore enregistré**.

### Étape 2 — Enregistre les changements (commit)

```bash
git commit -m "Ajout du README initial"
```

👉 C'est ici que Git **enregistre définitivement** un instantané de ton projet, avec un message expliquant le changement.

### Étape 3 — Consulte l'historique

```bash
git log
git log --oneline
```

👉 Chaque commit a un identifiant unique (hash) — c'est le "point de sauvegarde" auquel tu pourras toujours revenir.

---

## Partie 4 — Modifier, comparer, recommencer

### Étape 1 — Modifie le fichier

```bash
echo "Ceci est mon tout premier projet Git." >> README.md
```

### Étape 2 — Vois ce qui a changé exactement

```bash
git diff
```

👉 Affiche les lignes ajoutées (`+`) / supprimées (`-`) avant même de commit — très utile pour vérifier ce que tu es sur le point d'enregistrer.

### Étape 3 — Ajoute et commit à nouveau

```bash
git add README.md
git commit -m "Ajout d'une description dans le README"
```

### Étape 4 — Ajoute un deuxième fichier

```bash
echo "recette : pâtes carbonara" > recette.txt
git add recette.txt
git commit -m "Ajout d'une recette"
```

### Étape 5 — Regarde l'historique complet

```bash
git log --oneline
```

Tu dois voir 3 commits maintenant.

---

## Partie 5 — Annuler des erreurs (indispensable à savoir)

> ⚠️ Les commandes d'annulation peuvent supprimer des changements. Utilise-les seulement quand tu comprends ce qu'elles font.

### Annuler une modification non commitée (revenir à la dernière version enregistrée)

```bash
echo "erreur test" >> README.md
git status
git checkout -- README.md
cat README.md
```

👉 Le fichier revient à son état du dernier commit — la ligne "erreur test" a disparu.

### Revenir à un commit précédent (annuler le dernier commit)

```bash
git log --oneline
git reset --soft HEAD~1
```

👉 `--soft` annule le commit mais garde les modifications prêtes à recommit — utile si tu t'es juste trompé de message.

---

## Partie 6 — Créer un compte GitHub et connecter ton dépôt

### Étape 1 — Crée un compte

Va sur [github.com](https://github.com) et inscris-toi (gratuit).

### Étape 2 — Crée un nouveau dépôt vide

Sur GitHub : bouton **"+"** en haut à droite → **New repository**

- Nom : `mon-premier-projet`
- **Ne coche rien** (pas de README, pas de .gitignore — on a déjà notre projet local)
- Clique **Create repository**

### Étape 3 — Configure l'authentification (obligatoire depuis 2021)

GitHub n'accepte plus les mots de passe classiques pour Git. Deux options :

**Option A — HTTPS avec token**

1. GitHub → ton profil → **Settings → Developer settings → Personal access tokens → Tokens (classic) → Generate new token**
2. Coche la case `repo`, génère, copie le token (tu ne le reverras plus)

**Option B — SSH (plus pratique à long terme)**

```bash
ssh-keygen -t ed25519 -C "ton.email@example.com"
cat ~/.ssh/id_ed25519.pub
```

Copie la clé affichée → GitHub → **Settings → SSH and GPG keys → New SSH key** → colle-la.

### Étape 4 — Relie ton dépôt local à GitHub

**Si tu utilises HTTPS :**

```bash
git remote add origin https://github.com/TON-USERNAME/mon-premier-projet.git
```

**Si tu utilises SSH :**

```bash
git remote add origin git@github.com:TON-USERNAME/mon-premier-projet.git
```

### Étape 5 — Vérifie la connexion

```bash
git remote -v
```

### Étape 6 — Envoie tes commits vers GitHub

```bash
git branch -M main
git push -u origin main
```

### Étape 7 — Vérifie sur GitHub

Rafraîchis la page de ton dépôt sur github.com — tu dois voir `README.md` et `recette.txt`.

---

## Partie 7 — Le cycle quotidien, et la différence entre push et pull

```bash
git status              # voir ce qui a changé
git add <fichier>       # préparer les changements
git commit -m "message" # enregistrer localement
git push                # envoyer sur GitHub
```

### Exercice pratique

```bash
echo "recette : tarte aux pommes" >> recette.txt
git add recette.txt
git commit -m "Ajout d'une deuxième recette"
git push
```

### Étape 1 — Modifie un fichier directement sur GitHub

1. Va sur ton dépôt `mon-premier-projet` sur github.com
2. Clique sur le fichier `recette.txt`
3. Clique sur l'icône **crayon** (Edit this file) en haut à droite du fichier
4. Ajoute une ligne : `recette : quiche lorraine`
5. Descends en bas de page → dans la zone **"Commit changes"** : titre `Ajout d'une recette depuis GitHub`
6. Clique **Commit changes** (directement sur la branche `main`)

👉 Tu viens de créer un commit **directement sur GitHub**, sans jamais toucher à ta VM/PC.

### Étape 2 — Retourne sur ta machine locale (sans avoir encore synchronisé)

```bash
cd ~/mon-premier-projet
cat recette.txt
```

👉 La ligne "quiche lorraine" **n'apparaît pas encore** ici — ton dépôt local ne sait rien du changement fait sur GitHub.

### Étape 3 — Récupère le changement avec `git pull`

```bash
git pull
```

Tu dois voir un résultat du type :

```
remote: Enumerating objects...
Unpacking objects: 100%
From https://github.com/TON-USERNAME/mon-premier-projet
   a1b2c3d..e4f5g6h  main       -> origin/main
Updating a1b2c3d..e4f5g6h
Fast-forward
 recette.txt | 1 +
 1 file changed, 1 insertion(+)
```

Vérifie sur GitHub que le changement apparaît.

---

## Partie 8 — Récupérer un projet existant (clone), contribuer, puis personnaliser

### 8.1 — Contribuer au dépôt commun

Le dépôt partagé pour cet exercice est [`EYAannabi/Linktree`](https://github.com/EYAannabi/Linktree.git) — remplace-le par ton propre dépôt si tu fais l'exercice en dehors de l'atelier.

Choisis un **participant contributeur**. Les autres sont les **participants qui synchronisent**.

**Le contributeur clone le projet dans un dossier séparé :**

```bash
cd ~
git clone https://github.com/EYAannabi/Linktree.git Linktree-contributeur
cd Linktree-contributeur
```

**Le contributeur ajoute une box GitHub.** Dans `index.html`, ajoute ce bloc juste avant `</div>`, la fermeture de la zone `.links` :

```html
<a href="#" class="link-box github">
  <span class="icon">💻</span> GitHub
</a>
```

> Le fichier `style.css` contient déjà la classe `.github` : la box sera automatiquement stylée.

**Le contributeur enregistre et partage sa contribution :**

```bash
git status
git add index.html
git commit -m "Ajout de la box GitHub"
git push
```

**Les autres participants récupèrent la contribution**, dans leur propre clone de `Linktree` :

```bash
cd ~/Linktree
git pull
cat index.html
```

👉 Tu dois maintenant voir apparaître la nouvelle box GitHub dans ta copie locale.

| | |
|---|---|
| **Push** | Le contributeur envoie sa contribution de son clone vers GitHub. |
| **Pull** | Les autres participants récupèrent cette contribution dans leur clone. |

### 8.2 — Personnalise TA propre version (locale, non partagée)

C'est ici la nuance importante : **on ne pousse plus les liens personnels sur le dépôt commun** — chacun garde sa version en local uniquement, puis déploie **séparément** avec GitHub Pages.

**Personnalise ta copie locale.** Dans `~/Linktree/index.html`, remplace les `href="#"` par tes vrais liens :

```html
<a href="https://facebook.com/tonprofil" class="link-box facebook">
<a href="https://instagram.com/tonprofil" class="link-box instagram">
<a href="https://linkedin.com/in/tonprofil" class="link-box linkedin">
<a href="https://github.com/tonprofil" class="link-box github">
```

> ⚠️ **Ne fais PAS `git push` de ce changement** sur le dépôt commun — c'est ta personnalisation privée.

**Déploie TA version avec GitHub Pages, sur TON propre dépôt.** Crée d'abord un nouveau dépôt vide `mes-liens-perso` sur GitHub (comme à l'étape 2 de la Partie 6), puis :

```bash
cd ~/Linktree
git remote remove origin
git remote add origin https://github.com/TON-USERNAME/mes-liens-perso.git
git add .
git commit -m "Ma version personnalisee"
git push -u origin main
```

**Active GitHub Pages** : `Settings → Pages` → branche `main`, dossier `/ (root)` → **Save**.

**Accède à ton site en ligne**, disponible après 1-2 minutes sur :

```
https://TON-USERNAME.github.io/mes-liens-perso/
```

---

## Partie 9 — Les branches : diverger, fusionner, résoudre les conflits

### 9.1 — Crée une branche et pousse-la

```bash
cd ~/mon-premier-projet
git checkout -b nouvelle-fonctionnalite
echo "recette : soupe à l'oignon" >> recette.txt
git add recette.txt
git commit -m "Nouvelle recette sur une branche séparée"
git push -u origin nouvelle-fonctionnalite
```

> `origin` = le nom de ton remote (GitHub), `nouvelle-fonctionnalite` = le nom de la branche côté GitHub. `-u` (upstream) lie ta branche locale à sa version distante, pour que les prochains `git push`/`git pull` fonctionnent sans repréciser `origin nouvelle-fonctionnalite` à chaque fois.

### 9.2 — Visualise où tu en es

```bash
git log --oneline --graph --all
```

```
* 1c15b73 (HEAD -> nouvelle-fonctionnalite) add pasta recipie
* 818fa1f Nouvelle recette sur une branche séparée
* 1738cc9 (origin/main) Add quiche lorraine to recette.txt
* 674e5f6 Ajout d'une deuxième recette
...
```

👉 `nouvelle-fonctionnalite` a **2 commits d'avance** sur `main`.

### 9.3 — Vérifie que les branches sont isolées

```bash
git checkout main
cat recette.txt
```

👉 Ni "pasta recipie" ni "soupe à l'oignon" n'apparaissent ici — la preuve que les branches sont bien **isolées**.

### 9.4 — Fais évoluer `main` en parallèle (pour créer une vraie divergence)

```bash
echo "recette : ratatouille" >> recette.txt
git add recette.txt
git commit -m "Ajout ratatouille sur main"
git log --oneline --graph --all
```

```
* a1b2c3d (HEAD -> main) Ajout ratatouille sur main
| * 1c15b73 (nouvelle-fonctionnalite) add pasta recipie
| * 818fa1f Nouvelle recette sur une branche séparée
|/
* 1738cc9 (origin/main) Add quiche lorraine to recette.txt
...
```

👉 Deux lignes de développement séparées, à partir du même point de départ.

### 9.5 — Fusionne `nouvelle-fonctionnalite` dans `main`

```bash
git diff main nouvelle-fonctionnalite
git merge nouvelle-fonctionnalite
```

Si un conflit apparaît (lignes qui se chevauchent), tu verras :

```
<<<<<<< HEAD
recette : ratatouille
=======
recette : pasta
>>>>>>> nouvelle-fonctionnalite
```

Résous-le à la main (garde ce que tu veux, supprime les marqueurs), puis :

```bash
git add recette.txt
git commit -m "Fusion de nouvelle-fonctionnalite dans main"
```

### 9.6 — Vérifie le résultat final

```bash
git log --oneline --graph --all
cat recette.txt
```

👉 Toutes les recettes des deux branches sont maintenant réunies dans `main`.

### 9.7 — Pousse tout vers GitHub

```bash
git push
git push -u origin nouvelle-fonctionnalite   # si pas encore fait
```

### 9.8 — Nettoie la branche fusionnée

```bash
git branch -d nouvelle-fonctionnalite
```

👉 `-d` refuse de supprimer si la branche a des commits non fusionnés (sécurité) — signe que ton merge a bien fonctionné si ça passe sans erreur.

---

## Partie 10 — Protéger les secrets avec `.gitignore`

### Étape 1 — Sans `.gitignore`, observe le problème

```bash
echo "DB_PASSWORD=supersecret123" > config.env
git status
```

👉 `config.env` apparaît comme fichier **non suivi**. Si tu fais `git add .` maintenant, il serait ajouté au dépôt et **visible publiquement** sur GitHub — grave erreur de sécurité si tu push.

### Étape 2 — Crée le `.gitignore`

```bash
echo "config.env" > .gitignore
git status
```

👉 `config.env` n'apparaît plus du tout — Git l'ignore complètement. Seul `.gitignore` apparaît comme nouveau fichier.

### Étape 3 — Commit le `.gitignore` (mais jamais le fichier secret)

```bash
git add .gitignore
git commit -m "Ajout du gitignore pour proteger config.env"
git push
```

### Étape 4 — Teste : même en forçant, le fichier reste protégé

```bash
git add .
git status
```

👉 `git add .` (qui ajoute normalement TOUT) **ignore quand même** `config.env`, grâce au `.gitignore`.

### Étape 5 — Vérifie sur GitHub

Va sur ton dépôt en ligne — tu verras `.gitignore` listé, mais **`config.env` n'existe nulle part** sur GitHub, alors qu'il existe bel et bien sur ton disque local.

### Quelques règles utiles en plus

```
*.log
__pycache__/
.env
config.env
node_modules/
```

> ⚠️ Si un secret a déjà été envoyé sur GitHub, le supprimer du fichier ne suffit pas : considère-le comme compromis et change-le immédiatement.

---

## 🗺️ Récapitulatif visuel

```
Dossier de travail (fichiers modifiés)
        │  git add
        ▼
Zone de préparation (staging)
        │  git commit
        ▼
Dépôt local (.git/, historique)
        │  git push
        ▼
GitHub (dépôt distant, sauvegarde + partage)
```

## 📋 Aide-mémoire des commandes

| Commande | Ce qu'elle fait |
|---|---|
| `git status` | Voir ce qui a changé et ce qui est préparé |
| `git add <fichier>` | Préparer un fichier pour le prochain commit |
| `git commit -m "message"` | Enregistrer les changements préparés, localement |
| `git push` | Envoyer les commits locaux vers GitHub |
| `git pull` | Récupérer et fusionner les changements distants |
| `git clone <url>` | Télécharger un dépôt en local |
| `git log --oneline --graph --all` | Visualiser l'historique et les branches |
| `git diff` | Voir les changements non préparés |
| `git checkout -b <nom>` | Créer une branche et basculer dessus |
| `git merge <branche>` | Fusionner une branche dans la branche actuelle |
| `git branch -d <nom>` | Supprimer une branche déjà fusionnée |
| `git remote add origin <url>` | Relier un dépôt local à un dépôt GitHub |
| `git reset --soft HEAD~1` | Annuler le dernier commit, garder les changements préparés |

## 🧹 Nettoyage de l'exercice

```bash
cd ~
rm -rf mon-premier-projet Linktree-contributeur Linktree
```

> Sous Windows, `rm -rf` fonctionne dans Git Bash. En PowerShell, utilise plutôt `Remove-Item -Recurse -Force mon-premier-projet`.

Le dépôt sur GitHub reste — supprime-le manuellement via **Settings → Danger Zone** si tu veux tout effacer.
