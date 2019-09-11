## Trello
> Move card `As a logged in user, I can download a recipe in pdf format.` from the `Backlog` to the `Doing` list.
___


Let's start by installing the required libraries. So, stop the server, make sure your virtual environemnt is activated and start installing

`terminal`
```shell
(env)$ pip install -U django-easy-pdf WeasyPrint
```

Don't forget to update the requirements file too
```shell
(env)$ pip freeze > requirements.txt
```

We'll start by creating the view. 
`recipes/views.py`
```py
from django.shortcuts import render, redirect
from django.views.generic import CreateView, UpdateView
from django.http import JsonResponse
from django.contrib.auth.decorators import login_required
from easy_pdf.rendering import render_to_pdf_response

from .models import Recipe, Category, Ingredient, Instruction, SavedRecipe
from .forms import RecipeForm, IngredientFormSet, InstructionFormSet

...
@login_required
def export_recipe(request, recipe_slug):
	recipe = Recipe.objects.get(slug=recipe_slug)
	context = { "recipe" : recipe }
	return render_to_pdf_response(request, 'export_recipe.html', context)
```

The only new thing here is the response which is a `render_to_pdf_response` which was imported from the installed library. What this basically does is, it renders the `export_recipe.html` template and turns it into a pdf.

Create a template with this name and write anything you want in it.

Let's create a url for this view, so you can test it out before we go any further.

`recipes/urls.py`
```py
...
    path('export/<recipe_slug>/', views.export_recipe, name="export-recipe"),
...
```

Let's add a button in the recipe detail page that calls this view

`recipe/templates/recipe_details.html`
```html
<a href="{% url 'export-recipe' recipe.slug %}">Export</a>
```

Test that everything is working. If everything is working fine, we'll be working on the template to actually have the recipe's information there.

Do keep in mind that we cannot use bootstrap here.

`recipes/templates/export_recipe.html`
```
<div style="padding: 20px">
	<h1 style="text-align: center; font-size: 40px; margin-bottom: -200px">{{recipe.name}}</h1>
	<img src="{{recipe.image.url}}" width="500px" style="object-fit: cover;">
	<p style="">Recipe by: {{recipe.owner}}</p>
	<p style="">{{recipe.description}}</p>
	
	<hr>
	<h3> Ingredients </h3>
	<div>
		<ul>
		{% for measurement in recipe.measurements.all %}
			 <li>{{measurement.amount}} {{measurement.measure}} - {{measurement.ingredient}}</li>
		{% endfor %}
		</ul>
	</div>
	<hr>
	<h3> Method </h3>
	<div>
		<ol>
		{% for instruction in recipe.instructions.all %}
			 <li>{{instruction.description}}</li>
		{% endfor %}
		</ol>
	</div>
</div>
```

I've displayed the recipes information with minimal styling. So, you can go ahead and style your pdf however you want. Enjoy


## Trello
> Move card `As a logged in user, I can download a recipe in pdf format.` from the `Doing` to the `Review` list if someone will needs to review it, otherwise move it to `Done`.
___

## Git

Create a new checkpoint

```shell
$ git add .
$ git commit -m "finished pdf export functionality"
$ git push
```
___
