
Let's start by explainig the difference between the two and what each one is used for.


### Static Files

​​​Static content is any content that can be delivered to an end user without having to be generated, modified, or processed.

Static files can be images, JS files, CSS files, and even fonts.

Example:

You go on https://codeunicorn.io/ and you see codeunicorns's logo at the top. The logo is on all pages and unchanged. That's a static image file.

You can read more about static files [here](https://blog.stackpath.com/static-content/).

Just to make sure this is understood. static files are files that you manually as the developer will add to your project.

To setup static files to work properly we must go to our `settings.py` file and provide it with a `STATIC_URL` and a `STATIC_ROOT`.

`setting.py`
```python
STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'static')
```

`STATIC_URL` is the URL pattern that you can use to access the static files and `STATIC_ROOT` is the path to where the static files are stored.

Next we need to actually use the `STATIC_URL` and `STATIC_ROOT` to dynamically create URLs to access any file with in a static directory.

In the `urls.py` file in the inner project folder you shoul write the following lines of code, which specifies where project should look for the static files.

```python
...

from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    ...
]

urlpatterns+=static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
```

Any static file you want to keep in your project must be inside a `static` folder. You can have a `static` folder inside any of your Django apps. You can find a `static` folder inside of the `recipes` app.

How would we actually use anything from our `static` folder? Well, in any HTML file you'd like to call static content into, you first have to `load static` then you can refer to any static file using the static tag.

The `static` tag is used in the `base.html`. Go try to find it, take a look at it. Just take things slow...no need to rush things.


___

### Media Files

Media files are images or files that are dynamically generated.

What do we mean by dynamically generated? They're files that are uploaded by users.

For example, if you try to add a new recipe and you choose an image for that recipe, that image is a media file. 

To setup media files to work properly we must go to our settings.py file and provide it with a `MEDIA_URL` and a `MEDIA_ROOT`.

`settings.py`
```python
MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
```

`MEDIA_URL` is the URL pattern that you can use to access the media files and `MEDIA_ROOT` is the path to where the media files are stored.

Next we need to actually use the `MEDIA_URL` and `MEDIA_ROOT` to dynamically create URLs to access any file with in a media directory.

`urls.py`
```python
...

from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    ...
]

urlpatterns+=static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

Finally you'll want to install Pillow using pip so that your project can take dynamically generated images.
```shell
(env)$ pip install Pillow
```

All of these steps have already been done for you. So, there is no need to actually install anything.
