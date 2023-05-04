# Sonarqube

![](readme_docs/logo.png)

SonarQube est un logiciel libre de qualimétrie en continu de code. 

Il aide à la détection, la classification et la résolution de défaut dans le code source, permet d'identifier les duplications de code,
de mesurer le niveau de documentation et connaître la couverture de test déployée.

SonarQube permet une surveillance continue de la qualité du code grâce à son interface web 
permettant de voir les défauts de l'ensemble du code et ceux ajoutés par la nouvelle version. 

Le logiciel peut être interfacé avec un système d'automatisation comme **Jenkins** pour inclure l'analyse 
comme une extension du développement.

***

# Lancement du container

Le script suivant correspond à un `docker-compose up -d`:

```bash
bin/start
```


# Création de volumes

```
docker volume create --name sonarqube_data
docker volume create --name sonarqube_logs
docker volume create --name sonarqube_extensions
```

# Compose de Sonarqube

On lui passe le fichier **.env**

```
docker-compose --env-file .env -p sonarqube up 
```

* -d pour lancer en mode détaché *


# Accès à Sonarqube

http://localhost:9000/

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
sudo 
sysctl -w vm.max_map_count=262144
```

# Pour récupérer l'adresse URL du container

```
docker inspect sonarqube_sonarqube_1  | grep IPAddress
```