




https://github.com/Lycee-Experimental/django-lxp/blob/main/inscription/tables.py

Définit l'allure général du tableau, notament les champs à afficher.

```python
import django_tables2 as tables
from django.utils.translation import gettext_lazy as _
from .models import BaseEleve

class ListeEleveTableau(tables.Table):
    """Affiche le tableau de recherche des inscrit.e.s."""
    class Meta:
        # L'allure du tableau
        template_name = "django_tables2/bootstrap4.html"
        attrs = {"class": "table table-striped table-hover"}
        # Les données à utiliser
        model = BaseEleve
        # Les champs à afficher
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
        per_page = 20
```



https://github.com/Lycee-Experimental/django-lxp/blob/main/inscription/urls.py

    path('inscriptions', login_required(InscriptionView.as_view()), name="inscriptions"),






https://github.com/Lycee-Experimental/django-lxp/blob/main/inscription/views.py

```python
class InscriptionView(PagedFilteredTableView):
    """ Une view pour afficher un tableau de recherche dans la base.
    """
    filter_class = ListeEleveFiltre
    model = BaseEleve
    table_class = ListeEleveTableau
    template_name = "inscription/filtre_eleves.html"
    formhelper_class = ListeEleveForm

    def get_queryset(self):
        return BaseEleve.objects.filter()
```

https://github.com/Lycee-Experimental/django-lxp/blob/main/inscription/filters.py
```python
import django_filters
from .models import BaseEleve


class ListeEleveFiltre(django_filters.FilterSet):
    class Meta:
        model = BaseEleve
        fields = {
            "nom": ["icontains"],
            "prenom": ["exact"],
        }
        order_by = ["nom"]
```



https://github.com/Lycee-Experimental/django-lxp/blob/main/templates/inscription/filtre_eleves.html

```html

{% extends 'base.html' %}
{% load crispy_forms_tags i18n %}
{% load render_table from django_tables2 %}

{% block title %}{% trans "Inscriptions au Lycée Expérimental" %}{% endblock title %}

{% block content %}
<div class="container">
  <h1>{% trans "Inscriptions au Lycée Expérimental" %}</h1>
  <hr />
  <br />

  {% crispy filter.form filter.form.helper %}
  <br />
  {% render_table table %}
</div>
{% endblock content %}
```