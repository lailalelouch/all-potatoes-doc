A `base` file is used to decrease redundant code. You can find this file in `recipes/templates/base.html`. This template has the code or the html that will be repeated in all the other templates. For example, the `navbar` should appear in all pages. So, we'll just add it in this file and all the other `templates` inherit from the `base.html`.
___

`recipes/templates/base.html`
```
...
    {% block title %}
    {% endblock title %}
...
    {% block content %}
    {% endblock content%}
...
```

When you open the file, you'll find these two blocks. The blocks are the parts that will be filled by the templates that will inherit from the `base.html`. So, everything in this file will be shared by all `templates` inheriting from it, but each `template` will have different code written in these blocks. 

I'd recommend that you take a look at `recipes/templates/recipes_list.html` and just take a look at how it's inheriting from the `base.html` and how it's using the blocks.

___

`recipes/templates/base.html`
```
...
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css" integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm" crossorigin="anonymous">
    <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.7.2/css/all.css" integrity="sha384-fnmOCqbTlWIlj8LyTjo7mOUStjsKC4pOpQbqyi7RrhN7udi9RwhKkMHpvLbHG9Sr" crossorigin="anonymous">
    <link rel="stylesheet" type="text/css" href="{% static 'css/colors.css' %}">
    <link rel="stylesheet" type="text/css" href="{% static 'css/general.css' %}">
    <script
  src="https://code.jquery.com/jquery-3.3.1.min.js"
  integrity="sha256-FgpCb/KJQlLNfOu91ta32o/NMZxltwRo8QtmkMRdAu8="
  crossorigin="anonymous"></script>
...
...

<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.12.9/umd/popper.min.js" integrity="sha384-ApNbgh9B+Y1QKtv3Rn7W3mgPxhU9K/ScQsAP7hUibX39j7fakFPskvXusvfa0b4Q" crossorigin="anonymous"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/js/bootstrap.min.js" integrity="sha384-JZR6Spejh4U02d8jOt6vLEHfe/JQGiRRSQQxSfFWpi1MquVdAyjUar5+76PVCmYl" crossorigin="anonymous"></script>
```

There are many `script`s and `link`s in the `base.html`. These are files that we're trying to import into our project. For example, the below link is `bootstrap`

```
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css" integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm" crossorigin="anonymous">
```

The below two `link`s might be a bit different though, because we're linking the `base.html` to static files that we created and added to the `static` folder in the `recipes` app. These are two `css` files that I created to write my own custom `css` classes to use throughout the project. 


```
    <link rel="stylesheet" type="text/css" href="{% static 'css/colors.css' %}">
    <link rel="stylesheet" type="text/css" href="{% static 'css/general.css' %}">
```

do notice that at the top of the file, you'll find this
```
{% load static %}
```
You need to load/import the static tag, then you can use the static tag to call static content into your `html` file.

___

The last part that might be new to you is this

`recipes/templates/base.html`
```
{% include 'navbar.html' %}
```

What this does is, it finds the file called `navbar.html` in the `templates` folder and replaces this single line with everything inside of the `navbar.html` file. Basically, it brings the file's content here.
