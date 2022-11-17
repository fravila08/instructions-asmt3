
# Assessment-3 Steps

## Creating a Django Project, Application

Creating a Django Project
```
#create and activate virtual environment
python3 -m venv env
source env/bin/activate
#install Django utilizing pip
pip install django
#start a Django Project
django-admin startproject ecom_pro
cd ecom_pro
```
Now that we have a project to work from we can create an application
with a templates folder and a urls python file.
```
#Creates Django Application
python3 manage.py startapp ecom_app

#We want to create an index.html file inside of a pages folder nested in a templates folder
mkdir ecom_app/templates
mkdir ecom_app/templates/pages
touch ecom_app/templates/pages/index.html

#Create a url python file
touch ecom_app/urls.py
```
Next we will create a static folder which will hold our static files for our application
```
mkdir static
mkdir static/js
mkdir static/css
touch static/js/main.js
touch static/css/main.css
```

We don't want our secret keys to be exposed so we will create our .env file to hide any vital information
```
touch .env
```
Great, now that we have created all of our necessary folders and files we can move into ecom_pro/setting.py
to install our ecom_app and stablish our static file directory.
```
#first import os
import os

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'ecom_app' #<-- Here we are installing our ecom_app
]


STATICFILES_DIRS=[
    BASE_DIR, os.path.join('static') #<-- this will ensure that the path to our static folder is correct
]
```
Our Djago Secret Key is inside of our ecom_pro/setting.py. We want to ensure that it stays secret so we will
copy and paste the key into our .env and set it to a variable.
(inside .env)
```
django= 1gui3423juh41iu2r123u4123uy13uig3
```
Now we need to load it into our ecom_pro/settings.py using dotenv. We will install python-dotenv and then change 
our settings.py.
``` 
#IN TERMINAL
pip install python-dotenv

#IN ecom_pro/settings.py
#import load_dotenv and use os to call your django key from your .env file.
from dotenv import load_dotenv
load_dotenv()

SECRET_KEY = os.environ['django']
```
Awesome, now that our settings.py file is all set up we can move into connecting our app and project urls.
```
#IN ecom_pro/urls.py
from django.urls import path, include #<-- ensure to import include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('ecom_app.urls')) #<-- tells Django to look into ecom_app/urls.py for url applications
]

#IN ecom_app/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('', views.home), #<--tells the application to render the view home
]
```
Now to actually set up our view we will move to ecom_app/views and define the home view.
```
def home(request):
    return render(request, 'pages/index.html') 
    #^^^ we created index.html earliear and now we are telling this view to render it for the viewr. 
```
Although we created index.html we never actually did anything to it. So let's create that now.
in ecom_app/templates/pages/index.html
```
<!DOCTYPE>
<html>
    <head>
        <title>Assessment</title>
    </head>
    <body>
        <h1>Welcome</h1>
    </body>
</html>
```
Now we are finally ready to migrate our migrations and run the server!
in terminal
```
python3 manage.py migrate
python3 manage.py runserver
```
Now we can go to our local browser and expect to see this.

<img width="1512" alt="Screenshot 2022-11-16 at 10 17 13 PM" src="https://user-images.githubusercontent.com/105952966/202371162-ccb7f036-a7e7-421f-8259-9e21d934a775.png">

## Linking static files to our project
Although we do have our initial page rendering we don't have a connection to our static files.
Let's link our static files to index.html and get some proof that they are working.
```
#IN ecom_app/templates/pages/index.html 
{% load static %} <-- we want to add the load static tag
<html>
    <head>
        <title>Assessment</title>
        <link rel="stylesheet" href="{% static 'css/main.css' %}" />
        <script src="{% static 'js/main.js' %}"></script>
        #^^ these two lines use the static tag to map to our static files
    </head>

#IN static/js/main.js
console.log("I am connected don't worry")

#IN static/css/main.css
h1{
    color: red;
}
```
Now when we go to our locolhost and refresh the page we will see the following:

<img width="1512" alt="Screenshot 2022-11-16 at 10 29 55 PM" src="https://user-images.githubusercontent.com/105952966/202373053-c8212a9f-a18b-4045-98dd-52ab97cbcfaf.png">

## Using bootstrap and the extends function
To use bootstrap with our project we are going to add a link to the header pulled
staight from https://getbootstrap.com/ to our header.
```
#IN ecom_app/templates/page/index.html
<head>
    <title>Assessment</title>
    <!-- vvv this link below adds bootstrap functionality to our application -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-Zenh87qX5JnK2Jl0vWa8Ck2rdkQ2Bzep5IDxbcnCeuOxjzrPF/et3URy9Bv1WTRi" crossorigin="anonymous">
    <link rel="stylesheet" href="{% static 'css/main.css' %}" />
    <script src="{% static 'js/main.js' %}"></script>
</head>
```
Now we can grab a bootstrap component and place it in our index.html. In this case we will add a navbar to
our project.
```
<body>
        <header>
            <h1>Welcome</h1>
            <nav class="navbar navbar-expand-lg bg-light">
                <div class="container-fluid">
                  <a class="navbar-brand" href="#">Navbar</a>
                  <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
                    <span class="navbar-toggler-icon"></span>
                  </button>
                  <div class="collapse navbar-collapse" id="navbarSupportedContent">
                    <ul class="navbar-nav me-auto mb-2 mb-lg-0">
                      <li class="nav-item">
                        <a class="nav-link active" aria-current="page" href="#">Home</a>
                      </li>
                      <li class="nav-item">
                        <a class="nav-link" href="#">Link</a>
                      </li>
                      <li class="nav-item dropdown">
                        <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                          Dropdown
                        </a>
                        <ul class="dropdown-menu">
                          <li><a class="dropdown-item" href="#">Action</a></li>
                          <li><a class="dropdown-item" href="#">Another action</a></li>
                          <li><hr class="dropdown-divider"></li>
                          <li><a class="dropdown-item" href="#">Something else here</a></li>
                        </ul>
                      </li>
                      <li class="nav-item">
                        <a class="nav-link disabled">Disabled</a>
                      </li>
                    </ul>
                    <form class="d-flex" role="search">
                      <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search">
                      <button class="btn btn-outline-success" type="submit">Search</button>
                    </form>
                  </div>
                </div>
              </nav>
            </header>
    </body>
```
You should see the following on your localhost now:

<img width="1512" alt="Screenshot 2022-11-16 at 10 46 00 PM" src="https://user-images.githubusercontent.com/105952966/202375824-50284753-6841-46d1-afea-51d074ab7055.png">

It seems like index.html is holding everything that we would want to show on every page. 
We are going to make it our layout and extend it to all other templactes.
first in index.html we are going to create some blocks to input content into.
```
            </header>
            <main>
                {% block main %}{% endblock %}
            </main>
            <footer>
                {% block footer %}{% endblock %}
            </footer>
    </body>
</html>
``` 
Now lets rename this file into layout.html and create home.html
```
#IN TERMINAL
#this will rename index.html into layout.html
mv ecom_app/templates/pages/index.html ecom_app/templates/pages/layout.html

#create home.html
touch ecom_app/templates/page/home.html
```

Now let's extend layout into home.html and provide a main block to display.
```
#IN ecom_app/templates/pages/home.html
{% extends 'pages/layout.html' %} <-- this will extend layout.html
{% load static %}
{% block main %} <-- this will match the block main tags in layout and fill in the content.
    <h4> This is my home page being displayed</h4>
{% endblock %}
```
Lastly we will change our view to render home.html and launch the server.
```
#IN ecom_app/views.py
def home(request):
    return render(request, 'pages/home.html') #<--changed from index to home.
```
Now we can see the following on local host:

<img width="1512" alt="Screenshot 2022-11-16 at 11 00 02 PM" src="https://user-images.githubusercontent.com/105952966/202378231-baee28dd-5906-45f8-879e-5d8003fd8a65.png">



## Working with the Noun API

The noun api is @ https://thenounproject.com
and in the documentation they provide an exaple for their endpoint.

#### Get item

```http
  GET "http://api.thenounproject.com/icon/{item}"
```

| Parameter | Type     | Description                       |
| :-------- | :------- | :-------------------------------- |
| `id`      | `string` | **Required**. Id of item to fetch |

#### Here we have an example of how it works with Python
```
import requests
from requests_oauthlib import OAuth1

auth = OAuth1("your-api-key", "your-api-secret")
endpoint = "http://api.thenounproject.com/icon/1"

response = requests.get(endpoint, auth=auth)
```
Once you have your api-key and your secret api-key we can hide them in our env.
```
key="your-api-key"
secret="your-api-secret"
```
Lets set up our search bar to work as a form and trigger our api call.
```
IN layout.html
#vv here we will give form an action attribute to a "/product/" url and a POST method
<form class="d-flex" role="search" action="/product/" method="POST">
    {% csrf_token %} <-- csrf_token will allow it to send a bank end call without having a csrf cookie
    #vv ensure you give your input field a name attribute
    <input name="item" class="form-control me-2" type="search" placeholder="Search" aria-label="Search">
    <button class="btn btn-outline-success" type="submit">Search</button>
</form>
```
Now we can go to our ecom_app/urls.py and create a path for this post method.
```
urlpatterns = [
    path('', views.home), 
    path('product/', views.product),#<--this will receive the post method and call the product view
]
```
Now to create the view:
```
#first we need to install requests and requests_oauthlib
#IN TERMINAL
pip install requests
pip install requests_oauthlib

#IN views.py
import os
import requests
from requests_oauthlib import OAuth1

#now create the view
def product(request):
    item=request.POST.get("item") #<--grabs user input from form
    #lets get our API keys from our .env
    key=os.environ["key"]
    secret= os.environ["secret"]
    #lets stablish our auth variable
    auth = OAuth1(key, secret)
    #now stablish our endpoint
    endpoint= f"http://api.thenounproject.com/icon/{item}"
    #finally we can make our api call to the Noun project
    API_response = requests.get(endpoint, auth=auth)
    #although our response was successful it's unreadable so we will need to create it into JSON
    API_response=API_response.json()
    #now we can print the url and map into our img
    img=API_response["icon"]["preview_url"]
    #now we can pass this url through a dictionary to a template
    data={
        "img":img
    }
    return render(request, "pages/product.html", data)
```
We need to create product.html extend layout and display our img now.
```
#IN TERMINAL
touch ecom_app/templates/pages/product.html

#IN product.html
{% extends 'pages/layout.html' %}
{% load static %}
{% block main %}
    <img src="{{img}}"/>  #<--This will grab the img_url and place it in the src
{% endblock %}
```

Finally when you run your server you and place "car" into the form you will see the following:

<img width="1512" alt="Screenshot 2022-11-16 at 11 37 10 PM" src="https://user-images.githubusercontent.com/105952966/202384976-59b8d197-bfa9-4ef2-935f-da98bfe43f4f.png">

# Handling csv's

To effectively handle csv's we will create a csv_handlies.py file where we will stablich a class and create some instance methods to read, write, remove from a csv.
```
import csv, os

class Csv:
    def __init__(self, filename): #<-- our init method will take in only the name of the file
        with open(os.path.join(filename), "r") as f:
            reader = csv.DictReader(f, skipinitialspace=True, delimiter=',') #<--this will read the file and get the create dictionaries for each item
            self.column_names = reader.fieldnames
        
        self.filename = filename
        self.all_data =self.update_data_from_file()
        
    def update_data_from_file(self):
         """ reads the csv file and update the all_data """
         data = []
         with open(self.filename, "r") as f:
             reader = csv.DictReader(f, skipinitialspace=True, delimiter=',')
             for row in reader:
                 data.append(row)
         self.all_data = data
         return self.all_data #<-- returns an array of dictionaries
```
Now lets create a csv file and some instance to see the functionality.
```
#IN TERMINAL
mkdir ecom_app/data
touch ecom_app/data/inventory.csv

#IN ecom_app/data/inventory.csv
id,title,img,price,category
1,Fire Red,https://www.digitalgamemuseum.org/collection/files/fullsize/a4415e9903844aca35f253a3e7307201.jpg,30,games
2,Leaf Green,https://images.emulatorgames.net/gameboy-advance/pokemon-leaf-green-version-v1-1.webp,35,games


#IN views.py
import the class from csv_handler.py
from .csv_handler import Csv

inventory=Csv('ecom_app/data/inventory.csv')
print(inventory.all_data)

#run your server and in your terminal you should get this print statement:
[{'id': '1', 'title': 'Fire Red', 'img': 'https://www.digitalgamemuseum.org/collection/files/fullsize/a4415e9903844aca35f253a3e7307201.jpg', 'price': '30', 'category': 'games'}, {'id': '2', 'title': 'Leaf Green', 'img': 'https://images.emulatorgames.net/gameboy-advance/pokemon-leaf-green-version-v1-1.webp', 'price': '35', 'category': 'games'}]
```
