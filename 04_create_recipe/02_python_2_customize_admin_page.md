Go into the admin page and try to add a recipe.

You'll notice that it's quite the hassle. You'd first have to go and create a new recipe. Then, create a new ingredient separately and choose that specific recipe for that ingredient and create another ingredient and another. After that, you'd have to go to the instructions and create a new instruction object and so on. Now that's alot. I'd rather just write the recipe on a piece of paper and just get this over with. 


The solution to this problem is to customize the admin page and instead of having to add the ingredients and instructions separatley, it would be nice to be able to add and remove in the same page we're creating the recipe.


To modify our admin page we need to write some code. But where do we write it. I'll give you a hint "we're customizing the admin page". If you still can't guess, have a dancing potato to help you think.


![dancing potato](https://media1.tenor.com/images/61497871ab091f01703a3f1a624fb3c4/tenor.gif?itemid=11684043)

If you still don't know, we're writing the code in the `admin.py`.

`recipes/admin.py`
```python
admin.site.register(Recipe)
admin.site.register(Category)
admin.site.register(Ingredient)
admin.site.register(Instruction)
```

Right now all we're doing is registering the models to the admin page. To customize any model you need to create a new class that inherits from `ModelAdmin`. Since we need to customize the recipes admin page we'll create a class for it and register it to the admin page. 

`recipes/admin.py`
```python
class RecipeAdmin(admin.ModelAdmin):
	pass


admin.site.register(Recipe, RecipeAdmin)
```

There are many attributes that you can override in the `RecipeAdmin`. Take a look at the [https://docs.djangoproject.com/en/2.2/ref/contrib/admin/](documentation) and see all the options that you have.

The option that we'll be needing is `inlines`. `inlines` takes a list that specifies which models should be shown inline to the `Recipe` model when creating or modifying the `Recipe` object. 

This is how you specify the `inline`
`recipes/admin.py`
```python
class IngredientInline(admin.TabularInline):
    model = Ingredient
    extra = 1


class InstructionInline(admin.TabularInline):
    model = Instruction
    extra = 1


class RecipeAdmin(admin.ModelAdmin):
	inlines = [IngredientInline, InstructionInline]
```

The best way to understand what just happened is go to the admin page and try to add a new recipe. The difference is huge and it's much user friendly.


It is worth mentioning that this is only possible when there is a `ForeignKey` relationship between the two models. Otherwise, you wouldn't be able to use the `inline`.

Also, if you're wondering what the `extra` means in the `IngredientInline` and the `InstructionInline`, this is to specify how many empty forms for the ingredient and instruction would you like to be initially shown.


I want you to try and change the `extra` and see the difference it makes, remove one of the inlines and see the change. Experiment with it to get a better idea of what we need to build for our create recipe page.










