# srvweb
Docker pour simuler l'hébergement AlwaysData Apidae

Plutôt que d'avoir 1 container pour chaque sous hébergement du serveur AlwaysData, ce container permet de les faire tourner en même temps (et donc d'avoir tout le monde sur le port 80).
Pour chaque nouvel hébergement il faut :

`XXXX = nom du compte sur alwaysdata`

`YYYY = nom du DocumentRoot, en général www ou symfony`

- docker-compose.yml
  - service
    - web
      - volumes
        - ${PWD}/data/home/XXXX/symfony:/home/XXXX/YYYY
        - ${PWD}/data/home/XXXX/logs:/home/XXXX/logs
        - ${PWD}/docker/web/XXXX.conf:/etc/apache2/sites-enabled/XXXX.conf
- Créer les répertoires
  - `/data/home/XXXX/YYYY`
  - `/data/home/XXXX/logs`
- Créer le fichier `${PWD}/docker/web/XXXX.conf` (en copiant un de ceux déjà créés)
  - Changer le ServerName et ServerAlias vers :
    - Soit un domaine que vous possédez (ex: xxx.mondomaine.fr)
    - Soit un domaine fictif, que vous pouvez configurer dans /etc/hosts sous linux
      - `${PWD}/docker/web/XXXX.conf` : `ServerName XXXX.apidae-tourisme.com`
      - /etc/hosts : `127.0.0.1	roadmap.apidae-tourisme.local`
      
 Une dois le container lancé (docker-compose up -d), vous pourrez accéder à tous les sous répertoires sur les différents noms :
 roadmap.apidae-tourisme.local
 md.apidae-tourisme.local
 ...
 
 # TODO
 Gérer les certificats https (pour l'instant ça ne fonctionne qu'en http)