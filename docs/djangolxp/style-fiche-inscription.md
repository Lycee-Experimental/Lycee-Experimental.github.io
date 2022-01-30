# Affichage de la fiche d'inscription
La fiche inscription au format html est accessible à l'adresse `https://127.0.0.1/fiche/1/xxxxxxx` ou 1 est l'id 
d'une fiche d'inscription, et xxxxx son hash. 

Le pdf est téléchargeable à l'adresse : 
`https://127.0.0.1:8000/pdf/1/xxxxxxx`

Vous pourrez soit y avoir accès à la fin de la saisie d'un nouveau formulaire, soit depuis l'interface d'administration :
https://127.0.0.1:8000/admin.

## Template
Le fichier modèle (template) de la fiche d'inscription (au format html comme pdf) est le suivant : 
[`templates/inscription/fiche_inscription.html`](https://github.com/Lycee-Experimental/django-lxp/blob/main/templates/inscription/fiche_inscription.html)

Globalement, il est constitué de balises html (`<div>,<h1>,<p>`...) auxquelles on associe des classes css (par ex. `<div class="col-10">`) pour mettre en forme le document (voir plus bas pour le css).

La valeur d'un champ de notre base est affichée avec une expression de type :
`{{ fiche.prenom }}`
Éventuellement le nom de ce champ tel que défini dans models.py est appelé avec :
`{% get_verbose_field_name fiche "prenom" %}`

## CSS
Pour modifier l'affichage de la fiche au **format html**, nous pouvons nous appuyer sur les classes css existantes
du framework bootstrap4, notamment pour la [gestion des grilles](https://getbootstrap.com/docs/4.0/layout/grid/), 
et la **typographie** [ici](https://getbootstrap.com/docs/4.0/content/typography/) 
et [là](https://www.w3schools.com/bootstrap4/bootstrap_typography.asp).

Pour la fiche au **format PDF**, il semblerait que Weasyprint supporte mal les CSS de Bootstrap, notamment les grilles.
Il va peut-être falloir rédiger des classes CSS plus basiques (voir la 
[doc de Weasyprint](https://doc.courtbouillon.org/weasyprint/stable/api_reference.html#css)) et l'utilisation des 
[grilles](https://developer.mozilla.org/fr/docs/Web/CSS/CSS_Grid_Layout), 
les [coupures du texte](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Text/Wrapping_Text), l'[alignement du texte](https://developer.mozilla.org/fr/docs/Web/CSS/text-align).
, et plus généralement la [mise en forme du texte](https://developer.mozilla.org/fr/docs/Learn/CSS/Styling_text/Fundamentals)
Il est également possible de définir des classes personnalisées dans le fichier 
[`static/css/fiche_inscription.css`](https://github.com/Lycee-Experimental/django-lxp/blob/main/static/css/fiche_inscription.css).