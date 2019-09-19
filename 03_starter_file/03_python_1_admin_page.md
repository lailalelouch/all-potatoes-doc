The admin page is the page only admins can access.

![obvious](https://media.makeameme.org/created/captain-obvious-approves-5bd0ce.jpg)

You can access the admin page by going to this url `127.0.0.1:8000/admin` after running the server. When you login, you should be able to access all the objects you have in your database.

To login, you need a `superuser` account. To create one, stop the server(ctrl+c) and run the following command

```shell
(env)$ python manage.py createsuperuser
Username (leave blank to use <USERNAME>'): <ENTER_YOUR_USERNAME>
Email address: <ENTER_YOUR_EMAIL_OR_LEAVE_BLANK>
Password: <ENTER_YOUR_PASSWORD>
```

When you sign in to the admin page, you should be able to see the models that we've created (Recipe, Ingredint,...). These models appeared there becase we registered them.

How did we register them? open `recipes/admin.py` and you should know the answer.
