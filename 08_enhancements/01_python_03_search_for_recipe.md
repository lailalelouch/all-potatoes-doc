## Trello
> Move card `As a user, I can search for a recipe by name` from the `Backlog` to the `Doing` list.
___


In this section we'll be adding a search bar that the user can use to search for recipes by name. So, obviously the first step is to add a search bar which is basically just a form. I have decided to put it in the navbar, but you can put it anywhere you want. You can find this form in `navbar.html` surrounded by comment tags, all you have to do is remove these tags.


`recipes/templates/navbar.html`
```html
...
    <form class="form-inline my-2 my-lg-0" action="{% url 'recipes-list' %}" method="get">
      <input class="form-control mr-sm-2" type="search" placeholder="Search" aria-label="Search" name="q">
      <button class="btn btn-outline-light my-2 my-sm-0" type="submit">Search</button>
    </form>
...
```

This form is submitted through a `get` request to the `recipes-list` view. You'll notice that the form has only one input with name `q`. We'll use this name to retrieve whatever was written in this input in the view. 


`recipes/views.py`
```py
...
def recipes_list(request):
	if request.GET.get('q'):
		recipes = Recipe.objects.filter(name__icontains=request.GET.get('q'), private=False)
	else:
		recipes = Recipe.objects.filter(private=False)

	categories = Category.objects.all()

	context = {
		"recipes" : recipes,
		"categories" :categories
	}
...
```

We retrieved the search value sent through the `GET` request by using the input name
```py
request.GET.get('q')
```

Note: the name does not have to be `q`, it could be anything. However, the name used in the input has to be the same as the one used in the view.


There are two cases, either the user is searching for something or not. If they are searching, then `q` will be sent through the `GET` request and we will filter the `recipes` accordingly. Otherwise, we'll just retrieve all the public recipes.


## Trello
> Move card `As a user, I can search for a recipe by name` from the `Doing` to the `Review` list if someone will needs to review it, otherwise move it to `Done`.
___

## Git

Create a new checkpoint

```shell
$ git add .
$ git commit -m "finished searching recipes by name functionality"
$ git push
```
___


And that's it. Searching is just that easy...dancing potato to celebrate.


![dancing potato](https://media1.tenor.com/images/61497871ab091f01703a3f1a624fb3c4/tenor.gif?itemid=11684043)
