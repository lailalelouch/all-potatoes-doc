
## Trello
> Move card `As a logged in user, I can create a new recipe` from the `Backlog` to the `Doing` list.
___


We'll start by creating a form for the recipes. Since we're creating a form, we'll write it inside of a file called `forms.py` and why is the file called `forms.py` because it makes sense to have your forms in a file called `forms.py` just out of organization.


So, start by creating a file called `forms.py` in the recipes app.


`recipes/forms.py`
```python
from django.forms import ModelForm
from .models import Recipe


class RecipeForm(ModelForm):
	class Meta:
		model = Recipe
		exclude = ['owner', 'slug']

```

The above form will have all the fields except for the `owner` and the `slug` fields. Why don't we want those two fields to be in the form?

Take a few minutes and try to think why...a dancing potato to help

![dancing potato](https://media1.tenor.com/images/61497871ab091f01703a3f1a624fb3c4/tenor.gif?itemid=11684043)


For the `owner` field, we obvioulsy wouldn't want the user to set the owner manually to any user he wants. The `owner` has to be set by us the developers as the logged in user who's creating this recipe.

For the `slug` field, this field is being generated automatically. Go into the `starter_file` chapter `models` section to see how the slug is being generated.


We're not done yet though. This form only shows the recipe fields. There are still no ingredients nor instructions. To add the ingredients and instructions we need to create an inline formset for each model. 

Before we actually start making thses `inline formsets`. I should probably explain what they are. Let's start with what is a `formset`. Basically a `formset` is a set of forms, but that's pretty much like defining a grasshopper as grass that hops not a very helpful definition.


Let's do this again. A `formset` is used when you need more than one of the same form. For example, when it comes to ingredients, each recipe will have more than one ingredient and therefore the user will be filling out more than one ingredient form when creating a recipe. That is why we need a `formset`.

The next part is `inline` is used when there is a `ForeignKey` relationship. So, using an `inline formset` rather than just a `formset` simplifies working with the related object. In this case, the `Ingredient` is related to the `Recipe` model through a `ForeignKey` relationship. Also, the `Instruction` model is related to the `Recipe` model through a `ForeignKey` relationship. 

I'll go through this again. We used `formset`s because we need a set of `Instruction` forms and a set of `Ingredient` forms. Second, we used an `inline formset` because these two models are connected through a `ForeignKey` relationship to the `Recipe` model so using an `inline formset`simplifies things for us.


`recipes/forms.py`
```python
from django.forms import ModelForm
from django.forms.models import inlineformset_factory
from .models import Recipe, Instruction, Ingredient

...

IngredientFormSet = inlineformset_factory(Recipe, Ingredient, exclude=['recipe'], extra=1)
InstructionFormSet = inlineformset_factory(Recipe, Instruction, fields=['description'], extra=1)
```

We created the `inline formset`s using the `inlineformset_factory`. The first arguement that it took is the parent model which `Recipe`. The second argument is the model with `ForeignKey` field. The third arguemnt is an array of the fields that should be in the `Ingredient` or `Instruction` form. Finally, the `extra` is how many empty forms do you want to be generated.

I chose the `extra` to equal to 1 because I only want one form of each `Ingredient` and `Instruction` form to be shown to the user and give the user the option to add more forms just like what happened in the admin page after we customized it.

So far all the forms are done, so next we'll be working on the actually page. We'll use those forms in the `view` that we'll be creating in the next section.

