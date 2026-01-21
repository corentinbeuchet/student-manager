# Partie 1 – Initialisation du repository

## Étape 0: Configuration de Git <span style="color:green">[TOUTE L'ÉQUIPE]</span>
Si vous ne l'avez pas déjà fait, configurez Git pour ce projet.
```bash
git config --global user.name "Prénom NOM"
git config --global user.email "p.nom@lyon.ort.asso.fr"
```

## Étape 1: Créer un nouveau repository <span style="color:red">[Étudiant A]</span>

### 1. Créer un nouveau repository appelé 
`student-manager`

### 2. Clôner ce project en local
```bash
git clone https://github.com/corentinbeuchet/student-manager.git
cd student-manager
````

### 3. Vérifier que Node.js est bien installé
Lancer la commande `node index.js` et vérifier que le projet fonctionne.

### 4. Premier commit
```bash
git init
git add .
git commit -m "chore: Initial commit"
git remote add origin [Chemin vers le repo git créé].git
git push -u origin main
```

## Étape 2: Créer les premiers fichiers <span style="color:green">[Étudiant B]</span>

### 1. Récupérer le projet depuis le repo git de l'étudiant A
```bash
git clone [Chemin vers le repo git créé].git
cd student-manager
```

### 2. Modifier le contenu des fichiers
Modifier [README.md](README.md)
en ajoutant le contenu suivant:
```markdown
# Student Manager (JavaScript)

Simple Node.js command-line app to manage students.
```

### 2. Faire un commit et un push
```bash
git init
git add README.md
git commit -m "feat: Modification du README.md"
git remote add origin [Chemin vers le repo git créé].git
git push -u origin main
```

## Étape 3: Développement en parallèle

### 1. Feature : supprimer un étudiant <span style="color:red">[Étudiant A]</span>
#### I. Création d'une branche
```bash
git switch -c feat/remove-student
```
#### II. Modifier le fichier [students.js](src/students.js)
```javascript
const fs = require("fs");


function loadStudents() {
    return fs.readFileSync("./src/data.txt", "utf-8").split("\n").filter(Boolean);
}


function showStudents() {
    const students = loadStudents();
    console.log("Students:");
    students.forEach(s => console.log("-", s));
}

function removeStudent(name) {
    const students = loadStudents().filter(s => s.trim() !== name);
    fs.writeFileSync("./src/data.txt", students.join("\n") + "\n");
}

module.exports = { showStudents, removeStudent };
```
#### III. Modifier le fichier [index.js](index.js)
```javascript
const { showStudents, removeStudent } = require('./src/students');


showStudents();
removeStudent('Alice');
showStudents();
```
#### IV. Vérifier le bon fonctionnement de la feature
Lancer la commande `node index.js` et vérifier que la feature fonctionne.

#### V. Push nos modifications
```bash
git add src/students.js index.js
git commit -m "feat: remove student"
git push origin feat/remove-student
```
### 2. Feature : ajouter un étudiant <span style="color:green">[Étudiant B]</span>
#### I. Création d'une branche
```bash
git switch -c feat/add-student
```
#### II. Modifier le fichier [students.js](src/students.js)
```javascript
const fs = require("fs");


function loadStudents() {
    return fs.readFileSync("./src/data.txt", "utf-8").split("\n").filter(Boolean);
}

function showStudents() {
    const students = loadStudents();
    console.log("Students:");
    students.forEach(s => console.log("-", s));
}

function addStudent(name) {
    fs.appendFileSync("./src/data.txt",  "\n" + name);
}

module.exports = { showStudents, addStudent };
```
#### III. Modifier le fichier [index.js](index.js)
```javascript
const { showStudents, addStudent } = require('./src/students');


showStudents();
addStudent('Marc');
showStudents();
```
#### IV. Vérifier le bon fonctionnement de la feature
Lancer la commande `node index.js` et vérifier que la feature fonctionne.

#### V. Push nos modifications
```bash
git add src/students.js index.js
git commit -m "feat: add student"
git push origin feat/add-student
```

### 3. Merge <span style="color:red">[Étudiant A]</span>
#### I. Switch sur main et récupérer les modification sur le repository distant
```bash
git switch main
git pull origin main
```
#### II. Merge la branche de l'édutiant B
```bash
git merge feat/add-student
```

#### III. Résoudre les conflits et push sur main
```bash
git commit
git push origin main
```

### 3. Rebase <span style="color:red">[Étudiant B]</span>
#### I. Switch sur main et récupérer les modification sur le repository distant
```bash
git switch main
git pull origin main
```
#### II. Retourner sur sa branche et rebase
```bash
git switch feat/remove-student
git rebase main
```

#### IV. Résoudre les conflits
Une fois les modifications pour résoudre les conflits fait :
```bash
git add .
git rebase --continue
```

#### V. Force push
```bash
git push -f origin feat/remove-student
```

### 3. Merge final <span style="color:red">[Étudiant A]</span>
```bash
git switch main
git merge feat/remove-student
git push origin main
```

## Étape finale: questionnement
### 1. Quelle est la différence entre `git merge` et `git rebase` ?
### 2. Pourquoi as-t-on du utiliser `--force` après un rebase ?
### 3. Quel workflow vous préférez pour un projet d'équipe et pourquoi ?