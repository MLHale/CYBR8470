# Secure Access Control in Django APIs

### Introduction

In this module, you will learn how to implement secure access control mechanisms in Django for various API endpoints. You will build upon the existing Dog API application to add authentication, permissions, and object-level access control. This will ensure that only authenticated users can access the APIs and that they can only access data they are permitted to.

### Goals

By the end of this tutorial, you will be able to:

- Implement authentication in Django using built-in and third-party libraries
- Apply access control and permissions to your Django APIs (SOAP, REST, and GraphQL)
- Implement object-level permissioning to restrict access to specific data
- Test your API security features manually using Postman

### Materials Required

For this lesson, you will need:

- PC - Windows, Mac, or Linux
- Internet connection
- Completion of the previous lab (Django API Examples - SOAP, REST, and GraphQL)

## Prerequisites

Before starting this lab, make sure you have completed the previous lab and have the Django project with the `Dog` model and the three APIs (SOAP, REST, and GraphQL) up and running.

## 1. Updating the Dog Model to Include an Owner

To implement object-level permissions, we need to associate each `Dog` with a specific user (owner). This way, we can restrict access to the `Dog` objects based on the logged-in user.

### 1.1. Modify the `Dog` Model

In `dogapp/models.py`, update the `Dog` model to include a `ForeignKey` to the `User` model:

```python
from django.db import models
from django.contrib.auth.models import User

class Dog(models.Model):
    owner = models.ForeignKey(User, on_delete=models.CASCADE)
    name = models.CharField(max_length=100)
    age = models.IntegerField()
    breed = models.CharField(max_length=100)

    def __str__(self):
        return self.name
```

### 1.2. Make Migrations and Migrate
After modifying the model, you need to create a new migration and apply it:

```bash
python manage.py makemigrations
python manage.py migrate
```

### 1.3. Update the Admin Interface
In `dogapp/admin.py`, update the `DogAdmin` class to include the `owner` field:

```python
from django.contrib import admin
from .models import Dog

class DogAdmin(admin.ModelAdmin):
    list_display = ('name', 'age', 'breed', 'owner')
    search_fields = ('name', 'breed')
    list_filter = ('age', 'breed', 'owner')
    list_display_links = ('name',)
    fields = ('owner', 'name', 'age', 'breed')

admin.site.register(Dog, DogAdmin)
```

### 1.4. Create Some Users and Dogs
- Create a few users using the Django admin interface at `http://localhost:8000/admin/`.
- Assign different dogs to different users.

## 2. Implementing Authentication
We will now implement authentication mechanisms for our APIs to ensure that only authenticated users can access them.

### 2.1. Setting Up Django Authentication
Django provides built-in authentication mechanisms. Ensure that the `django.contrib.auth` and `django.contrib.sessions` apps are included in your `INSTALLED_APPS` in `webservices/settings.py` (they are included by default).

### 2.2. Enabling Authentication in REST Framework
Since we are using Django REST Framework (DRF) for the REST API, we can use DRF's authentication mechanisms.

#### 2.2.1. Update settings.py
In `webservices/settings.py`, add REST framework settings:

```python
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': (
        'rest_framework.authentication.SessionAuthentication',
        # Alternatively, you can use TokenAuthentication
        # 'rest_framework.authentication.TokenAuthentication',
    ),
    'DEFAULT_PERMISSION_CLASSES': (
        'rest_framework.permissions.IsAuthenticated',
    ),
}
```

#### 2.2.2 Restart your server
```bash
python manage.py runserver
```

### 2.3. Implementing Login and Logout Views
To authenticate users, we need to provide login and logout views. We'll use Django's built-in authentication views.

#### 2.3.1. Update `urls.py`
In `webservices/urls.py`, add the following imports and URL patterns:

```python
from django.contrib import admin
from django.urls import path, include
from django.contrib.auth import views as auth_views

urlpatterns = [
    path('admin/', admin.site.urls),
    # ... existing URLs ...
    path('accounts/login/', auth_views.LoginView.as_view(), name='login'),
    path('accounts/logout/', auth_views.LogoutView.as_view(), name='logout'),
]
```

#### 2.3.2. Create Login Template
Create a directory named `templates` in your project root (where `manage.py` is located), and inside it, create a directory named `registration`.

Create a file named `login.html` inside `templates/registration/`:

```html
<!-- templates/registration/login.html -->
<!DOCTYPE html>
<html>
<head>
    <title>Login</title>
</head>
<body>
    <h2>Login</h2>
    {% if form.errors %}
        <p>Your username and password didn't match. Please try again.</p>
    {% endif %}
    <form method="post" action="{% url 'login' %}">
        {% csrf_token %}
        <label for="username">Username:</label>
        <input type="text" name="username" autofocus required><br>
        <label for="password">Password:</label>
        <input type="password" name="password" required><br>
        <input type="submit" value="Login">
    </form>
</body>
</html>
```
### 2.4. Test Authentication
Run the server, navigate to `http://localhost:8000/accounts/login/`, and try to login using your username and password to ensure the login page is working. Test to see what happens with an incorrect username/password or both. Is the error message a good one? What might be better?

## 3. Securing the REST API
Now, we will secure the REST API so that only authenticated users can access it, and users can only access their own `Dog` objects.

### 3.1. Update the REST API View
In `dogapp/views.py`, update the `rest_get_dog` view to enforce permissions:

```python
from rest_framework import status, permissions
from rest_framework.response import Response
from rest_framework.decorators import api_view, permission_classes
from .models import Dog
from .serializers import DogSerializer

@api_view(['GET'])
@permission_classes([permissions.IsAuthenticated])
def rest_get_dog(request, dog_id):
    try:
        dog = Dog.objects.get(pk=dog_id)
        if dog.owner != request.user:
            return Response({'error': 'You do not have permission to view this dog.'}, status=status.HTTP_403_FORBIDDEN)
        serializer = DogSerializer(dog)
        return Response(serializer.data)
    except Dog.DoesNotExist:
        return Response({'error': 'Dog not found.'}, status=status.HTTP_404_NOT_FOUND)
```
        
### 3.2. Test the REST API
#### 3.2.1. Using Session Authentication in Postman
Since we are using `SessionAuthentication`, we need to log in to obtain a session cookie.

- Open Postman.
- Create a new request to `http://localhost:8000/accounts/login/`.
- Set the method to `POST`.
- In the Body tab, select `x-www-form-urlencoded`.
- Add the following key-value pairs:
  - `username`: your username
  - `password`: your password
- Send the request.

If the login is successful, Postman will receive a session cookie.

#### 3.2.2. Use the Session Cookie for API Requests
- In Postman, the session cookie should be automatically saved in the Cookies section.
- Now, create a new GET request to `http://localhost:8000/rest/dog/1/`.
- Ensure that the session cookie is included in the request (Postman does this automatically).
- Send the request.

You should receive the data for the dog if you own it, or a permission error if you do not.


