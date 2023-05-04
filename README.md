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