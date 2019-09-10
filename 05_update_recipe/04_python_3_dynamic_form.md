To make the deletion work in the dynamic form we need to change a few things. the first thing we'll change is the forms. 

`recipes/forms.py`
```py
IngredientFormSet = inlineformset_factory(Recipe, Ingredient, exclude=['recipe'], extra=1, can_delete=True)
InstructionFormSet = inlineformset_factory(Recipe, Instruction, fields=['description'], extra=1, can_delete=True)
```

Add the `can_delete` attribute to both `inlineformset_factory`s and set it to `True`. This allows the ingredient and instruction that the user chooses to remove from the form to be actually deleted from the database.


The second change we'll be making is in the4 template itself. We need to add `{{form.DELETE}}` to each form. This is a hidden field that will allow the `ingredient` and `instruction` object to be marked for deletion. Also, add `{{form.id}}` before each form.

`recipes/templates/update_recipe.html`
```html
...
		    {% for form in ingredient_form %}
		    	{{form.id}}
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
			        {{form.DELETE}}
			    </div>                      
		    {% endfor %}
...
            {% for form in instruction_form.forms %}
            	{{form.id}}
                <div class="inline {{ instruction_form.prefix }}">
                	{{ form.description|as_crispy_field}}
                	{{form.DELETE}}
                </div>
            {% endfor %}
```

And that's it. 

I'll add some styling, just because I want our forms to look prettier. So, the first thing, I don't want the textarea for the instruction description to be this big. To specify the size, we can add a widget to the `inlineformset_factory` so that even the create page will get the same affect.

`recipes/forms.py`
```py
from django.forms import ModelForm, Textarea
from django.forms.models import inlineformset_factory
from .models import Recipe, Instruction, Ingredient

...

InstructionFormSet = inlineformset_factory(
	Recipe, 
	Instruction, 
	fields=['description'],
	widgets = { 'description': Textarea(attrs={'cols': 60, 'rows': 2})},
    extra=1,
    can_delete=True
    )
```

And that's the final styling for this page. Take your time to read it.

`recipes/templates/update_recipe.html`
```html
{% extends 'base.html' %}
{% load crispy_forms_tags %}

{% block content %}
	<script type="text/javascript">
    $(function() {
        $('.inline.{{ ingredient_form.prefix }}').formset({prefix: '{{ ingredient_form.prefix }}'});
        $('.inline.{{ instruction_form.prefix }}').formset({prefix: '{{ instruction_form.prefix }}'});
    })
	</script>

	<div class="card m-auto col-11 p-0">
		<div class="card-body">
	    	<h2 class="card-title">Edit Recipe</h2>
	        <form action="." method="post" autocomplete="off" enctype="multipart/form-data">
	            {% csrf_token %}
	            <div class="row">
		            <div class="col-lg-4 col-sm-11">
		                {{ form|crispy }}
		            </div>
		       		<div class="col-lg-7 col-sm-11 pl-5 mb-5 ml-5 pl-5">
			            <fieldset>
			                <legend class="card-title">Ingredients</legend>
			                {{ ingredient_form.non_form_errors }}
			                {{ ingredient_form.management_form }}
			                {% for form in ingredient_form %}
			                
			                	{{ form.id }}
			                    <div class="inline {{ ingredient_form.prefix }} row col-12">
			                    	<div class="mr-1">
			                        	{{ form.measure|as_crispy_field }}
			                    	</div>
			                    	<div class="mx-1">
			                        	{{ form.amount|as_crispy_field }}
			                        </div>
			                        <div class="mx-1">
			                        	{{ form.ingredient|as_crispy_field }}
			                        </div>
			                        {{ form.DELETE }}                  
			                    </div>
			                {% endfor %}
			            </fieldset>
			            <fieldset>
			                <legend class="card-title mt-3">Instructions</legend>
			                
			                {{ instruction_form.non_form_errors }}
			                {{ instruction_form.management_form }}
			                {% for form in instruction_form %}

			                	{{ form.id }}
			                    <div class="inline {{ instruction_form.prefix }} row col-12">
			                        {{ form.description|as_crispy_field}}
			                        {{ form.DELETE }}
			                    </div>
			                {% endfor %}
			            </fieldset>
			        </div>
			    </div>
	            <input type="submit" value="Update recipe" class="btn btn-outline-beige col-6 d-inherit m-auto" />
	        </form>
	    </div>
	</div>
{% endblock content %}

```

## Trello
> Move card `As a logged in user, I can update my recipes` from the `Doing` to the `Review` list if someone will needs to review it, otherwise move it to `Done`.
___

## Git

Create a new checkpoint

```shell
$ git add .
$ git commit -m "finished update recipe functionality"
$ git push
```
___


