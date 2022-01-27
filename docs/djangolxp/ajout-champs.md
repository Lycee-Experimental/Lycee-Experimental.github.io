# Ajout de champs avec Django
## Création de champs dans la base : [models.py](https://github.com/Lycee-Experimental/django-lxp/blob/main/inscription/models.py)
Les champs de la base élèves sont à définir dans le fichier  [models.py](https://github.com/Lycee-Experimental/django-lxp/blob/main/inscription/models.py) qui correspond à la gestion des bases de données.

Exemple :
```python
phone = models.CharField(max_length=255, blank=True, null=True,
                             verbose_name="Téléphone")
```
* `phone` est le nom du champ.
* `CharField` signifie que c'est une chaine de caractères (on pourrait plutôt implémenter un contrôle du nombre de chiffres...).
* `verbose_name` est son nom complet que l'on pourra utiliser pour l'affichage dans les formulaires.
* `blank=True, null=True` permet d'indiquer que ce champ n'est pas nécessaire à renseigner pour ajouter un élève dans la base.

L'enjeux ici est donc de déterminer les champs à ajouter, s'ils sont indispensables ou pas à la saisie, et de quel type ils sont.
> Pour déterminer les champs à créer, se baser sur ceux des fiches d'inscription du LXP, et des bases [SIECLE](https://github.com/Lycee-Experimental/django-lxp/blob/main/extra/Siecle/Exemple.csv) et [CYCLADES](https://github.com/Lycee-Experimental/django-lxp/raw/main/extra/cyclades/champs_cyclades.ods)

> Pour en savoir plus sur les types de champs, voir la [doc de django](https://docs.djangoproject.com/fr/4.0/ref/models/fields/).

## Affichage du champ dans un formulaire : [forms.py](https://github.com/Lycee-Experimental/django-lxp/blob/main/inscription/forms.py)
Pour pouvoir faire des saisies d'élèves et ainsi tester notre appli, il faut également faire en sorte que ces champs (et surtoût les champs nécessaires), soient affichés dans les formulaires.
Cela se passe dans le fichier [forms.py](https://github.com/Lycee-Experimental/django-lxp/blob/main/inscription/forms.py).
Il existe différents formulaires qui correspondent aux étapes de la saisie :
```python
class InscriptionForm1(forms.ModelForm):
```
Dans le formulaire de votre choix, les nouveaux champs créés précédement dans models.py **doivent être renseignés à 2 niveaux** :
1. Pour la mise en forme du formulaire dans la partie Layout :
```python
 self.helper.layout = Layout(
            'civility',
            'last_name',
            'first_name',
            'birth_date',
            'birth_place',
            'birth_country',
            'address',
            'photo',
        )
```

> Pour en savoir plus sur la mise en forme des champs et changer leur apparence, voir la [doc de la librairie Crispy](https://django-crispy-forms.readthedocs.io/en/latest/layouts.html).


2. Pour lier ce champs à notre modèle de Base Elève
```python
 class Meta:
        # Modèle utilisé et entrées à renseigner
        model = BaseEleve
        fields = ['address', 'civility', 'last_name', 'first_name', 'birth_date', 'birth_place', 'birth_country',
                  'photo',]
```

## Ajouter une étape au formulaire : [views.py](https://github.com/Lycee-Experimental/django-lxp/blob/main/inscription/views.py)

Si vous souhaitez rajouter une étape à la saisie, créer un nouveau formulaire dans forms.py, par exemple
```python

class InscriptionForm4(forms.ModelForm):
    name = 'Ma nouvelle étape'

    def __init__(self, *args, **kwargs):
        """
        Surcharge de l'initialisation du formulaire
        """
        super().__init__(*args, **kwargs)
        # FormHelper pour customiser ton formulaire
        self.helper = FormHelper()
        # Id et la classe bootstrap de ton formulaire
        self.helper.form_class = 'form-horizontal'
        self.helper.form_id = 'BaseEleve-form'
        # Largeur des labels et des champs sur la grille
        self.helper.label_class = 'col-md-4'
        self.helper.field_class = 'col-md-8'
        # Affichage de ton formulaire
        self.helper.layout = Layout(
            # Liste des champs à afficher
           'mon_nouveau_champ',
        )

    class Meta:
        # Modèle utilisé et entrées à renseigner
        model = BaseEleve
        fields = ['mon_nouveau_champ',
                 ]
```

Importer votre nouveaux formulaire dans le fichier [views.py](https://github.com/Lycee-Experimental/django-lxp/blob/main/inscription/views.py)
```python
from .forms import InscriptionForm1, InscriptionForm2, InscriptionForm3, ListeEleveForm, InscriptionForm4
```
Et l'ajouter à la liste utilisée par notre formulaire d'inscription :
```python
form_list = [InscriptionForm1, InscriptionForm2, InscriptionForm3, InscriptionForm4,]
```
