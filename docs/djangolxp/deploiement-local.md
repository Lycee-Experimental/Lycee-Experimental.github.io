# Déploiement de django en local
## Installation des paquets nécessaires

```bash
#  Pour travailler avec python 3.9 
sudo apt install python3.9-venv python3-pip python3.9-dev
# Pour travailler avec le système de gestion de versions décentralisé Git
sudo apt install git
```

## Récupération du code source de django-lxp

```bash
# Création d'un dossier contenant notre projet
mkdir ~/Documents/Programmation
git clone https://github.com/Lycee-Experimental/django-lxp ~/Documents/Programmation/django-lxp
```

## Installation de toutes les dépendances python

```bash
# On se rend dans notre dossier de travail
cd ~/Documents/Programmation/django-lxp
# On y crée un environnement virtuel de travail basé sur python3.9
python3.9 -m venv env
# On se connecte à notre environnement pour y travailler
source env/bin/activate
# On installe l'ensemble des dépendances listée dans le fichier requirements.txt 
python -m pip install wheel
python -m pip install -r requirements.txt
```

## Génération du .env contenant nos clés secrètes
Demander le mot de passe à un membre de l'équipe de développement du LXP...
```
cd ~/Documents/Programmation/django-lxp
openssl enc -d -aes-256-cbc -in encrypted.data -out .env
```

## Génération de la base de donnée pour Django

```bash
# On va dans notre dossier de travail
cd ~/Documents/Programmation/django-lxp
# On se connecte à notre environnement pour y travailler
source env/bin/activate
# On utilise l'utilitaire manage.py de django pour évaluer les modifications à apporter à la base de données.
python manage.py makemigrations
# On effectue les modifications à apporter à la base 
# Rq : une valeur par défaut peut alors nous être demandée pour les entrées déjà existantes dans la base.
python manage.py migrate
```

## Création d'un superutilisateur
```bash
cd ~/Documents/Programmation/django-lxp
source env/bin/activate
python manage.py createsuperuser
```

## Lancement du serveur Django

```bash
cd ~/Documents/Programmation/django-lxp
source env/bin/activate
python manage.py runserver
```
> /home/User/Documents/Programmation/django-lxp/staticfiles
>
> /home/user/Documents/Programmation/django-lxp/staticfiles
>
> Watching for file changes with StatReloader
>
> Performing system checks...
>
>
> System check identified no issues (0 silenced).
>
> December 15, 2021 - 17:55:40
>
> Django version 4.0, using settings 'django-lxp.settings'
>
> Starting development server at http://127.0.0.1:8000/
>
> Quit the server with CONTROL-C.

C'est tout bon ! Vous pouvez accéder au site en vous connectant à l'addresse : http://127.0.0.1:8000/