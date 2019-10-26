For the recipe saving function to actually work, we need a way to save or connect a user to a recipe. We can do that through building a new model. Models do not always mean an item of some sort like a recipe, a book, a user. Sometimes a model is a relationship connecting two models. In this case the relationship is a `user` saving a `recipe`. 

Let's see how this model will look like

`recipes/models.py`
```py
...

class SavedRecipe(models.Model):
	recipe = models.ForeignKey(Recipe, on_delete=models.CASCADE, related_name="saved")
	user = models.ForeignKey(User, on_delete=models.CASCADE, related_name="saved_recipes")
```

So, each `SavedRecipe` has one recipe and one user. Basically, everytime a user saves a recipe we'll create a new `SavedRecipe` object with the user and the recipe thay want to save.


Now you've made changes to the models you need to `makemigrations` and `migrate`.


Register the `SavedRecipe` model to the admin page and create a few objects because in the next section we'll be creating the page where the user can see all the recipes they've saved.

