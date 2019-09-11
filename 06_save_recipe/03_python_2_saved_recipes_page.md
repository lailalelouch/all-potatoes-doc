In this section, we'll be creating the page where the user sees the list of recipes that they've saved. To create this page, we'll do the `view`, `template`, and `url`.


### View

`recipes/views.py`
```py
from django.shortcuts import render, redirect
from django.views.generic import CreateView, UpdateView
from django.contrib.auth.decorators import login_required

from .models import Recipe, Category, Ingredient, Measurement, SavedRecipe
from .forms import IngredientFormSet, InstructionFormSet

...

@login_required
def saved_recipe_list(request):
	return render(request, "saved_recipe_list.html")
```

You might've noticed the `login_required` decorator on top of the view. What this decorator does is check whether the user is logged in or not everytime this view is accessed. If the user is logged in, operate as normal, otherwise redirect him to `login/` url. 

However, if the url for your login page is not `login/` you can change it in the `baked-potato/settings.py`. By default you'll find it like this

`baked-potato/settings.py`
```python
...
LOGIN_URL = "/login/"
...
```

To change the `login` url, just change the value of `LOGIN_URL` to your desired url.


### URL

`recipes/urls.py`
```py
...
path('recipes/saved/', views.saved_recipe_list, name="saved-recipe-list"),
...
```

### Template

create a template in the templates folder with the name `saved_recipe_list.html`

`recipes/templates/saved_recipe_list.html`
```html
{% extends "base.html" %}

{% block title %}
Saved Recipes
{% endblock title %}

{% block content %}
			{% for saved_recipe in user.saved_recipes.all %}
				<img src="{{saved_recipe.recipe.image.url}}">
				<h5>{{saved_recipe.recipe.name}}</h5>
				<a href="{% url 'recipe-details' saved_recipe.recipe.slug %}">Recipe</a>
			{% endfor %}
{% endblock content%}

```

We looped through the recipes that the user saved by first getting the logged in `user` and then using the `related_name` from the `SavedRecipe` model which is `saved_recipes` and finally `all` means we're getting all the related recipes.


## Trello
> Move card `As a logged in user, I can see a list of saved recipes` from the `Doing` to the `Review` list if someone will needs to review it, otherwise move it to `Done`.
___ 