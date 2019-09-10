So far, we haven't done the actual saving action. We can only see the recipes we've saved but we can't save any unless we create new `SavedRecipe` objects in the admin page. So, this is what we need to do next. But, before we do that we need to clarify a few things.


We want to have a button in the recipe details page that the user presses to save the recipe. However, we don't want the page to be refreshed everytime a button is clicked. To make this happen we'll be using a bit of javascript and `AJAX`.

`AJAX` is used to call a view and get back the response to be used in the view without refreshing the page. So, the view that it will call will be slightly different than the ones we normally did. The difference is in the response. Usually the response is a `render` function that returns a template. However, this time we're not returning a template, we're returning a `JsonResponse`.


Let's start with the view that will make the magic happen. 

`recipes/views.py`
```py
from django.shortcuts import render, redirect
from django.http import JsonResponse
from django.views.generic import CreateView, UpdateView
from django.contrib.auth.decorators import login_required

from .models import Recipe, Category, Ingredient, Instruction
from .forms import RecipeForm, IngredientFormSet, InstructionFormSet


...
@login_required
def save_recipe(request, recipe_id):
	saved_recipe, created = SavedRecipe.objects.get_or_create(recipe_id=recipe_id, user=request.user)
	if created:
		saved = True
	else:
		saved = False
		saved_recipe.delete()
	return JsonResponse({"saved" : saved})
``` 

This view receives the id of the recipe that the user wants to save. Then, using this id with the logged in user, we either get the `SavedRecipe` object or create one. This is what `get_or_create` does. It tries to find such a `SavedRecipe` and if it doesn't find it, it creates it. Notice that this query returns two things. The `SavedRecipe` object and a `Boolean` that indicates whether the object was retrieved or created.

```py
	saved_recipe, created = SavedRecipe.objects.get_or_create(recipe_id=recipe_id, user=request.user)
```

If the object was retrieved, then that means the user already saved the recipe and since this view is triggered then that means that the user is trying to unsave it. So, we delete the object.

```py
	saved_recipe.delete()
```

At the end of the view, we're returning a `JsonResponse` which is a response in the form of `json`. `json` is basically dictionaries and lists. So, the response that we returned was a dictionary with key `saved` and value either `True` or `False` depending on whether the recipe was saved or unsaved.

```py
	return JsonResponse({"saved" : saved})
```


### URL

`recipes/urls.py`
```py
...
path('save/<recipe_id>/', views.save_recipe, name="save-recipe"),
```
