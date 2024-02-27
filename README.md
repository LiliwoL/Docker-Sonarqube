# Sonarqube

![](readme_docs/logo.png)

SonarQube est un logiciel libre de qualimétrie en continu de code. 

Il aide à la détection, la classification et la résolution de défaut dans le code source, permet d'identifier les duplications de code,
de mesurer le niveau de documentation et connaître la couverture de test déployée.

SonarQube permet une surveillance continue de la qualité du code grâce à son interface web 
permettant de voir les défauts de l'ensemble du code et ceux ajoutés par la nouvelle version. 

Le logiciel peut être interfacé avec un système d'automatisation comme **Jenkins** pour inclure l'analyse 
comme une extension du développement.

---

# Lancement du container

Le script suivant correspond à un `docker-compose up -d`:

```bash
bin/start
```

Le lancement peut être long.
Bien regarder les logs pour voir si des soucis de mémoire sont rencontrés.
Modifier les valeurs de mémoire affectées au container si besoin.

---

# Composer de Sonarqube

On lui passe le fichier **.env**

```
docker-compose --env-file .env -p sonarqube up 
```

* -d pour lancer en mode détaché *

---

# Accès à Sonarqube

http://localhost:9005/

![](readme_docs/login.png)

## Identifiants par défaut

> A modifier directement

Login: admin
Password: admin


# En cas de souci avec vm.max_map_count

Sur windows:
```
wsl -d docker-desktop
sysctl -w vm.max_map_count=262144
```

Sur Linux:
```
sudo sysctl -w vm.max_map_count=262144
```

# Pour récupérer l'adresse URL du container

```
docker inspect sonarqube_sonarqube_1  | grep IPAddress
```

---

# Configuration de Sonarqube

## Installation de plugins

Rendez-vous dans le marketplace, et installez les modules souhaités.

Conseils:
- French Language

---

## Création d'un nouveau projet sur SonarQube

Menu Projets > Create a local project

Créer un nouveau projet, et lui donner un nom.

Définissez une clé de projet unique, et validez.

Générez un **token d'authentification** pour le projet.

Ce token devra être fourni au scanner.

---

# Configuration du scanner Sonar scanner

## Scanner un projet

Plusieurs manières de scanner un projet:

- Avec un scanner installé localement
- Avec un scanner embarqué dans un docker
- Avec une action sur GitHub
  - Nécessite que le serveurb Sonarqube soit accessible depuis l'extérieur
  - https://github.com/marketplace/actions/official-sonarqube-scan
- Avec un pipeline GitLab
- Avec un pipeline Jenkins
- ...

---

## Scanner un projet avec docker

Au préalable, il faut avoir un fichier de configuration **sonar-project.properties** dans le dossier racine du projet.

Attention aux variables à personnaliser :

```bash
docker run \
    --rm \
    -e SONAR_HOST_URL="${SONARQUBE_URL}" \
    -e SONAR_SCANNER_OPTS="-Dsonar.projectKey=${SONAR_PROJECT_KEY}" \
    -e SONAR_TOKEN="${SONAR_TOKEN}" \
    -v ${APP_PATH}:/usr/src \
    sonarsource/sonar-scanner-cli
```

${SONARQUBE_URL} : URL du serveur SonarQube
${SONAR_PROJECT_KEY} : Clé du projet (fourni à la création du projet sur SonarQube)
${SONAR_TOKEN} : Token d'authentification (fourni à la création du projet sur SonarQube)
${APP_PATH} : Chemin (**absolu**) du projet à analyser

> Source:
> https://dev.to/tacianosilva/running-a-sonarqube-server-and-client-with-docker-7bk


> Exemple:
> Dans le dossier **application-to-analyse** et dans **bin/analyse**
 
![](readme_docs/0ecd1cc4.png)

---

## Scanner un projet avec sonar-scanner

Téléchargez le package **sonar-scanner**.

```bash
wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-4.2.0.1873-linux.zip
unzip sonar-scanner-cli-4.2.0.1873-linux.zip
mv sonar-scanner-4.2.0.1873-linux /opt/sonar-scanner
```

Assurez-vous que le dossier **/opt/sonar-scanner** est bien dans le PATH.

```bash
#/bin/bash
export PATH="$PATH:/opt/sonar-scanner/bin"
```

Lancez ensuite la commande **sonar-scanner** depuis le répertoire du projet.

```bash
sonar-scanner \
-Dsonar.projectKey=TestProjet \
-Dsonar.sources=. \
-Dsonar.host.url=SONARQUBE_URL \
-Dsonar.login=TOKEN
```

> Source:
> https://techexpert.tips/sonarqube/sonarqube-scanner-installation-ubuntu-linux/