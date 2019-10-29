So far we've got 4 models:

 * Category
 * Recipe
 * Instruction
 * Ingredient

These are very straight forward. Each `Recipe` belongs to a `Category`. Each `Recipe` has multiple ingredients and instructions.

The part that might be confusing is the `slug`.

What is a slug? It's a part of the URL which identifies a particular page on a website in a form readable by users.

A slug is a more readable way to give each object a unique identifier. Usually the `id` is the unique identifier. There is only one recipe with id 1. However, that is not very user friendly and that is where slugs come.

If recipe with id 1 is called `Banana cream pie`, the slug would be `banana-cream-pie`. If we have another recipe with id 24 called `Banana cream pie` the slug would be `banana-cream-pie-1` which makes it unique for each recipe.

You can see a slug in action...wait, let's take a moment to appreciate this slug in action


![slug in action](https://media.giphy.com/media/11zeCgKZ1MaNuE/giphy.gif)

Let's do this again. You can see the slug in action if you open the `crumblemumble` website and click to see the details of any recipe and notice the url. What you're seeing there is the slug which is more readable for the user to see than the id.

If you're interested in how slugs are generated, the next part is for you.

The slug is automatically generated from the name of each recipe.

`recipes/models.py`
```
def create_slug(instance, new_slug=None):
    slug = slugify (instance.name)
    if new_slug is not None:
        slug = new_slug
    qs = Recipe.objects.filter(slug=slug)
    if qs.exists():
        try:
            int(slug[-1])
            if "-" in slug:
                slug_list = slug.split("-")
                new_slug = "%s%s"%(slug[:-1],int(slug_list[-1])+1)
            else:
                new_slug = "%s-1"%(slug)
        except:
            new_slug = "%s-1"%(slug)
        return create_slug(instance, new_slug=new_slug)
    return slug
```

This function takes a recipe object (`instance`) and slugifies its name. So, if the name was `chocolate chip cookies`, the slug would be `chocolate-chip-cookies`. You'll also notice that this function is premade by django and we had to import it.


```python
slug = slugify(instance.name)
```
It then checks if there is another recipe with the same slug, which means there was another recipe with the same name. If there was, then it will number this new slug. For example, let's say there are three recipes in our website that have the title `chocolate-chip-cookies`. The first one created will have this slug `chocolate-chip-cookies`, the second one this slug `chocolate-chip-cookies-1`, and the third one this `chocolate-chip-cookies-2`. These numbers were added just to make sure that each recipe has a unique slug.

We still haven't called this function anywhere, right?
We want a slug to be added to each new recipe we create. So, we could just call it in the create recipe view (which we still haven't made). However, there is even a better way to do it.

We'll be using something called `signals`. A signal is used to trigger an action when a certain event happens.
In our case, the action that we want triggered is the `create_slug` function. 
When do we want it? when a recipe object is being saved.

Let's see how we can create this signal
`recipes/models.py`
```python
from django.db.models.signals import pre_save
from django.dispatch import receiver

...
@receiver(pre_save, sender=Recipe)
def generate_slug(instance, *args, **kwargs):
    if not instance.slug:
        instance.slug=create_slug(instance)
```

What made this function a signal is the `receiver` decorator above it. The `receiver` takes two parameters, the type of the `signal` and the `sender` (a model).

What does the `receiver` do? It waits for an event to happen and then triggers an action.

In this case, the event it waits for is right before Recipe object gets saved.

The action that gets triggered is the `generate_slug` function which takes instance as a parameter which is a recipe object.

All we did in the function was check if the object has a slug. If it doesn't create one.
