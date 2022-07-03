# Utilisation de Github avec Pycharm

**Github** est le site le plus utilisé pour la gestion des versions (historique des modifications du code) avec l'outil **Git**. Bien que racheté récemment par Microsoft, nous allons donc apprendre à l'utiliser (d'autres services basés sur git existent : Gitlab, bitbucket...).

L'objectif de cette page wiki n'est pas de remplacer la documentation de l'outil [Git](https://git-scm.com/book/fr/v2) ou de [Github](https://docs.github.com/en), mais de voir comment utiliser Github avec notre IDE **PyCharm** pour centraliser les modifications apportées au projet.


## Workflow

Nous allons essayer de travailler comme le font désormais la majorité DevOps pour une intégration continue : avec un développement basé sur le tronc (trunk based development). Plus de détail [ici](https://www.atlassian.com/fr/continuous-delivery/continuous-integration/trunk-based-development).

En effet, pour avoir un code qui évolue rapidement par les contributions simultanées de plusieurs développeurs, nous aurons les 2 objectifs suivants :
- Conserver une version du code bien testée et fonctionnelle en production (branche `main` aka le tronc)
- Créer des branches éphémères à chaque fois qu'on souhaite implémenter une nouvelle fonctionalité.

Une fois que nos quelques petites modifications de branche sont testées (pas de grosses révolutions du code, on y va par petites touches...) on pourra alors procéder à un **Pull Request**, c'est dire tenter une fusion de notre branche éphémère à la branche `main`.

Voir un exemple ici : https://github.com/Lycee-Experimental/django-lxp/pull/2

### Créer une nouvelle branche avec Pycharm

Commencer par mettre votre branche `main` à jour des dernières modifs depuis github avec un `git pull` obtenu via le raccourci en haut à gauche de pycharm : ![pull](https://i.imgur.com/JPZJgCd.png)

Cliquer ensuite sur l'icône suivant en bas à droite de la fenêtre de PyCharm : ![branch-icon](https://i.imgur.com/cxzQwtG.png)

Puis sur `+ New Branch` pour créer une nouvelle branche dont PyCharm vous demande ensuite le nom. :

![new-branch](https://i.imgur.com/L1ObUow.png)

### Modification de nos fichiers sources
Effectuez alors des modifs dans les fichiers du code, par exemple `models.py` et `forms.py` pour [ajouter des champs et les afficher dans le formulaire](https://github.com/Lycee-Experimental/django-lxp/wiki/5.-Ajout-de-champs-dans-le-formulaire).

### Tester les changements en local
Avant de valider nos changements et les pousser (push) sur Github, tester localement notre application : ![launch](https://i.imgur.com/Wn58KyI.png)

### Enregistrement des modifications dans notre nouvelle branche
La commande `git commit` permet d'enregistrer les changements, on y accède ici avec PyCharm : ![commit](https://i.imgur.com/cP2HRTZ.png)

Une fenêtre s'ouvre à gauche pour indiquer les fichiers à intégrer à notre enregistrement (commit), et le commentaire à associer à ce changement, puis `COMMIT` pour les enregistrer localement, ou `COMMIT AND PUSH` pour envoyer directement sur github :

![push](https://i.imgur.com/R5jrx2m.png)

Un test automatique de l'application est alors effectué par Github Actions. S'il est valide, on obtient ceci :

![action-ok](https://i.imgur.com/rc4LIhW.png)

### Bascule des changements dans `main`.

Aller dans le menu `Git` du haut, puis `Github`, puis `Create Pull Request`.

![PR](https://i.imgur.com/AKGTKso.png)

Renseigner le titre et la description de votre requète de fusion de votre branche avec `main`
On pourra alors discuter sur Github de la pertinence de cette fusion, et y apporter d'éventuelles modifications avant de la valider.

Un test de la **pull request** sera réalisé par **Github Actions**, qui s'il est valide, sera déployé automatiquement par **Heroku** à une adresse de la forme : https://django-lxp-pr-3.herokuapp.com

Une fois la Pull Request validée par notre équipe, notre nouvelle main sera accessible à l'adresse : https://main.inscription.cf, prête à passer en production à l'adresse : https://inscription.cf

Vous pouvez ensuite effacer votre nouvelle branche dans le menu en bas à droite.
![del-branch](https://i.imgur.com/haEALYL.png)