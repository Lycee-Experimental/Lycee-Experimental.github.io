# Affichage des listes d'élèves

Une fois les données entrées dans la base, il nous faut pouvoir les afficher.

Dans un premier temps, 2 objectifs sont à atteindre :
- Pouvoir faire des recherches dans la base sur différents critères (nom, niveau, spé...)
- Générer des tableaux que l'on puisse ensuite imprimer sur le même type de critères (listes des élèves par GB, par niveaux, par spé...)


Voici le détail des fonctions de génération des tableaux, à modifier et améliorer selon nos besoins. Voir les **TODO** par la suite.


## Créer un compte administrateur

Evidemment les listes d'élèves doivent être accessibles que par des personnes authorisées.
Pour cela, si ce n'est pas encore fait, créer un compte administateur de l'application Django :

Dans le dossier contenant votre fichier docker-compose.yml, entrer les commandes suivantes :
```
# Lancer l'application
docker-compose up -d
# Créer votre compte administrateur
docker-compose exec web python manage.py createsuperuser
```

Vous deviez maintenant pouvoir vous connecter à l'interface d'administration à l'adresse suivante : http://localhost:8000/admin

## URLs des listings

Deux URLs sont pour l'instant disponibles pour accéder aux listes.
Elles sont définies dans le fichier `django-lxp/inscription/urls.py`
https://github.com/Lycee-Experimental/django-lxp/blob/main/inscription/urls.py

```python
urlpatterns = [
    path('inscriptions', login_required(InscriptionRechercheView.as_view()), name="inscriptions"),
    path('inscriptions_table', login_required(InscriptionTableView.as_view()), name="inscriptions_table"),
```

- http://localhost:8000/inscriptions permet de faire des recherches dynamiques
- http://localhost:8000/inscriptions_table permet de générer les tableaux à imprimer.


## Views des listing

Comme tout ce qui s'affiche avec Django, nos 2 vues liées aux URLs précédentes sont définies dans le fichier `django-lxp/inscription/views.py`
https://github.com/Lycee-Experimental/django-lxp/blob/main/inscription/views.py

```python
class InscriptionRechercheView(PagedFilteredTableView):
    """ Une view pour afficher un tableau de recherche dans la base.
    """
    # La classe de filtrage définie dans django-lxp/inscription/filters.py
    filter_class = ListeEleveFiltre
    # La base dans laquelle on fait nos recherches
    model = BaseEleve
    # Le tableau à afficher défini dans django-lxp/inscription/tables.py
    table_class = ListeEleveTableau
    # Le template de la page web à afficher : django-lxp/template/inscription/recherche_eleves.html
    template_name = "inscription/recherche_eleves.html"
    # Le formulaire pour effectuer des recherches django-lxp/inscription/forms.py
    formhelper_class = ListeEleveForm

    def get_queryset(self):
        # On recherche parmi tou·t·es les élèves
        return BaseEleve.objects.filter()


class InscriptionTableView(PagedFilteredTableView):
    """ Une view pour afficher un tableau à imprimer.
    On y effectue les recherches directement avec un requette GET 
    ex : http://localhost:8000/inscriptions_table?nom=LeNomRecherché
    """
    # La classe de filtrage définie dans django-lxp/inscription/filters.py
    filter_class = ListeEleveFiltre
    # La base dans laquelle on fait nos recherches
    model = BaseEleve
    # Le tableau à afficher défini dans django-lxp/inscription/tables.py
    table_class = ListeEleveTableau
    # Le template de la page web à afficher : django-lxp/template/inscription/filtre_eleves.html
    template_name = "inscription/filtre_eleves.html"
    formhelper_class = ListeEleveForm

    def get_queryset(self):
        # On recherche parmi tou·t·es les élèves
        return BaseEleve.objects.filter()
```


## Le filtre de recherche

On utilise la librairie `django_filters` pour nous faciliter la tache.

La classe est définie dans le fichier `django-lxp/inscription/filters.py`

Cela permet de lister les élèves par critères :
- Soit grâce au formulaire de recherche (voir après) sur http://localhost:8000/inscriptions 

- Soit avec des paramêtre GET sur http://localhost:8000/inscriptions_table
Par exemple : http://localhost:8000/inscriptions_table?gb_annee_en_cours=1

!!! todo TODO 
    - Ajouter d'autres champs à filtrer accompagnés de leur label (voir leur dénomination dans le fichier `models.py`). 
    - Créer une page interface avec des liens pour générer directement les pages par GB, par niveau, par spé...

```python
class ListeEleveFiltre(django_filters.FilterSet):
    """
    Classe facilitant le filtrage de la liste d'élève.
    """
    nom = django_filters.CharFilter(lookup_expr='icontains')
    # Définit la lookup expression, pour que la recherche fonctionne même si le terme n'est pas exact
    prenom = django_filters.CharFilter(lookup_expr='icontains')

    def __init__(self, *args, **kwargs):
        """Bidouille pour redéfinir le label des champs du formulaire de recherche."""
        super(ListeEleveFiltre, self).__init__(*args, **kwargs)
        self.filters['nom'].label = "Nom"
        self.filters['prenom'].label = "Prénom" 
        self.filters['gb_annee_en_cours'].label = "GB" 

    class Meta:
        """Définition des champs de la base élève dans lesquels effectuer la recherche"""
        model = BaseEleve
        fields = ['gb_annee_en_cours','nom','prenom']
        order_by = ["nom"]
```
## Formulaire de recherche

Comme tout formulaire, on le retrouve dans `django-lxp/inscription/forms.py`

C'est ce fichier qu'il faut modifier pour la mise en forme du formulaire (classes css bootstrap etc.). Comme l'ensemble des formulaires de cette appli, la librairie crispy est utilisée. Voir [ici](https://django-crispy-forms.readthedocs.io/en/latest/layouts.html) pour plus d'info sur la mise en forme.

!!! todo TODO 
    Il faudrait ajouter d'autres champs de recherche et travailler à leur mise en forme. 

```python
class ListeEleveForm(FormHelper):
    """
    Formulaire pour faire des recherches dans le tableau
    Voir https://django-crispy-forms.readthedocs.io/en/latest/form_helper.html
    """
    #form_class = "form-inline"
    form_class = "form-horizontal"
    use_custom_control = True
    form_id = "inscription-search-form"
    form_method = "GET"
    form_tag = True
    label_class = 'col-md-3'
    field_class = 'col-md-9'
    html5_required = True
    #form_show_labels = False
    #form_style = 'inline'
    #field_template = 'bootstrap4/layout/inline_field.html'
    layout = Layout(
                Fieldset(
                  "<span class='fa fa-search'></span> " + str(_("Rechercher des élèves")),
                    Div(
                        "gb_annee_en_cours",
                        "nom",
                        "prenom",
                        css_class='row' 
                    ),
                    FormActions(
                        Submit("submit", _("Filtrer")),
                        css_class="text-right align-self-center",
                    ),
                )
            )
```
## Le tableau en lui même

Définit ici : `django-lxp/inscription/tables.py`
https://github.com/Lycee-Experimental/django-lxp/blob/main/inscription/tables.py

C'est là qu'on modifie les champs à afficher et l'allure du tableau. On utilise pour cela la librairie `django_table2`

!!! todo TODO 
    Travailler l'allure du tableau et les champs à afficher (niveau, spé, teléphone, email...). Eventuellement créer d'autres tableaux selon les informations que l'on souhaite afficher (peut-être pas les même pour les GB, le GN...).

```python
class ListeEleveTableau(tables.Table):
    """Affiche le tableau de recherche des inscrit.e.s."""
    class Meta:
        # L'allure du tableau, on utilise celui de bootstrap4
        template_name = "django_tables2/bootstrap4.html"
        # Quelques classes css pour sa customisation
        attrs = {"class": "table table-striped table-hover"}
        # Les données à utiliser
        model = BaseEleve
        # Les champs à afficher dans le tableau
        fields = (
            "nom",
            "prenom",
            "nom_usage"
        )
        # Le texte si aucune entrées
        empty_text = _(
            "Aucun.e élève ne correspond aux critères de recherche."
        )
        # Le nombre d'entrée max par page
        per_page = 50
```

## Enfin, les templates de nos pages pour mettre tout ça

C'est là qu'on défini la structure des pages qui affichent les tableaux.

!!! todo TODO 
    Faire une belle mise en forme, notamment pour les impressions.

`django-lxp/template/inscription/filtre_eleves.html`

_Remarque_ : Le titre de la page doit être passé à l'url, par exemple :
http://localhost:8000/inscriptions_table?titre=Groupe de Base 1&gb_annee_en_cours=1
```
{% extends 'base.html' %}
{% load crispy_forms_tags i18n %}
{% load render_table from django_tables2 %}

{% block content %}
<div class="container">
  <h1>{{ titre }}</h1>
  <hr />

  <br />
  {% render_table table %}
</div>
{% endblock content %}
```



`django-lxp/template/inscription/recherche_eleves.html`
Sans titre, avec le formulaire de recherche crispy en plus

```
{% extends 'base.html' %}
{% load crispy_forms_tags i18n %}
{% load render_table from django_tables2 %}

{% block content %}
<div class="container">
  <h1>Inscriptions au Lycée Expérimental</h1>
  <hr />
  <br />

  {% crispy filter.form filter.form.helper %}
  <br />
  {% render_table table %}
</div>
{% endblock content %}
```
