title: Django
---

Django ORM
----------

pet_set is a lazy object that only makes a call to the dtabase when you begin
to iterate over it. When the queryset is evaluated it will caches the results
so latter calls to pet_set will not also call the database.

.. sourcecode:: python

    pet_set = Pet.objects.filter(species="Dog")
    # The query is executed and cached.
    for pet in pet_set:
        print(pet.first_name)
    # The cache is used for subsequent iteration.
    for pet in pet_set:
        print(pet.last_name)

The "if" statement will evaluate the queryset and cache the results so that
only one call to the database is made. 

You can avoid evaluating and potentially
caching a largy queryset by calling queryset.exists() which just checks if at
least one row in the database matches.

.. sourcecode:: python

    restaurant_set = Restaurant.objects.filter(cuisine="Indian")
    if restaurant_set.exists():
        print("Found some resturaunts!")

    # The `if` statement evaluates the queryset.
    if restaurant_set:
        # The cache is used for subsequent iteration.
        for restaurant in restaurant_set:
            print(restaurant.name)

you can evaluate a queryset without caching the resutls by calling iterator()

.. sourcecode:: python

    pet_set = Pet.objects.all()
    for pet in pet_set.iterator():
        print(pet.name)

iterate over large dataset example

.. sourcecode:: python

    pet_set = Pet.objects.all()
    pet_iterator = pet_set.iterator()
    #Look at first item in the iterator
    try:
        pet_one = next(pet_iterator)
    except:
        #No rows found, so no pets in the set
        pass
    else:
    from itertools import chain
        for pet in chain([pet_one], pet_set):
            print(pet.name)

via http://blog.etianen.com/blog/2013/06/08/django-querysets/


Model Forms with m2m Data
-------------------------

If a m2m field exists in the form, but is not rendered in the template then all m2m data for a given instance will be lost.
For example if you fill out a *DogForm* then add toys via the *DogToyForm* then go back and edit *DogForm* then all his toys will be lost!

.. sourcecode:: python

    #in models.py
    class DogToy(models.Model):
        name = models.CharField(max_length=60, blank=True)

    class Dog(models.Model):
        color = models.CharField(max_length=60, blank=True)
        toys = models.ManyToManyField('DogToy', blank=True)

    #in forms.py
    class DogForm(forms.ModelForm):
        class Meta:
            model = Dog

    class DogToyForm(forms.ModelForm):
        class Meta:
            model = DogToys


    #in template
    <form action="." method="post">
        {% csrf_token %}
        {{ form.color }}
        <input type="submit" />
    </form>

So if you wish to allow a dogs information to be edited in a form that does not include photos it is import that you exclude forms that
will not be rendered in the template. In the example above the *DogToyForm* should be changed to the following

.. sourcecode:: python

    class DogToyForm(forms.ModelForm):
        class Meta:
            model = DogToys
            exclude = ('toys')

via http://blog.etianen.com/blog/2013/06/08/django-querysets/

Coding Conventions
------------------

Ordering of imports
+++++++++++++++++++

.. sourcecode:: python

    # Stdlib imports
    # Core Django imports
    # Third-party app imports
    # Imports from your apps

Imports of 'test' package
+++++++++++++++++++++++++

.. sourcecode:: python

    #test/views.py
    from .models import Test
    from .forms import TestForm


Naming Conventions
++++++++++++++++++

app names should be lowercase single words like "jobs", "messages", "cars" 
(they should be plural versions of apaps main model)


Settings
++++++++

It is common to see a production and local settings file however I think that 
it can be simplified by making your local development as close to production
as possible. So everything required to run the project exists in base.py and
very little if any logic is contains in local.py. 

base.py
    common settings for local and production environment

test.py
    settings for running tests

local.py
    local packages like debug toolbar

.. sourcecode:: python

    #settings/local.py
    from .base import *
    DEBUG = True # do I need this, couldn't I just set TEMPLATE_DEBUG
    TEMPLATE_DEBUG = DEBUG

Sensitive information should be stored in environment variables, they can be
loaded into via your virtual environments activate script.

.. sourcecode:: shell

    # standard generated code here
    export SOME_SECRET_KEY=password-1-2-3

