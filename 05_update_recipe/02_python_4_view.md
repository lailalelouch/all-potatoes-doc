The recipe update view is very similar to the create.

`recipes/views.py`
```py
from django.shortcuts import render, redirect
from django.views.generic import CreateView, UpdateView

from .models import Recipe, Category, Ingredient, Instruction
from .forms import RecipeForm, IngredientFormSet, InstructionFormSet

...

class RecipeUpdateView(UpdateView):
	template_name = 'update_recipe.html'
	form_class = RecipeForm
	success_url = 'recipe-details'
	model = Recipe
```

This part is identical to the create view but instead of inheriting `CreateView`, it's inheriting from `UpdateView`. Also, we need to specify the `model`. Without the model the `UpdateView` wouldn't know where to retrieve or what kind of object we want to update. So, we specified that we are updating a `Recipe` object.


### get()

`recipes/views.py`
```py
class RecipeUpdateView(UpdateView):
	template_name = 'update_recipe.html'
	form_class = RecipeForm
	success_url = 'recipe-details'
	model = Recipe

	def get(self, request, *args, **kwargs):
		self.object = self.get_object()
		form_class = self.get_form_class()
		form = form_class(instance=self.object)
		ingredient_form = IngredientFormSet(instance=self.object)
		instruction_form = InstructionFormSet(instance=self.object)
		return self.render_to_response(
			self.get_context_data(form=form,
								  ingredient_form=ingredient_form,
								  instruction_form=instruction_form))
```

In the `get()` function we want to show the user the recipe form with its ingredients and instructions all filled out for him so he can change whatever he wants in the recipe. I'll go through how we were able to do this in details. However, if you want to pass this part skip to the dancing potato.

```py
self.object = self.get_object()
```
In the first line unlike the create, we actually have an object. So, were able to set the recipe object through the `get_object()` function. This function will get the slug from the url and does the required get query to get us the recipe.


```py
form = form_class(instance=self.object
ingredient_form = IngredientFormSet(instance=self.object)
instruction_form = InstructionFormSet(instance=self.object)
```

## Trello
> Move card `As a logged in user, I can update my recipes` from the `Backlog` to the `Doing` list.
___

In the create view, all the forms were empty. However, here we had to fill the forms. `instance` is the recipe object that we want to update, the recipe object that we want to fill the forms with.

Everything else in this function is identical to the recipe create view.

![dancing potato](https://media1.tenor.com/images/61497871ab091f01703a3f1a624fb3c4/tenor.gif?itemid=11684043)


### post()


`recipes/views.py`
```py
class RecipeUpdateView(UpdateView):
	template_name = 'update_recipe.html'
	form_class = RecipeForm
	success_url = 'recipe-details'
	model = Recipe

	...

	def post(self, request, *args, **kwargs):
		self.object = self.get_object()
		form_class = self.get_form_class()
		form = form_class(request.POST, request.FILES, instance=self.object)
		ingredient_form = IngredientFormSet(self.request.POST, instance=self.object)
		instruction_form = InstructionFormSet(self.request.POST, instance=self.object)
		if (form.is_valid() and ingredient_form.is_valid() and
			instruction_form.is_valid()):
			return self.form_valid(form, ingredient_form, instruction_form)
		else:
			return self.form_invalid(form, ingredient_form, instruction_form)
```

This function is also identical to the `post()` method in the create with the difference of setting the object in the first line and adding the `instance` to the forms. If you're wondering why we need to add the `instance` to these forms, the reason is because we want the form to know that we are not creating a new recipe with the information sent through the `post` request. We are updating the information that already exists in the `Recipe` object specified in the `instance`.


### form_valid()

`recipes/views.py`
```py
class RecipeUpdateView(UpdateView):
	template_name = 'update_recipe.html'
	form_class = RecipeForm
	success_url = 'recipe-details'
	model = Recipe

	...

	def form_valid(self, form, ingredient_form, instruction_form):
		form.save()
		ingredient_form.save()
		instruction_form.save()
		return redirect(self.get_success_url(), self.object.slug)
```

All we did here was save the forms which is saving the changes made to the recipe.

### form_invalid()

`recipes/views.py`
```py
class RecipeUpdateView(UpdateView):
	template_name = 'update_recipe.html'
	form_class = RecipeForm
	success_url = 'recipe-details'
	model = Recipe

	...

	def form_invalid(self, form, ingredient_form, instruction_form):
		return self.render_to_response( self.get_context_data(form=form, ingredient_form=ingredient_form, instruction_form=instruction_form))
``` 

In here we're just rendering the same page with the same forms again so the user can fix his errors like if any fields were left empty and so on.
