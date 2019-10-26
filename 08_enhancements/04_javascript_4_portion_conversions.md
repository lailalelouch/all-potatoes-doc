## Trello
> Move card `As a user, I can change the recipes portions and get the correct amounts accordingly` from the `Backlog` to the `Doing` list.
___


In this section we'll be manipulating the ingredients amounts in the recipe details page. So, if the user wants to double everything up they just have to change the portion and the amounts will double up accordingly. All of this will happen without refreshing the page which means we'll be using javascript.

First, we'll need to add a text input for the portion which will have the value of 1 by default and then. theu ser can change this number however he wants.


I decided to add the input next to the `Ingredients` headline.

`recipes/templates/recipe_details.html`
```html
<h5 class="navbar-border pb-2 pl-4">
	Ingredients 
	<div class="float-right ">
		Portion:<input type="number" id="portion" value="1" class="col-6" onkeyup="changeAmounts()">
	</div>
</h5>
```

Whenever the number inside the text input changes, a function that changes the amounts accordinly should be called. `onkeyup` is whenever you press on a keyboard key, the function will be called. So, whenever the user changes the number in the text field the function `changeAmounts()` should be called.

```
<input type="number" id="portion" value="1" class="col-6" onkeyup="changeAmounts()">
```

We also need to add an id to the ingredient's amount so that the function can retrieve it and change it.
`recipes/templates/recipe_details.html`
```html
{% for ingredient in recipe.ingredients.all %}
	 <li><span id="ingredient-amount-{{ingredient.id}}">{{ingredient.amount}}</span> {{ingredient.measure}} - {{ingredient.ingredient}}</li>
{% endfor %}
```

All I did was wrap the amount with a `span` tag and gave it and id `ingredient-amount-{{ingredient.id}}`. The id will be different for each ingredient since we added the ingredient id to the span tag id.

Now, we can start with function. Add it anywhere inside the `<script>` tag.

`recipes/templates/recipe_details.html`
```javascript
	function changeAmounts(){
		portion = $("#portion").val();
		{% for ingredient in recipe.ingredients.all %}
			$("#ingredient-amount-{{ingredient.id}}").html({{ingredient.amount}}*portion);
		{% endfor %}
	}
```

The first thing we did in the function was get the portion which the number the user wrote in the text input.
```js
portion = $("#portion").val();
```

Then, we looped through the ingredients and retrieved the `span` tag that we added using the id. Then we modified the `html` that is inside the tag to match the new portions.


## Trello
> Move card `As a user, I can change the recipes portions and get the correct amounts accordingly` from the `Doing` to the `Review` list if someone will needs to review it, otherwise move it to `Done`.
___

## Git

Create a new checkpoint

```shell
$ git add .
$ git commit -m "finished changing portions functionality"
$ git push
```
