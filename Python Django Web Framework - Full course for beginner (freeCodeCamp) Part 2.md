# Python Django Web Framework - Full course for beginner (freeCodeCamp) Part 2

- YouTube video link: https://www.youtube.com/watch?v=F5mRW0jo-U4&t=1807s&ab_channel=freeCodeCamp.org



## Change a Model

### Change a model <u>*without deleting database*</u> or making migration  for the  newly added matter

- ```python
  class Product(models.Model):
  	title = models.CharField(maxlength=120) # maxlength is required for CharField
  	description = models.TextField(blank=True, null=True)
  	price = models.DecimalField(decimal_places=2, max_digits=100)
      summary = models.TextField()
      
      # add new field below
      featured = models.BooleanField()
  ```

  - `featured` was added, and if run `python manage.py makemigrations`, will get error saying non-nullable field. 

    - check the ***app_name/migrations*** folder, the latest file for example 0001_initial.py, under fields entry of operations, we cannot find the newly added field `featured`. 

    - We can add `null=True` or `default=True` in the bracket to hard code them, this is one way to solve the problem

    - Or get the error, and provide one-off default which means input `1` and enter `True` (this means everything in the db, go through all of them, and save those as having `featured` field with the value of True). 

      - then run the `makemigrations` again will create a new migration file for us, however it still does not say the `featured` field. 
      - run `migrate`  we can see the changes on the web

    - `blank=` deals with how the field is rendered, `null=` deals with database.

      - `blank=False` : render as required, not relate to database

      - `null=False/True`: database can be null or empty

        

## Default Homepage to Custom Homepage

- Create a class or a function based view.

#### Example Step:

##### create a new app called Pages

##### add Pages to INSTALLED_APPS

##### Pages/views.py

- ```python
  from django.http import HttpResponse
  def home_view(*args, **kwargs): 
  	return HttpResponse("<h1>hello world</h1>") # string of HTML code
  ```

  - look up for `*args, **kwargs` in Python

- Until here, if run the server, will not find the page, this is where url comes in

  - check app_name/setting.py, `ROOT_URLCONF` 

##### app_name/urls.py

- ```python
  from Pages import views
  urlpatterns = [
  	path('', views.home_view, name='home'),
  	path('admin/', admin.site.urls),
  ]
  ```

- A better way of coding: (can help in case of multiple views are imported from different apps)

  - ```python
    from Pages.views import home_view
    urlpatterns = [
    	path('', home_view, name='home'),
    	path('admin/', admin.site.urls),
    ]
    ```

    

## URL Routing and Requests

- Views are created to handle URLs 

  - ```python
    from Pages.views import home_view, contact_view
    urlpatterns = [
    	path('', home_view, name='home'),
        path('contact/', contact_view),
    	path('admin/', admin.site.urls),
    ]
    ```

    - *practice:* create contact_view function in Pages/views.py 

###### Some tips

- ```python
  def home_view(request, *args, **kwargs): 
      print(args, kwargs)
      print(request.user)  #can see who is requesting at the terminal
  	return HttpResponse("<h1>hello world</h1>") 
  ```

  

## Django Templates 

#### Pages/views.py (use render)

- ```python
  def home_view(request, *args, **kwargs):
  	return render(request, "home.html", {}) #return HTML file
  ```

- need to create a location for it, otherwise cannot find the html file

#### templates/home.html

- Inside of **src** folder in the root of the app_name project, create a folder called `templates` 

  - templates/home.html

    - ```html
      <h1>Hello world</h1>
      <p>This is a template</p>
      ```

#### app_name/settings.py

- One way: hard code it (only work on own computer)

  - ```python
    TEMPLATES = [
    	{	
         	'BACKEND': XXXXXX,
    		'DIRS':['path/to/templates'], # USE 'pwd' at the terminal in the root of project
         	'APP_DIRS': XXXXX,
            XXXXX
    	}
    ]
    ```

- Another way: 
  - copy the line `BASE_DIR = os.path.dirname(......)` with some addition 
  - `'DIRS': [os.path.join(BASE_DIR, "templates")],`



## Django Templating Engine Basics

- ```html
  <h1> Hello </h1>
  {{ request.user }}
  <p>This is a template</p>
  ```

#### Template Inheritance 

##### Create a root page called base.html and call this html from other page html

- ```html
  <!doctype html>
  <html>
  <head>
  	<title>Hey</title>
  </head>
  <body>
      <h1>
          This is navbar
      </h1>
      {% block content %}
      {% endblock %}
  </body>
  </html>
  ```

- ```html
  {% extends 'base.html' %}
  
  {% block content %}
  	<h1> Hello </h1>
  	<p>This is a template</p>
  {% endblock %}
  ```

  

```

```

## Include Template Tag

#### Example: Need a navbar on each page

##### navbar.html

- ```html
  <nav>
  	<ul>
          <li>Brand</li>
          <li>Contact</li>
          <li>About</li>
      </ul>
  </nav>
  ```

##### base.html (include external template into template)

- ```html
  <!doctype html>
  <html>
  <head>
  	<title>Hey</title>
  </head>
  <body>
      {% include 'navbar.html' %}
      {% block content %}
      {% endblock %}
  </body>
  </html>
  ```

  

## Rendering Context in a Template

#### Pages/views.py

- ```python
  def home_view(request, *args, **kwargs):
  	return render(request, "home.html", {}) #return HTML file
  
  def about_view(request, *args, **kwargs):
  	my_context = {
          "my_text": "This is about us",
          "my_number": 123,
          "my_list": [1234, 234, 5453]
      }
      return render(request, "about.html", my_context)
  ```

#### templates/about.html

- ```html
  {% extends "base.html" %}
  
  {% block content %}
  <h1>About</h1>
  <p>This is a template</p>
  <p>
      {{ my_text }}, {{ my_number }}
  </p>
  {% endblock content}
  ```

  - context variable inside the {{ }} to access data passed from the python code

    

## For Loop in a Template

#### Print elements in the list one by one (not a whole list)

##### templates/about.html

```html
<ul>
{% for my_sub_item in my_list}
    <li>{{ my_sub_item }}</li>
{% endfor %}    
</ul>
```

- `{{ forloop.counter }}` can see current iteration number



## Using Conditions in a Template

#### Be careful when naming the variable 

#### Conditions in html

- ```html
  {% if my_sub_item == 312 %}
  	<li>{{ forloop.counter}} - {{ my_sub_item|add:22 }}</li>
  {% elif my_sub_item == "abc" %}
  	<li>This is abc</li>
  {% else %}
  	<li>{{ my_sub_item }}</li>
  {% endif %}
  ```

  

## Template Tags and Filters

#### Template tags

- `extends, block, for, if` are template tags we used so far, check out *built-in tags* on official documents

#### Filters

- `|add:22` is a filter, it take whatever my_sub_item value is and add 22 to this value. 
  - `|` is pipe, what after pipe is the filter 
  - *built-in filter* reference 
  - example: 
    - `<h1>{{ title|capfirst|upper }}</h1>`
    - `{{ my_html|safe }}`  # python pass a html string, render will print it as text unless using `|safe` filter



