# Python Django Web Framework - Full course for beginner (freeCodeCamp) Part 1

- YouTube video link: https://www.youtube.com/watch?v=F5mRW0jo-U4&t=1807s&ab_channel=freeCodeCamp.org

  

## Before Beginning

- create and activate your virtual environment 

  - `source virt_name/bin/activate`

  - `deactivate` 

    

## Build-in components

 - `python manage.py createsuperuser` 

   

## First App Components

- `python manage.py startapp app_name`

### A way  to store data

#### app_name/models.py

 - ```python
   class Product(models.Model):
   	title = models.TextField()
   	description = models.TextField()
   	price = models.TextField()
       summary = models.TextField(default='this is cool!')
   ```

#### project/settings.py

- add `'app_name',` under INSTALLED_APPS
  - will be shown after log into the admin page 

#### Important Note:

- `python manage.py makemigrations`
- `python manage.py migrate` 

#### app_name/admin.py

- ```python
  from .models import Product 
  #import Product class
  
  admin.site.register(Product)
  ```



### Create Product Objects in the Python Shell

- `python manage.py shell` # a python shell 

  - can do

    - ```python
      from app_name.models import Product
      Product.objects.all()
      Product.objects.create(title="New product 2", description="another one", price="1234", summary="sweet")
      # note all four entries are string due to TextField
      ```

    - now if run the server, we navigate to app_name from admin page, we can see a new Product object created with increasing number in the bracket: Product object(#)



### New Model Fields

- To start all over, can delete files inside migrations of app_name (can keep _init_.py) , as well as _pycache_.py folder, and db.sqlite3. 
  - (delete migration and database)
    - will need to `createsuperuser` again since delete database

#### Reference: djangoproject.com 

- Field Type 

- ```python
  class Product(models.Model):
  	title = models.CharField(maxlength=120) # maxlength is required for CharField
  	description = models.TextField(blank=True, null=True)
  	price = models.DecimalField(decimal_places=2, max_digits=100)
      summary = models.TextField()
  ```



### 