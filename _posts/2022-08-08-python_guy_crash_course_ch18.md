---
layout: posts
title: "Python Crash Course Youtube Video 18"
date: 2022-08-08 16:04:54 -0700
categories: django crash_course
---

# Learning Python Youtube Channel - Chapter 18 of Python Crash Course - Getting Started with Django

* Wrote a spec for Learning Log
* Create directory called learning log and change to it
* Create venv called ll_env and activate it

* Install Django

* Start new project
    
    ```
    django-admin startproject learning_log .
    ```

    Creates another learning_log directory and manage.py file

Create database
    python manage.py migrate
        creates db.sqlite3 in same directory as manage.py

Run server to test everything
    python manage.py runserver
        Runs on localhost:8000
    python manage.py runserver 8001 # lets you change port

Create an app called learning_logs
    python manage.py startapp learning_logs
        Structure at this point:
            learning_log # project directory
                manage.py # main file
                db.sqlite3 # database
                learning_log # project subdirectory
                learning_logs # app subdirectory
                ll_env # virtual environment directory

In models.py inside app subdirectory
    class Topic(models.Model):  # Create a class for our Topic - each user can have multiple topics - chess, rock climbing, etc. Inherit models.Model from Django
        text = models.CharField(max_length=200)  # The description of our topic - 200 characters max_length
        date_added = models.DateTimeField(auto_now_add=True) # Creation date of our topic. Automatically set it using a django function

        def __str__(self):
            return self.text  # Returns string representation of the model.

Have to tell the Django project about the app we're creating. In project subdirectory edit settings.py and add to INSTALLED_APPS list:
    # My apps -- put this ahead of the default apps that were put there by Django
    'learning_logs',

    # Default django apps

Activate the changes we made to the database:
    python manage.py makemigrations learning_logs  # Creates the migrations
    python manage.py migrate # Does the actual migrations.

    This is a normal 3 step process whenever we modify the database:
        modify models.py
        makemigrations
        migrate

Create a superuser:
    python manage.py createsuperuser
        he used ll_admin as the user name, blank email and then the password.

Modify learning_logs admin.py file so we can see our database on the admin page of the website.
    from .models import Topic
    admin.site.register(Topic)

Add some topics from the admin page -
    Go to localhost:8000/admin in web browser and add Chess and Rock Climbing as topics.

Add Entry class to our database - it will have a topic field which is tied to one of our topics. There will be many entries for each topic.
    a text field where the use types what's been done re: learning in this topic at this time, and a date_added field similar to the topic date_added field

    class Entry(models.Model): # Inherit from Django's model class
        topic = models.ForeignKey(Topic, on_delete=models.CASCADE) # this ties the entry to one of the topics
        text = models.TextField() # indeterminate length text field
        date_added = models.DateTimeField(auto_now_add=True) # and the date created.

        class Meta:  # return 1st 50 characters of the text field
            verbose_name_plural = 'entries' # simply tells Django how to spell entries - otherwise Django would spell the plural entrys.
            def __str__(self):
                return f"{self.text[:50]}..."

Have to repeat the makemigrations step and the migrate step, now that we've updated the model.

Change admin.py to add the Entry class
    from .models import Topic, Entry
    admin.site.register(Topic)
    admin.site.register(Entry)

Then create two entries under the topic Chess and one under Rock Climbing

Can open Django shell to explore the database:
python3 manage.py shell

from learning_logs.models import Topic  # access the Topic class
Topic.objects.all()  # will list all the topics we've created.

Or go through the topics one by one
topics = Topic.objects.all()
for topic in topics:
    print(topic.id, topic)

Examine items individually
t = Topic.objects.get(id=1)
t.text will print out Chess
t.date_added will print out the time and date
