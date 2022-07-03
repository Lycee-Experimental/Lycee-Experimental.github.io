# Utilisation
## Installation
Avec linux, rien de plus simple :
```shell
sudo apt install git-all
```

## Renseigner son identité
```shell
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com
```
!!! warning "Attention !"

    Attention, votre email doit être le même que celui de votre compte Github !

## Cloner un dépot existant sur Github
Commencez par vous rendre dans le dossier ou vous souhaitez importer le projet :
```shell
cd ~/Documents/mes_projets_de_progra
```
La commande `git clone` permet ensuite d'importer le contenu d'un projet hébergé sur Github (ou autre) :
```shell
git clone https://github.com/lycee-experimental/django-lxp
```
Le contenu du dossier ainsi que l'intégralité de l'historique des versions sera ainsi importé dans le dossier `~/Documents/mes_projets_de_progra/django-lxp`

Vous pouvez aussi faire directement :
```shell
git clone https://github.com/lycee-experimental/django-lxp ~/Documents/mes_projets_de_progra/django-lxp
```