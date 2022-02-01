# Affichage de la fiche d'inscription
Le pdf de la fiche d'inscription est téléchargeable à l'adresse `https://127.0.0.1:8000/pdf/1/xxxxxxx` ou 1 est l'id
d'une fiche d'inscription, et xxxxx son hash.

Vous pourrez soit avoir accès à ce couple id/hash à la fin de la saisie d'un nouveau formulaire, 
soit depuis l'interface d'administration :
https://127.0.0.1:8000/admin.

D'une manière générale une librairie nommée Weasyprint transforme le contenu d'une page web en un fichier PDF.

Pour modifier la structure de cette page, il vous faudra donc avoir des bases de html et css.

Si ce n'est pas encore le cas, allez voir [par ](https://fr.khanacademy.org/computing/computer-programming/html-css).

## Modification du contenu de la fiche d'inscription
### Django et les fichiers Template
Le fichier modèle (template) de la fiche d'inscription (au format html comme pdf) est le suivant : 
[`templates/inscription/fiche_inscription.html`](https://github.com/Lycee-Experimental/django-lxp/blob/main/templates/inscription/fiche_inscription.html)

Globalement, il est constitué de balises html (`<div>,<h1>,<p>`...) auxquelles on associe des classes css 
(par ex. `<span class="float-right">`) pour mettre en forme le document (voir plus bas pour le css).

La valeur d'un champ de notre base est affichée avec une expression de type
`{{ fiche.prenom }}` qui correspond aux champs de notre modèle BaseEleve.

## Modifier l'apparence avec le CSS
L'affichage de la fiche au **format PDF**, passe par Weasyprint qui ne permet pas l'utilisation de tout type de CSS
(ex pas de Bootstrap utilisé ailleurs sur le site, pas de grid...). 
Il va donc falloir utiliser des classes CSS plus basiques (voir la 
[doc de Weasyprint](https://doc.courtbouillon.org/weasyprint/stable/api_reference.html#css)).

On peut donc utiliser un élément de base de HTML, l'élément `<table>` et ses composants pour constituer un **tableau**. 

D'autres éléments de css peuvent nous être utiles, comme les
[coupures du texte](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Text/Wrapping_Text), 
l'[alignement du texte](https://developer.mozilla.org/fr/docs/Web/CSS/text-align).
, et plus généralement la [mise en forme du texte](https://developer.mozilla.org/fr/docs/Learn/CSS/Styling_text/Fundamentals)
On peut personnaliser les classes  CSS dans le fichier 
[`static/css/fiche_inscription.css`](https://github.com/Lycee-Experimental/django-lxp/blob/main/static/css/fiche_inscription.css).