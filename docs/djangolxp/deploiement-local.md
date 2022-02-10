# Développement du projet avec Pycharm

## Installation de Pycharm
Commencer par installer [Pycharm](/pycharm/install-pycharm)

## Installation des dépendances
Installer les dépendances nécessaires au projet. Dans un terminal :
```shell
sudo apt install gdal-bin libpq-dev git python3.8-dev python3-distutils libmysqlclient-dev
# Installation de Poetry
curl -sSL https://raw.githubusercontent.com/sdispater/poetry/master/get-poetry.py | python3
```
!!! note "Pour info"

    `git` est le gestionnaire de version que nous utilisons via Github [Voir ici](/git/presentation.md). 
    `gdal-bin` est une librairie dont dépend l'affichage des cartes géographiques par `django-leaflet`.  
    `libpq-dev` est nécessaire à l'installation de psycopg, la librairie python pour se connecter à une base PostgreSQL  
    `python3.8-dev` est sont librairies de dévelopement de python3.8.
    `libmysqlclient-dev` est une librairie dont dépend `mysqlclient` pour se connecter à notre base mariadb
    `python3-distutils` est requis pour le bon fonctionnement de Poetry  
    `Poetry` est un outil qui permet de mettre en place un environnement virtuel et installer les dépendances  


## Configuration de Github dans Pycharm

### Création d'un compte Github

Si vous n'en avez pas encore créé un, rendez-vous sur [cette page](https://github.com/join). Indiquer le nom de codeur·euse que vous souhaitez.

### Intégration de l'Organisation Lycée Expérimental

Pour avoir le droit d'apporter des modifications au code de django-lxp, il vous faut appartenir à l'organisation "Lycée Expérimental".
Pour cela demander à l'un de ses membres de vous y inviter en lui indiquant le nom de votre nom de codeur·euse.

Vous devriez ensuite recevoir un email pour accepter l'invitation, ou alors rendez-vous simplement sur [la page
de l'organisation](https://github.com/lycee-experimental) pour accepter l'invit.

### Configurer Pycharm avec votre identifiant Github

Aller dans le menu `File` / `Settings`

![settings](https://i.imgur.com/t34xNqk.png)

Puis dans `Version control` / `Github` / `+` / `Log via Github`

![github](https://i.imgur.com/aLEIy25.png)

Votre navigateur devrait s'ouvrir pour que vous puissiez accepter que Pycharm soit associé à votre compte Github.

## Télécharement du code source du projet

Rendez-vous dans le menu `git` / `clone`.

![](https://i.imgur.com/HnQCBaL.png)

![](https://i.imgur.com/lCW2YoL.png)
Indiquez l'url de notre dépôt (`https://github.com/Lycee-Experimental/django-lxp`), ainsi que le répertoire dans lequel
vous souhaitez le télécharger, puis cliquez sur `Clone`.

## Configuration de l'environnement virtuel
Une fois le code source téléchargé depuis notre dépot distant Github, Pycharm va détecter le fichier `pyproject.toml`
et donc vous proposer de mettre en place l'environnement virtuel avec Poetry.
Cliquer sur `OK`.

![](https://i.imgur.com/pzBv48A.png)

Patienter pendant que Poetry install l'ensemble des paquets nécessaires

![](https://i.imgur.com/IO9aeky.png)


## Génération du fichier .env contenant nos clés secrètes, et des données initiales
Certaines clés ne doivent pas être publiées sur Github car sont personnelles (service AWS, google map...).
On va donc les générer à partir d'un fichier encrypté `encrypted.data`.
On télécharge ensuite les données initiales sous forme de json qui sont aussi encryptées.

- Ouvrir un terminal en cliquant en bas à gauche :

![](https://i.imgur.com/ZcxTcvb.png)

- Y copier la commande suivante (`CTR+SHIFT+V` pour coller dans le terminal) :

```shell
openssl enc -d -aes-256-cbc -in encrypted.data -out .env
export $(cat .env | xargs)
mkdir data
curl "https://gist.githubusercontent.com/davy39/c382e6a01e0b15a3be883bbfd4863627/raw/c875f335cc2b2d8f58adf9c6ff3545184f609afd/secured.tar.gz"  | openssl enc -d -aes256 -pbkdf2 -pass pass:$SECRET_KEY | tar xz -C data
```

!!! info

    Demander le mot de passe à un membre de l'équipe de développement du LXP...

## Génération de la base de donnée pour Django
Pour commencer à utiliser localement notre application, il faut générer la base de donnée. Pour cela, 2 possibilités :

- Soit entrer la commande suivante dans le terminal :

```shell
python manage.py migrate
```

- Soit ouvrir l'onglet manage.py à partir du menu `Tools` ou `CTL+ALT+R` :

  ![](https://i.imgur.com/7HvhW2h.png)
  Puis tapper simplement la commande `migrate`
  ![](https://i.imgur.com/i4Db9o7.png)

## Création d'un superutilisateur

Pour pouvoir accéder à l'interface d'administration de notre appli, il faut préalablement créer un superutilisateur.

- Toujours dans l'onglet `manage.py` (`CTL+ALT+R)`, entrer la commande `createsuperuser` puis renseigner un nom, email et mot de passe.

## Lancement du serveur Django
Il suffit ensuite de lancer l'application :
![launch](https://i.imgur.com/Wn58KyI.png)

L'application est alors accessible à l'adresse : [https://127.0.0.1](https://127.0.0.1)