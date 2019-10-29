Everything related to authentication is in the `authentication` app.


### Login

`authentication/views.py`
```python
def login_view(request):
	form = UserLogin()
	if request.method == 'POST':
		form = UserLogin(request.POST)
		if form.is_valid():
			username = form.cleaned_data['username']
			password = form.cleaned_data['password']

			auth_user = authenticate(username=username, password=password)
			if auth_user is not None:
				login(request, auth_user)
				return redirect('recipes-list')

	context = {
        "form":form
    }
	return render(request, 'login.html', context)
```

All this view is doing is extracting the information (username and password) from the `UserLogin`.
```python
if form.is_valid():
	username = form.cleaned_data['username']
	password = form.cleaned_data['password']
```
and checking if a user with this username-password combination exists.
```python
auth_user = authenticate(username=username, password=password)
```

If such a user exists, we'll log him in.
```python
if auth_user is not None:
	login(request, auth_user)
```

note: The `UserLogin` is defined in `authentication/forms.py`
___

### Register

`authentication/views.py`
```python
def register(request):
	form = UserRegister()
	if request.method == 'POST':
		form = UserRegister(request.POST)
		if form.is_valid():
			user = form.save(commit=False)
			user.set_password(user.password)
			user.save()
			login(request, user)
			return redirect("recipes-list")
	context = {
        "form":form,
    }
	return render(request, 'register.html', context)
```

First thing that's happening is we're filling the form with the information we received from the `POST` request.
```python
form = UserRegister(request.POST)
```

Next, we create a user object with the received information but without saving it to the database by adding `commit=False`
```python
user = form.save(commit=False)
```

Then, we use the user's `set_password` method to `hash` the raw password and then set the hashed password as the users password.
```python
user.set_password(user.password)
```

finally, we save the user to the database and log him in
```python
user.save()
login(request, user)
```
___
##### Fun Fact

`Hashing` is similar to `encryption`. However, the difference is when you `encrypt` something, you can `decrypt` it to its original value. On the other hand, when you `hash` something, there is no way to get the original value back.

___

### Logout

`authentication/views.py`
```python
def logout_view(request):
	logout(request)
	return redirect('recipes-list')
```

There is nothing to explain here. If it still doesn't make any sense, look at the dancing potato until it makes sense. That's all the help I can give you.

![dancing potato](https://media1.tenor.com/images/61497871ab091f01703a3f1a624fb3c4/tenor.gif?itemid=11684043)

