## Trello
> Move card `As a user, I can filter the recipes by category` from the `Backlog` to the `Doing` list.
___



In this section we'll be modifying the recipes list page so that the user can filter through the list without reloading the page. If you go to the homepage in the live demo, you should be able to see what we're going to start building.

I'll give you a bit of introduction of what we need to do to make this work. So, first thing, since this is going to work without reloading the page then there has to be javascript involved. So, as you saw in the live demo, there is button for each category and whenever a button is clicked, the recipes are filtered. So that means each time the button is clicked, a javascript function is being called.

Let's do this part 

`recipes/templates/recipes_list.html`
```HTML
{% block content %}
		<button class="btn btn-outline-dark active" id="btn-all">All</button>
		{% for category in categories %}
			<button class="btn btn-outline-dark" id="btn-{{category.name}}">{{category.name}}</button>
		{% endfor %}
...
```

We haven't done anything special here. The only thing you should focus on are the ids of the buttons. Because using the ids, we'll know which button was pressed and filter the recipes accordingly.


This is the javascript function that will make everythin work. Just add it at the bottom of the file.

`recipes/templates/recipes_list.html`
```js
{% block script %}
	<script type="text/javascript">
		$("#btn-all").click(function() {
			$("#btn-all").addClass("active");
			{% for category in categories %}
				$( "#"+'{{category.name}}' ).show( "fast" );
				$( "#btn-"+'{{category.name}}' ).removeClass("active");
			{% endfor %}
		});
		{% for category in categories %}
			$( "#btn-"+'{{category.name}}').click(function() {
				$( "#"+'{{category.name}}' ).show( "fast" );
				$("#btn-"+'{{category.name}}').addClass("active");
				$("#btn-all").removeClass("active");
				var category = '{{category.name}}';
				{% for category in categories %}
					if ('{{category.name}}' !== category){
						$( "#"+'{{category.name}}' ).hide( "fast" );
						$( "#btn-"+'{{category.name}}' ).removeClass("active");
					}
				{% endfor %}
			});
		{% endfor %}
	</script>
{% endblock script %}
```

Let's break it down. We'll start with the first button `all`
```js
		$("#btn-all").click(function() {
			$("#btn-all").addClass("active");
			{% for category in categories %}
				$( "#"+'{{category.name}}' ).show( "fast" );
				$( "#btn-"+'{{category.name}}' ).removeClass("active");
			{% endfor %}
		});
```

Here we're using `jquery`. basically anything with the dollar sign `$` is jquery. 

```js
$("#btn-all").click(function() {

});
```

This first part means. When the button with id `btn-all` is clicked, run this function. Let's go into the function next

```js
			$("#btn-all").addClass("active");
			{% for category in categories %}
				$( "#"+'{{category.name}}' ).show( "fast" );
				$( "#btn-"+'{{category.name}}' ).removeClass("active");
			{% endfor %}
```

When the button is clicked add the class `active` to that button. At the same time, it will show all the recipes that with all the categories names. Finally, it will remove the class active for all the other buttons.


Let's see the second part of the function.

```js
{% for category in categories %}
	$( "#btn-"+'{{category.name}}').click(function() {
		$( "#"+'{{category.name}}' ).show( "fast" );
		$("#btn-"+'{{category.name}}').addClass("active");
		$("#btn-all").removeClass("active");
		var category = '{{category.name}}';
		{% for category in categories %}
			if ('{{category.name}}' !== category){
				$( "#"+'{{category.name}}' ).hide( "fast" );
				$( "#btn-"+'{{category.name}}' ).removeClass("active");
			}
		{% endfor %}
	});
{% endfor %}
```

This code is being repeated for all categories. All that's happening here is we're showing all the recipes with the category's name as the id. Then we're adding the class `active` to the clicked button and removing it from the `all` button. However, we still need to remove the `active` class from the rest of the buttons. This is what this part is doing.

```js
	var category = '{{category.name}}';
	{% for category in categories %}
		if ('{{category.name}}' !== category){
			$( "#"+'{{category.name}}' ).hide( "fast" );
			$( "#btn-"+'{{category.name}}' ).removeClass("active");
		}
	{% endfor %}
```

We'll looping through all the categories and if the category is not the clicked button's category, we'll hide all of it's recipes and remove the class `active` from it.


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
___

