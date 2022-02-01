title: Installer Pycharm

Pour travailler sur notre projet, on utilise l'environnement de développement (Integrated Development Environment) Pycharm.
Ce logiciel dédié au développement python est gratuit dans sa [version communautaire](https://www.jetbrains.com/fr-fr/pycharm/download/download-thanks.html?platform=linux&code=PCC), mais payant pour avoir accès à des fonctionnalités supplémentaires, notamment pour le développement Web. Pour installer et activer la license pro en attendant de l'acquérir :

# Installation de Pycharm

```bash
# On télécharge la version pro de l'IDE
sudo wget -c https://download-cdn.jetbrains.com/python/pycharm-professional-2021.3.2.tar.gz -O - | sudo tar -xz -C /opt/
# Son patch d'activation
sudo wget -c https://github.com/Lycee-Experimental/django-lxp/raw/main/extra/ja-netfilter-all.tar.gz -O - | sudo tar -xz -C /opt/pycharm-2021.3.2
# on indique à l'IDE où trouver le patch
echo "-javaagent:/opt/pycharm-2021.3.2/ja-netfilter-all/ja-netfilter.jar" | sudo tee -a /opt/pycharm-2021.3.2/bin/pycharm64.vmoptions
# Enfin, on crée un élément de menu pour lancer notre IDE plus facilement
cat <<EOT > ~/.local/share/applications/pycharm.desktop
[Desktop Entry]
Name=PyCharm
Exec=/opt/pycharm-2021.3.2/bin/pycharm.sh
Comment=Programmation python
Terminal=false
Categories=Development;
Icon=/opt/pycharm-2021.3.2/bin/pycharm.svg
Type=Application
EOT

# On charge l'élément de menu ainsi créé 
sudo update-desktop-database ~/.local/share/applications
```

Un petit redémarrage (ou simplement déconnection/reconnection), PyCharm est maintenant prêt à être utilisé depuis le menu !

Au premier lancement, vous tomberez sur la page d'activation de Pycharm. Il vous faudra sélectionner le mode d'essai 
(`Start trial`) puis vous connecter sur Jetbrains, par exemple avec **vos identifiants Github**.

!!! note

        Si vous n'en avez pas encore créé un, rendez-vous sur [cette page](https://github.com/join). Indiquer le nom de codeur·euse que vous souhaitez.

