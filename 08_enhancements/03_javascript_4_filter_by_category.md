## Trello
> Move card `As a user, I can filter the recipes by category` from the `Backlog` to the `Doing` list.
___


In this section we'll be modifying the recipes list page so that the user can filter through the list without reloading the page. If you go to the homepage in the live demo, you should be able to see what we're going to start building.

I'll give you a bit of introduction of what we need to do to make this work. So, first thing, since this is going to work without reloading the page then there has to be javascript involved. So, there is a button for each category and whenever a button is clicked, the recipes are filtered. So that means each time the button is clicked, a javascript function is being called.

Let's do this part 

`recipes/templates/recipes_list.html`
```HTML
{% block content %}
		<button class="btn btn-outline-dark active" id="btn-all">All</button>
		{% for category in categories %}
			<button class="btn btn-outline-dark" id="btn-{{category.name}}" onclick="filter('{{category.name}}')">{{category.name}}</button>
		{% endfor %}
...
```

All we did here is specify a function to be called whenever this button is clicked. So, when any of the categories buttons are clicked a javascript function called `filter` will be called. This function take the category's name as a parameter. Also, do notice the ids of the button, these will play a part.


This is the javascript function that will make everything works. Just add it at the bottom of the file.

`recipes/templates/recipes_list.html`
```js
{% block script %}
	<script type="text/javascript">
		$("#btn-all").click(function() {
			$("#btn-all").addClass("active");
			$("#recipes-list div").show()
		});

		function filter(category){
			$("#btn-"+category).addClass("active");
			$("#recipes-list div").show().filter(function() {
				  var recipeCategory = $(this).find("span").text();
				  if (category !== recipeCategory)
				  	return true;
				}).hide();
			$("#btn-all").removeClass("active");
			{% for category in categories %}
				if ("{{category.name}}" !== category)
					$( "#btn-"+'{{category.name}}' ).removeClass("active");
			{% endfor %}
		}
	</script>
{% endblock script %}
```

Let's break it down. We'll start with the first button `all`
```js
		$("#btn-all").click(function() {
			$("#btn-all").addClass("active");
			$("#recipes-list div").show();
			{% for category in categories %}
				$( "#btn-"+'{{category.name}}' ).removeClass("active");
			{% endfor %}
		});
```

Here we're using `jquery`. basically anything with the dollar sign `$` is jquery. 

```js
$("#btn-all").click(function() {

});
```

This first part means when the button with id `btn-all` is clicked, run this function. 

Note: `#` this sign before a string means the string is an id. `.` a dot means it's a class. If there was nothing before the string, that means it's an html tag.


Let's go into the function next

```js
			$("#btn-all").addClass("active");
			$("#recipes-list div").show()
			{% for category in categories %}
				$( "#btn-"+'{{category.name}}' ).removeClass("active");
			{% endfor %}
```

When the button is clicked add the class `active` to that button (this is is the class that colors the button). At the same time, it will show all the `div`s inside the tag with id `recipes-list` which is the tag that has all recipes in it. Finally, it will remove the class active for all the other buttons.


Let's see the second part of the function.

```js
		function filter(category){
			$("#btn-"+category).addClass("active");
			$("#btn-all").removeClass("active");
			{% for category in categories %}
				if ("{{category.name}}" !== category)
					$( "#btn-"+'{{category.name}}' ).removeClass("active");
			{% endfor %}

			$("#recipes-list div").show().filter(function() {
				  var recipeCategory = $(this).find("span").text();
				  if (category !== recipeCategory)
				  	return true;
				}).hide();
		}
```

This is the `filter` function that the catogory buttons were calling and sending their category as a parameter.


The first thing we're doing in this function is changing the buttons classes, so that only the selected category button is highlighted. We did that by adding the `active` class to the selected category and removing it from all the others.


Next we start with the actual filtering. 

```js
			$("#recipes-list div").show().filter(function() {
				  var recipeCategory = $(this).find("span").text();
				  if (category !== recipeCategory)
				  	return true;
				}).hide();
```

First, we're getting all the `div`s under the tag with `recipes-list` id and making them visible using `show()`. Next, we're using `filter`. What `filter` does is it loops through all the `div`s and for each one, it runs the function inside of it. At the end, it returns all the `div`s that returned true in this function. Finally, we will use `hide()` to hide all the `div`s returned from the `filter`. 


There is one thing I need to emphasize on in case it wasn't clear. The `div`s inside the tag with the `recipes-list` id, are the recipes. So, each `div` is one recipes card that is shown to the user.


Let's go over what is happening inside of the filter:

```js
function() {
	  var recipeCategory = $(this).find("span").text();
	  if (category !== recipeCategory)
	  	return true;
	}
```

For each `div` we're getting the tag `span` that's in it (`$(this).find("span")`) and then getting the text inside of it `(.text();)`. If take a look at the `recipes_list.html`, you'll find this `<span hidden>{{recipe.category.name}}</span>`. So, each recipe has the category's name in a `hidden` `span` tag. It's hidden because we don't want the user to see it, we just need it for the filter.


After we got the the recipes category, we check if this category is the same as the category that was sent to the `filter` function as a parameter by the button. If it wasn't return `true`. We returned `true` because we will hide all the recipes/divs that return `true`.


## Trello
> Move card `As a user, I can filter the recipes by category` from the `Doing` to the `Review` list if someone will needs to review it, otherwise move it to `Done`.
___

## Git

Create a new checkpoint

```shell
$ git add .
$ git commit -m "finished filtering recipes by category functionality"
$ git push
```
