<br />
<div align="center">

![Static Badge](https://img.shields.io/badge/Docker-2CA5E0?style=for-the-badge&logo=docker&logoColor=white)        ![Postgre](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)      ![Docker Build](https://img.shields.io/badge/dockercompose-up-green)

</div>


Déploiement conteneurisé de l'ERP ODOO
----------------------

<img alt="logo" src="https://raw.githubusercontent.com/docker-library/docs/a11348f9798f9c5e51e92409ebf4d5b39988fd13/odoo/logo.png">


Odoo, anciennement connu sous le nom d'OpenERP, est une suite d'applications professionnelles open source écrites en Python et publiées sous licence LGPL. Cette suite d'applications couvre tous les besoins de l'entreprise, du site Web/commerce électronique jusqu'à la fabrication, l'inventaire et la comptabilité, le tout parfaitement intégré. Odoo est utilisé par 2.000.000 d'utilisateurs dans le monde allant de la très petite entreprise (1 utilisateur) à la très grande (300 000 utilisateurs)


Images Docker
----------------------

Récupération des images ODOO et PostGreSQL

- image Docker ODOO 12.0 <br />
- image Docker PostGreSQL 10 <br />

Configuration
----------------------

- Création de 2 services => "web" pour le frontend et "db" pour le serveur postgreSQL en backend
- Exposition du Front sur le port 8069 <br />
- Création d'un network "odoo_network" <br />
- Création de 2 volumes Docker => odoo-web-data et odoo-db-data <br />
- Création de 2 volumes Bind Mount => ./config:/etc/odoo + ./addons:/mnt/extra-addons <br />

```
version: '2'
services:
  web:
    image: odoo:12.0
    depends_on:
      - db
    ports:
      - "8069:8069"
    volumes:
      - odoo-web-data:/var/lib/odoo
      - ./config:/etc/odoo
      - ./addons:/mnt/extra-addons
    networks:
      - odoo_network
  db:
    image: postgres:10
    environment:
      - POSTGRES_DB=postgres
      - POSTGRES_PASSWORD=odoo
      - POSTGRES_USER=odoo
      - PGDATA=/var/lib/postgresql/data/pgdata
    volumes:
      - odoo-db-data:/var/lib/postgresql/data/pgdata
    networks:
      - odoo_network
volumes:
  odoo-web-data:
  odoo-db-data:
networks:
  odoo_network:
```


Lancement des Containers
----------------------

```
docker-compose up -d
docker-compose ps
```


Afficher le site web
----------------------

http://localhost:8069


Une fois l'application lancée, vous devez créer une Base de Données sur l'interface affichée.
Vous pouvez utiliser vos paramètres.

=> Pensez à cocher la case "inclure les données de Démo"

Ensuite Validez.

Enjoy !
