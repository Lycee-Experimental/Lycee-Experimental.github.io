title: Installer Pycharm

Pour travailler sur notre projet, on utilise l'environnement de développement (Integrated Development Environment) Pycharm.
Ce logiciel dédié au developpement python est gratuit dans sa [version communautaire](https://www.jetbrains.com/fr-fr/pycharm/download/download-thanks.html?platform=linux&code=PCC), mais payant pour avoir accès à des fonctionnalités supplémentaires, notamment pour le développement Web. Pour installer et activer la license pro en attendant de l'acquérir :

# Installation

```bash
# On télécharge la version pro de l'IDE
sudo wget -c https://download-cdn.jetbrains.com/python/pycharm-professional-2021.3.tar.gz -O - | sudo tar -xz -C /opt/
# Son patch d'activation
sudo wget https://github.com/Lycee-Experimental/django-lxp/raw/main/extra/fineagent.jar -O /opt/pycharm-2021.3/fineagent.jar
# on indique à l'IDE où trouver le patch
echo "-javaagent:/opt/pycharm-2021.3/fineagent.jar" | sudo tee -a /opt/pycharm-2021.3/bin/pycharm64.vmoptions
# On télécharge la clé d'activation dans le dossier de config
mkdir -p ~/.config/JetBrains/PyCharm2021.3/
wget https://github.com/Lycee-Experimental/django-lxp/raw/main/extra/pycharm.key -O ~/.config/JetBrains/PyCharm2021.3/pycharm.key
# Enfin, on crée un élément de menu pour lancer notre IDE plus facilement
cat <<EOT > ~/.local/share/applications/pycharm.desktop
[Desktop Entry]
Name=PyCharm
Exec=/opt/pycharm-2021.3/bin/pycharm.sh
Comment=Programmation python
Terminal=false
Categories=Development;
Icon=/opt/pycharm-2021.3/bin/pycharm.svg
Type=Application
EOT

# On charge l'élément de menu ainsi créé 
sudo update-desktop-database ~/.local/share/applications
```
PyCharm est maintenant pret à être utilisé depuis le menu !

# Configuration

Il nous faut maintenant ouvrir notre projet et lui indiquer d'utiliser l'instalation de python3.9 dans notre environement virtuel.

* Lancer le logiciel et ouvrir le dossier django-lxp -> `File` > `Open...`
* Cliquer sur `<No Interpreter>` en bas à droite puis sur `Add interpreter...`
* Sélectionner `Existing environment` et notre interpreter `Python3.9`