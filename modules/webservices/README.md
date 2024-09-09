# Django API Examples - SOAP, REST, and GraphQL

This repository contains examples of how to implement SOAP, REST, and GraphQL APIs in Django to access a `Dog` model.

### Introduction
In this module, you will learn how to build several different types of webservices in Django. We will use the DogAPI as a reference to recreate our own service.

### Goals
By the end of this tutorial, you will be able to:
* Build a `Django` application
* Create a `REST endpoint` on the `application server`
* Create a `SOAP service`
* Create a `GraphQL backend`

### Materials Required
For this lesson, you will need:

* PC - Windows, Mac, or Linux
* Internet connection

## Installing Python and Adding it to PATH (Windows)
Before we begin, you will need the latest version of Python and you will need to install *Django*, a web application framework built in Python.

### 1. Download Python
- Go to the official Python website: [https://www.python.org/downloads/](https://www.python.org/downloads/)
- Click the **Download Python** button. This will automatically download the latest version for your Windows system.

### 2. Run the Installer
- Locate the downloaded Python installer (`python-x.x.x.exe`) in your `Downloads` folder and run it.
- On the first screen, **check the box that says "Add Python x.x to PATH"** at the bottom. This is crucial for automatically adding Python to your PATH environment variable.

### 3. Customize Installation (Optional)
- Click on **Customize installation** if you want to modify the default installation options.
- Ensure that the following options are selected:
  - `pip` (this should be checked by default).
  - `tcl/tk and IDLE` (if you plan to use the Python IDLE).
  - `Python test suite` (optional).
  - `py launcher` (this is useful for running Python from the command line).

### 4. Install Python
- Click **Install Now** or **Install** (if you selected to customize).
- The installer will run and install Python, along with the selected options.

### 5. Verify Installation
After installation, verify that Python is installed and available on your PATH:

#### 5.1 Open a Command Prompt
- Press `Win + R`, type `cmd`, and press Enter.

#### 5.2 Check Python Version
- Type `python --version` and press Enter. You should see something like:

#### 5.3 Check `pip` Version
- Type `pip --version` and press Enter. You should see the installed version of `pip`, which is the package manager for Python.

### 6. Manually Add Python to PATH (If Not Added Automatically)
If Python wasn’t added to your PATH environment variable during installation, you can add it manually:

#### 6.1 Locate the Python Installation Path
- The default location is usually `C:\Users\<YourUsername>\AppData\Local\Programs\Python\Python<version>\` (for the Python executable).
- The `Scripts` folder, where `pip` and other utilities are located, will be in `C:\Users\<YourUsername>\AppData\Local\Programs\Python\Python<version>\Scripts\`.

#### 6.2 Add Python to PATH
1. Open the **Start menu**, type `Environment Variables`, and select **Edit the system environment variables**.
2. In the **System Properties** window, click **Environment Variables**.
3. Under **User variables** or **System variables**, select the `Path` variable and click **Edit**.
4. Click **New** and paste the Python installation path (e.g., `C:\Users\<YourUsername>\AppData\Local\Programs\Python\Python<version>\`).
5. Click **New** again and add the path to the `Scripts` folder (e.g., `C:\Users\<YourUsername>\AppData\Local\Programs\Python\Python<version>\Scripts\`).
6. Click **OK** to close all windows.

#### 6.3 Reopen Command Prompt
- Close and reopen Command Prompt to apply the changes.
- Verify by typing `python --version` and `pip --version` again to ensure they are recognized.

## Installing Python on MacOS

### 1. Check if Python is Already Installed

macOS comes with a version of Python pre-installed, usually Python 2.x. You can check which version is installed by opening the **Terminal** and running:

```bash
python --version
python3 --version
```
### 2. Install Homebrew (If Not Already Installed)
Homebrew is a package manager for macOS that simplifies the installation of software. First, check if Homebrew is installed by typing the following command in Terminal:

```bash
brew --version
```
If Homebrew is not installed, install it by running:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
### 3. Install Python with Homebrew
Once Homebrew is installed, you can easily install Python by running:

```bash
brew install python
```

## Install Django and configure a dogapp project

### 1. Install Django
```bash
python -m pip install Django
```

### 2. Create Django Project and App:
```bash
django-admin startproject myproject
cd myproject
python manage.py startapp dogapp
```

### 2. Create a `Dog` Model
Let's create a new database object to hold our dog information. It will have a name, an age, and a breed.

In `dogapp/models.py`:

```python
from django.db import models

class Dog(models.Model):
    name = models.CharField(max_length=100)
    age = models.IntegerField()
    breed = models.CharField(max_length=100)

    def __str__(self):
        return self.name
```

When we create objects in django, we need to update our database schema

```bash
python manage.py makemigrations
python manage.py migrate
```

Now we are ready for the real content...

## SOAP Service Example

### 1. Install Required Libraries (Spyne)
We will use a remote procedure call python library called spyne to build our SOAP-based service

Lets start by installing the required library via pip:

```bash
pip install django spyne
```

### 2. Define SOAP Service

Create the SOAP service by using the `spyne` library. In `dogapp/views.py`:

```python
from spyne import Application, rpc, ServiceBase, Integer, Unicode
from spyne.protocol.soap import Soap11
from spyne.server.django import DjangoApplication
from dogapp.models import Dog

class DogService(ServiceBase):
    @rpc(Integer, _returns=Unicode)
    def get_dog(ctx, dog_id):
        try:
            dog = Dog.objects.get(pk=dog_id)
            return f"Dog: {dog.name}, Age: {dog.age}, Breed: {dog.breed}"
        except Dog.DoesNotExist:
            return "Dog not found."

soap_app = Application([DogService], 'dogapp.soap',
                       in_protocol=Soap11(validator='lxml'),
                       out_protocol=Soap11())

dog_service = DjangoApplication(soap_app)
```

### 3. URL Configuration

In `myproject/urls.py`, add a URL that points to the SOAP service:

```python
from django.urls import path
from dogapp.views import dog_service

urlpatterns = [
    path('soap/dogservice/', dog_service),
]
```

### 4. Start the server
Start your Django server in development mode:

```bash
python manage.py runserver
```

### 5. Access your SOAP service
You can access the service at `http://localhost:8000/soap/dogservice/?wsdl` to see the WSDL. To make SOAP requests, you can use a SOAP client to call the `get_dog` method by passing the `dog_id`.

### Example SOAP Request:

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:dog="dogapp.soap">
   <soapenv:Header/>
   <soapenv:Body>
      <dog:get_dog>
         <dog_id>1</dog_id>
      </dog:get_dog>
   </soapenv:Body>
</soapenv:Envelope>
```

### Postman (for manual testing)
Postman is a popular API testing tool that supports SOAP requests. Here's how you can use it:

- Open Postman and create a new request.
- Set the method to `POST` and the URL to your service endpoint (e.g., `http://localhost:8000/soap/dogservice/`).
- Go to the Body tab and select raw.
- Set the body type to XML.
- Enter the SOAP request XML in the body (see example below).
- Click Send.

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:dog="dogapp.soap">
   <soapenv:Header/>
   <soapenv:Body>
      <dog:get_dog>
         <dog_id>1</dog_id>
      </dog:get_dog>
   </soapenv:Body>
</soapenv:Envelope>
```

## REST API Example

### 1. Install Django REST Framework

```bash
pip install djangorestframework
```

### 2. Create Serializer for the `Dog` Model

In `dogapp/serializers.py`:

```python
from rest_framework import serializers
from .models import Dog

class DogSerializer(serializers.ModelSerializer):
    class Meta:
        model = Dog
        fields = ['id', 'name', 'age', 'breed']
```

### 3. Create API View

In `dogapp/views.py`:

```python
from rest_framework import status
from rest_framework.response import Response
from rest_framework.decorators import api_view
from .models import Dog
from .serializers import DogSerializer

@api_view(['GET'])
def get_dog(request, dog_id):
    try:
        dog = Dog.objects.get(pk=dog_id)
        serializer = DogSerializer(dog)
        return Response(serializer.data)
    except Dog.DoesNotExist:
        return Response({'error': 'Dog not found.'}, status=status.HTTP_404_NOT_FOUND)
```

### 4. URL Configuration

In `dogapp/urls.py`:

```python
from django.urls import path
from .views import get_dog

urlpatterns = [
    path('dog/<int:dog_id>/', get_dog, name='get_dog'),
]
```

---

## GraphQL API Example

### 1. Install `graphene-django`

```bash
pip install graphene-django
```

### 2. Create the GraphQL Schema

In `schema.py`:

```python
import graphene
from graphene_django.types import DjangoObjectType
from dogapp.models import Dog

class DogType(DjangoObjectType):
    class Meta:
        model = Dog

class Query(graphene.ObjectType):
    dog = graphene.Field(DogType, id=graphene.Int())

    def resolve_dog(self, info, id):
        try:
            return Dog.objects.get(pk=id)
        except Dog.DoesNotExist:
            return None

schema = graphene.Schema(query=Query)
```

### 3. URL Configuration

In `urls.py`:

```python
from django.urls import path
from graphene_django.views import GraphQLView
from django.views.decorators.csrf import csrf_exempt

urlpatterns = [
    path('graphql/', csrf_exempt(GraphQLView.as_view(graphiql=True))),
]
```

### Example GraphQL Query:

```graphql
query {
  dog(id: 1) {
    id
    name
    age
    breed
  }
}
```

## Summary

This repository demonstrates how to implement a simple `Dog` model in Django and expose it through SOAP, REST, and GraphQL APIs.
