You might have notices that there is a `urls.py` file in each app. This is just a way to organize our paths/urls instead of having them all in one file (`urls.py` in the inner project folder).

In both `authentication/urls.py` and `recipes/urls.py` paths are written just like you normally write them in the `crumble_mumble/urls.py`. However, after that you need to include them in the `crumble_mumble/urls.py` file.

`crumble_mumble/urls.py`
```python
urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('recipes.urls')),
    path('', include('authentication.urls')),
]
```

And that's how it's done. So now all the paths written in both `urls.py` files is included in this file. If you're wondering why the first parameter for the path is an empty string, it's because I wanted it to be empty. An example would make this a bit clearer.


`authentication/urls.py`
```python
urlpatterns = [
	path('login/', views.login_view, name="login"),
	path('register/', views.register, name="register"),
	path('logout/', views.logout_view, name="logout"),
]
```

In our current case this url `127.0.0.1:8000/login/` will take you to the login page.

Let's change the `all_pottatoes/urls.py` and see the difference
`all_pottatoes/urls.py`
```python
urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('recipes.urls')),
    path('auth/', include('authentication.urls')),
]
```

Now, if we want to access the login page we'll use this url `127.0.0.1:8000/auth/login/`

