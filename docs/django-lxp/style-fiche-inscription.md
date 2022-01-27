La fiche inscription au format html est accessible à l'adresse https://inscription.cf/fiche/1/xxxxxxx ou 1 est l'id d'une fiche d'inscription, et xxxxx son hash. Le pdf est téléchargeable à l'adresse : https://inscription.cf/pdf/1/xxxxxxx

# Template

Le fichier modèle (template) de la fiche d'inscription (au format html comme pdf) est le suivant : [`templates/inscription/fiche_inscription.html`](https://github.com/Lycee-Experimental/django-lxp/blob/main/templates/inscription/fiche_inscription.html)

Globalement, il est constitué de balises html (`<div>,<h1>,<p>`...) auxquelles on associe des classes css (par ex. `<div class="col-10">`) pour mettre en forme le document (voir plus bas pour le css).

La valeur d'un champ de notre base est affichée avec une expression de type : `{{ fiche.prenom }}`
Eventuellement le nom de ce champ tel que défini dans models.py est appelé avec : `{% get_verbose_field_name fiche "prenom" %}`

# CSS

Pour modifier l'affichage de la fiche (au format html et pdf, nous pouvons nous appuyer sur les classes css existantes du framework bootstrap4, notamment pour la [gestion des grilles](https://getbootstrap.com/docs/4.0/layout/grid/), ou définir des classes personnalisées dans le fichier [`static/css/fiche_inscription.css`](https://github.com/Lycee-Experimental/django-lxp/blob/main/static/css/fiche_inscription.css).