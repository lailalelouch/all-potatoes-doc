Making the form dynamic will require some javascript. But, I'm not a big fan of javascript and there is no need to reinvent the wheel.

So, I used something called `google`, I don't know if you've heard of it but it's very useful. I found [this](https://code.google.com/archive/p/django-dynamic-formset/). This is all the javascript that we need to make our forms dynamic.

Go to [this url](https://code.google.com/archive/p/django-dynamic-formset/downloads) and download the latest version.

unzipe the folder and go into the `src` folder. You should find two files and one of them is called `jquery.formset.js`. Move this file into the `static` folder. `recipes/static`. Now that the file is in our project we need to link it to our templates. Since, it's a static file, we can link it just like we linked the `css` files in the `base.html` but since this is a `js` file we'll use the `script` tag.


Add the following line anywhere in the `head`

`recipes/templates/base.html`
```html
<head>
	...
	<script type="text/javascript" src="{% static 'jquery.formset.js' %}"></script>
	...
</head>
```

Now, we'll go to the templates that need dynamic forms. So, in the `create_recipe` template, add this at the start of the `content` block.

`recipes/templates/create_recipe.html`
```html
{% block content %}
	<script type="text/javascript">
    $(function() {
        $('.inline.{{ ingredient_form.prefix }}').formset({
            prefix: '{{ ingredient_form.prefix }}'
        });
        $('.inline.{{ instruction_form.prefix }}').formset({
            prefix: '{{ instruction_form.prefix }}'
        });
    })
	</script>
...
{% endblock content %}
```

According to the documentation, we need to call the `formset()` function for each formset that we have and give it the formset's prefix. So, first we need to find the formset and then call the function. 

This is how we're finsing the `formset`. The ingredient formset has class `inline` and class with the value of whatever the formset prefix is. In this case the prefix for the ingredient formset is `ingredients`. you can think of the prefix as the forms name.

```javascript
$('.inline.{{ ingredient_form.prefix }}')
```

So, the first thing we need to do is give those classes to our formsets so the magic can work. 


After you add those classes to the template, the form should now be dynamic.

`recipes/templates/create_recipe.html`
```html
		<div class="inline {{ ingredient_form.prefix }}">
			{{ingredient_form|crispy}}
		</div>
		<div class="inline {{ instruction_form.prefix }}">
			{{instruction_form|crispy}}
		</div>
```

However, we want to customize this more. For example, after each form you'll see `Delete` written there and we don't want that to be shown to the user. Also, For the ingredient form, I personally prefer to have the fields next to each other rather than under each other. So, to do this we need to open up the form more.

Let's start with the ingredients form

replace
```html
		{{ingredient_form|crispy}}
```

with
```html
	    {{ ingredient_form.non_form_errors }}
	    {{ ingredient_form.management_form }}

	    {% for form in ingredient_form %}
	    	<div class="row">
		    	<div class="mr-1">
		        	{{ form.measure|as_crispy_field }}
		    	</div>
		    	<div class="mx-1">
		        	{{ form.amount|as_crispy_field }}
		        </div>
		        <div class="mx-1">
		        	{{ form.ingredient|as_crispy_field }}
		        </div>
		    </div>                      
	    {% endfor %}
```

The first line is there so that if there are non form related errors. The second line has to be there or you'll get an error because it's required to manage the collection of forms in the formset.

Next is the for loop. The ingredient_form is a formset which means it is a collection of forms and we want to diaplay all these forms. So, we're looping through all the forms in this formset and displaying the fields for each one. The last thing is to style the field using `crispy forms` we used `|as_crispy_field`.


For the instruction form replace
```html
		{{instruction_form|crispy}}
```

with
```html
		<div class="inline {{ instruction_form.prefix }}">                    
		    {{ instruction_form.non_form_errors }}
		    {{ instruction_form.management_form }}

		    {% for form in instruction_form.forms %}
		        {{ form.description|as_crispy_field}}
		    {% endfor %}
		</div>
```

All that's left now is to style this page. Also, if you want to style the `remove` and `add another` buttons, you can do that in the `jquery.formset.js` that you added to the `static` folder. If you go to the bottom of the file, you should see this:


`recipes/static/jquery.formset.js`
```js
    $.fn.formset.defaults = {
        prefix: 'form',                  // The form prefix for your django formset
        formTemplate: null,              // The jQuery selection cloned to generate new form instances
        addText: 'add another',          // Text for the add link
        deleteText: 'remove',            // Text for the delete link
        addCssClass: 'add-row',          // CSS class applied to the add link
        deleteCssClass: 'delete-row',    // CSS class applied to the delete link
        formCssClass: 'dynamic-form',    // CSS class applied to each form in a formset
        extraClasses: [],                // Additional CSS classes, which will be applied to each form in turn
        added: null,                     // Function called each time a new form is added
        removed: null                    // Function called each time a form is deleted
    };
```

You can added any classes you want to add to the button here by just adding space and the class name to `addCssClass` or `deleteCssClass`. You can even change the text shown on the button.

And you're finally done from the create recipe page. What a ride!!!
