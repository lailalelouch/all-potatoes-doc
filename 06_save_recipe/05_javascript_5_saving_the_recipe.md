The last part left is to create the button that does the saving. We'll add the button in the details page. So, let's go there and finish this chapter.


Start by adding this button anywhere you want in the details page.
`recipes/templates/recipe_details.html`
```html
<button class="btn btn-light">
	<i class="far fa-save" id="save-btn"></i>
</button>
```

What should happen is whenever this button is `click`ed, the `save_recipe` view is called. To make this happen we need team work because team work makes the dream work. 

![teamwork](https://media.giphy.com/media/dSetNZo2AJfptAk9hp/giphy.gif)


So, this is where javascript comes in and gives us a hand. So, just before the `endblock`, add an empty javascript function.

`recipes/templates/recipe_details.html`
```js
...

<script type="text/javascript">
	function save() {

	}
</script>
{% endblock content %}
```

Then, make the button call this function whenever clicked
```html
<button class="btn btn-light" id="save-btn" onclick="save()">
	<i class="far fa-save"></i>
</button>
```

Now most of the work will be inside of the function. We'll make an `ajax` request to the `save_recipe` view. Before we do that, we need to clarify a few things. One, `AJAX` is a way for us to make requests without refreshing the page. Two, in order to use `AJAX`, we need to have have jquery in our project and make sure it's not the minified version. In this project, you don't need to worry about this, we've already done that in the `base.html`...I know we're spoiling you.


![spoil](https://media.giphy.com/media/DRY5URVQgjpgk/giphy.gif)

Let's go back to the function
`recipes/templates/recipe_details.html`
```js
...

<script type="text/javascript">
	function save() {
		$.ajax(
	        {
	            type:'GET',
	            url: '{% url "save-recipe" recipe.id %}',
	            error: function(){
	                console.log('error');
	            },
	            success: function(data){
            		if (data.saved){
	            		$("#save-btn").addClass("text-dark");
	            		$("#save-btn").removeClass("text-light");
	            	}else{
	            		$("#save-btn").addClass("text-light");
	            		$("#save-btn").removeClass("text-dark");
	            	}
	                
	            },
	        }
	    );
	}
</script>
{% endblock content %}
```

Let's break down the ajax request. 
 * `type`: The type of request we'd like to make.
 * `url`: The URL we're making a request to which in this case was `save-recipe`.
 * `error`: What would happen in case the request was unsuccessful.
 * `success`: What would happen in case the request was successful. The function success has a parameter called `data` which is the data received in the response. If you go back to the `save_recipe` view, you'll see that the response was a dictionary with an element with key `saved` which was a `Bool`. So, depending on the value of `saved` we'll change the color of the save icon by adding and removing classes.


 There is still one problem and that is whenever you refresh the page the color of the icon is black regardless of whether it was saved or not. To fix this problem, we need to know whether the recipe is saved or not and change the colors accordingly the moment the page is loaded. So, we need to modify the `recipe_details` view.

`recipes/views.py`
```py
 def recipe_details(request, recipe_slug):
	recipe = Recipe.objects.get(slug=recipe_slug)
	if SavedRecipe.objects.filter(recipe=recipe, user=request.user):
		saved = True
	else:
		saved = False
	context = {
		"recipe" : recipe,
		"saved" : saved
	}
	return render(request, "recipe_details.html", context)
```

`recipes/templates/recipe_details.html`
```html
	<button class="btn btn-light" id="save-btn" onclick="save()">
		<i class="far fa-save
					{% if saved %}
						text-dark
					{% else %}
						text-light
					{% endif %}" ></i>
	</button>
```

Now, everything is working perfectly....unless you've got some errors....I'll wait, just go and fix them. We're not going anywhere.


If everything is actually working perfectly, there is one more thing that's I'd like to fix. I want the save button to only show if this isn't the user's recipe.

`recipes/templates/recipe_details.html`
```html
{% if user == recipe.owner %}
	<a href="{% url 'update-recipe' recipe.slug %}">Edit</a>
{% else %}
	<button class="btn btn-light" id="save-btn" onclick="save()">
		<i class="far fa-save
					{% if saved %}
						pink-text
					{% else %}
						beige-text
					{% endif %}" ></i>
	</button>
{% endif %}
```

## Trello
> Move card `As a logged in user, I can save other peoples recipes` from the `Doing` to the `Review` list if someone will needs to review it, otherwise move it to `Done`.
___

## Git

Create a new checkpoint

```shell
$ git add .
$ git commit -m "finished saving recipes functionality"
$ git push
```
___
