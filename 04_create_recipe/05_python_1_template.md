The template is slightly easier than the view. Actually, the template is so much easier than the view. We've already done most of it, so we'll just tweak it from there.

First, we need to make this page inherit from the `base.html`. Then, we'll put our forms into the html `form` tag. 

Let's see how that's done

`recipes/templates/create_recipe.html`
```html
{% extends 'base.html' %}
{% load crispy_forms_tags %}

{% block content %}
	<form action="." method="POST" enctype="multipart/form-data">
		{% csrf_token %}
		{{form|crispy}}
		{{ingredient_form|crispy}}
		{{instruction_form|crispy}}
		<button type="submit">Add recipe</button>
	</form>
{% endblock content %}
```
I'll explain everything that happened line by line. If you don't care about these details, just skip down until the dancing potatoe.

```html
{% extends 'base.html' %}
{% load crispy_forms_tags %}
```

In these two lines, we're first extending the `base.html` so that this template inherits it. The second line we're importing something called `crispy forms` which is used to style our forms in an easy and quick way. 

```html
{% block content %}
	<form action="." method="POST" enctype="multipart/form-data">
		{% csrf_token %}
		...
		<button type="submit">Add recipe</button>
	</form>
{% endblock content %}
```

The `block` and `endblock` tags are used to specify which blocks of the `base.html` file are we filling. If you go into `base.html`, you'll find there is a block called `content`. So, In here we're specifying that we want to fill the `content` block with the form.

Then we started with the `form` tag. It takes three attributes `action`, `method`, and `enctype`. `action` is used to specify which view should the data submitted in this form be sent to. `.` as the `action` says that send it to the same view. `method` is self explanatory, when the form is submitted, we want a `post` request to be sent. Finally, `enctype` is used when there are files being submitted through the form. In this case, the recipe's image needs to be uploaded so we need to set the encryption type.

```html
{{form|crispy}}
{{ingredient_form|crispy}}
{{instruction_form|crispy}}
```

If you've notices, I've added `|crispy` to all the forms. This is how you use `crispy forms` to make your forms crisp. Just remove and add `crispy` and see the difference.

![dancing potato](https://media1.tenor.com/images/61497871ab091f01703a3f1a624fb3c4/tenor.gif?itemid=11684043)

One more thing, you must've noticed that you can only add one ingredient and one instruction to the recipe. The reason behind that is the `extra` that we set to 1 in the `inline formsets`. If you go to the forms and change it to any other number, you'll be able to add more ingredients and instructions. But, that's not very dynamic. We want the user to be able to add or remove an ingredient as he wishes just like what we did when we customized the admin page.

And that's what we're going to do in the next part.