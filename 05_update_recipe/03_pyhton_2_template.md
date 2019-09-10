The template for the update view couldn't be any simpler. Just go into the `create_recipe.html` template, copy everything there and paste it into `update_recipe.html`.


`recipes/templates/update_recipe.html`
```html
{% extends 'base.html' %}
{% load crispy_forms_tags %}

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
	<div class="col-7">
		<form action="." method="POST" enctype="multipart/form-data">
			{% csrf_token %}
			{{form|crispy}}

			<legend class="card-title">Ingredients</legend>
		    {{ ingredient_form.non_form_errors }}
		    {{ ingredient_form.management_form }}

		    {% for form in ingredient_form %}
		    	<div class="row inline {{ ingredient_form.prefix }}">
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


			<legend class="card-title mt-3">Instructions</legend>               
            {{ instruction_form.non_form_errors }}
            {{ instruction_form.management_form }}

            {% for form in instruction_form.forms %}
                <div class="inline {{ instruction_form.prefix }}">
                	{{ form.description|as_crispy_field}}
                </div>
            {% endfor %}
			<button type="submit">update recipe</button>
		</form>
	</div>
{% endblock content %}
```

Let's create a url for this view so you can actually see what you've done so far.

`recipes/urls.py`
```py
urlpatterns = [
	...
    path('update/<slug>/', views.RecipeUpdateView.as_view(), name="update-recipe" ),
]
```

While we're at it, let's add a button or link in the recipes details page to take the user to the edit page. This link should only appear for the owner of the recipe

Add this anywhere you want the link to appear in the template

`recipes/templates/recipe_details.html`
```html
	{% if user == recipe.owner %}
		<a href="{% url 'update-recipe' recipe.slug %}">Edit</a>
	{% endif %}
```

Now, the form works just fine but there is still one issue. Deleting the ingredients and instructions. There are a few things that we still need to do to make this work. We'll do that in the next section.