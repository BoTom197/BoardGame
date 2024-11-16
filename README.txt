!## This is a tutorial on how to implement a board game website management project using Django.

Step 1: Setting Up the Environment

1. Install Python and pip: Ensure that you have Python (version 3.6 or higher) and pip (the package manager for Python) installed.

2. Install Django : Open the terminal (cmd on Windows) and run the following command to install Django:
	pip install Django # bash file
3. Create a new project: Create a directory for your project and navigate into that directory, then run the command:
	django-admin startproject boardgame_registry # bash
	cd boardgame_registry # bash
4. Create a new application: Run the following command to create your main application:
	python manage.py startapp games # bash
5. Run server 
	python manage.py runserver
Open your browser and go to: http://127.0.0.1:8000. If you see the Django welcome page, you're set up successfully.

Step 2: Create the games Application
1. Create a new Django app named boardgame_web.
	python manage.py startapp boardgame_web #bash
2. Register the games app in settings.py.
	# boardgame_loan/settings.py
	# boardgame_loan/settings.py
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'games',
]
step 3: Design the Database (Models)
1. Define the models in games/models.py
	from django.db import models
	from django.contrib.auth.models import User

	class BoardGame(models.Model):
    	title = models.CharField(max_length=200)
   	 description = models.TextField()
    	created_at = models.DateTimeField(auto_now_add=True)
    	updated_at = models.DateTimeField(auto_now=True)

    	def __str__(self):
       	 return self.title
2. Create and apply migrations.
	python manage.py makemigrations #bash
	python manage.py migrate #bash

step 4: Admin Interface // Register the models in the admin interface for easy management.
	# games/admin.py
	from django.contrib import admin
	from .models import BoardGame, Loan

	admin.site.register(BoardGame)
	admin.site.register(Loan)
And then access the admin site at http://127.0.0.1:8000/admin. Log in with your admin account to manage the models.

step 5: Create Views and URLs
1. Create a view to display a list of board games.
	# games/views.py
	from django.shortcuts import render
	from .models import BoardGame

	def boardgame_list(request):
    	games = BoardGame.objects.all()
    	return render(request, 'games/boardgame_list.html', {'games': games})
2. Define URLs in games/urls.py
	from django.urls import path
	from .views import boardgame_list
	urlpatterns = [
    	path('', boardgame_list, name='boardgame_list'),
	]
3. Create the template to display the list of board games.
	<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Board Game List</title>
</head>
<body>
    <h1>Board Game List</h1>
    <ul>
        {% for game in games %}
            <li>{{ game.name }} - Owned by {{ game.owner.username }}</li>
        {% endfor %}
    </ul>
</body>
</html>
And then run the server again and visit: http://127.0.0.1:8000 to see the list of board games.

Finally, implement Borrow and Return Game Functionality
Add the logic to borrow and return games in views.py and update the corresponding templates. You will need to:

	Check the limit of 3 borrowed games per user.
	Notify the user when attempting to borrow more than the limit.
	Record the date and time of borrowing and returning.

Copyright NamNguyen
Project Django - Web boadgames 
11.2024