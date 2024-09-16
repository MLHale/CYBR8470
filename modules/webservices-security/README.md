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
    owner = models.ForeignKey(User, on_delete=models.CASCADE, default=None)
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

### 3.3. Using Token Authentication
Alternatively, you can use Token Authentication, which is useful for APIs because not all invocations happen within a same-domain session. Some might come from mobile apps or other applications using the API.

#### 3.3.1. Install `TokenAuthentication`
```bash
pip install djangorestframework
pip install djangorestframework-simplejwt
```

#### 3.3.2. Update `settings.py`
In `webservices/settings.py`, update the REST framework settings:

```python
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': (
        # 'rest_framework.authentication.SessionAuthentication',
        'rest_framework_simplejwt.authentication.JWTAuthentication',
    ),
    'DEFAULT_PERMISSION_CLASSES': (
        'rest_framework.permissions.IsAuthenticated',
    ),
}
```

#### 3.3.3. Add `rest_framework_simplejwt` to `INSTALLED_APPS`
```python
INSTALLED_APPS = [
    # ... other apps ...
    'rest_framework',
    'rest_framework_simplejwt.token_blacklist',
]
```

#### 3.3.4. Run Migrations
```bash
python manage.py migrate
```
#### 3.3.5. Update `urls.py` to Include Token Paths
Add the following imports and URL patterns to `webservices/urls.py`:

```python
from rest_framework_simplejwt.views import (
    TokenObtainPairView,
    TokenRefreshView,
)

urlpatterns = [
    # ... existing URLs ...
    path('api/token/', TokenObtainPairView.as_view(), name='token_obtain_pair'),
    path('api/token/refresh/', TokenRefreshView.as_view(), name='token_refresh'),
]
```

#### 3.3.6. Test with Postman
- Obtain a JWT token by making a POST request to `http://localhost:8000/api/token/` with your username and password in the body.
- Use the obtained token to authenticate your requests to the REST API by adding an `Authorization` header with the value `Bearer your_token_here`.

## 4. Securing the GraphQL API
Now, we'll secure the GraphQL API using authentication and permissions.

### 4.1. Update the GraphQL Schema
In `dogapp/schema.py`, import the required modules and enforce authentication:

```python
import graphene
from graphene_django.types import DjangoObjectType
from dogapp.models import Dog
from graphql import GraphQLError
from django.contrib.auth import get_user_model

class DogType(DjangoObjectType):
    class Meta:
        model = Dog

class Query(graphene.ObjectType):
    dog = graphene.Field(DogType, id=graphene.Int())

    def resolve_dog(self, info, id):
        user = info.context.user
        if user.is_anonymous:
            raise GraphQLError('Not authenticated')
        try:
            dog = Dog.objects.get(pk=id)
            if dog.owner != user:
                raise GraphQLError('You do not have permission to view this dog.')
            return dog
        except Dog.DoesNotExist:
            return None
```

Again - thinks about the kinds of error's you give. Is this object-level error sufficient? What might it tell an attacker about the existence of the dog they are searching for? Think like an adversary and adjust your error messages to reveal as little as possible (preferrably nothing).

### 4.2. Update `settings.py`
In `webservices/settings.py`, add the following setting to enable authentication in GraphQL:

```python
GRAPHENE = {
    'SCHEMA': 'dogapp.schema.schema',
}
```

### 4.3. Test the GraphQL API
#### 4.3.1. Obtain a Session (Using Cookies)
- Log in via the login page at `http://localhost:8000/accounts/login/`.
- Once logged in, you can access the GraphiQL interface at `http://localhost:8000/graphql/`.

#### 4.3.2. Use the Session Cookie in Postman
- In Postman, send a POST request to `http://localhost:8000/graphql/`.
- In the Headers tab, make sure to include the session cookie.
- In the Body tab, select `GraphQL`, and enter the following query:

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

- Send the request.

You should receive the data for the dog if you own it, or an error message if you do not.

### 4.4. Using JWT Authentication (Optional)
You can also implement JWT authentication in GraphQL using the `django-graphql-jwt` library.

#### 4.4.1. Install `django-graphql-jwt`
```bash
pip install django-graphql-jwt
```

#### 4.4.2. Update `settings.py`
Add the following to your `AUTHENTICATION_BACKENDS`:

```python
AUTHENTICATION_BACKENDS = [
    'graphql_jwt.backends.JSONWebTokenBackend',
    'django.contrib.auth.backends.ModelBackend',
]
```
Update your `GRAPHENE` settings:

```python
GRAPHENE = {
    'SCHEMA': 'dogapp.schema.schema',
    'MIDDLEWARE': [
        'graphql_jwt.middleware.JSONWebTokenMiddleware',
    ],
}
```

#### 4.4.3. Update the Schema
In `dogapp/schema.p`, add the authentication mutations:

```python
import graphene
import graphql_jwt

class Mutation(graphene.ObjectType):
    token_auth = graphql_jwt.ObtainJSONWebToken.Field()
    verify_token = graphql_jwt.Verify.Field()
    refresh_token = graphql_jwt.Refresh.Field()

schema = graphene.Schema(query=Query, mutation=Mutation)
```

In GraphQL, a mutation is a type of operation that allows clients to create, update, or delete data on the server. Mutations enable clients to send data to the server and define precisely how the data should change. Each mutation can also specify what data should be returned after the operation, which can be particularly useful for updating the client-side state immediately after the mutation.

#### 4.4.4. Test with Postman
Obtain a JWT token by making a mutation request:

```graphql
mutation {
  tokenAuth(username: "your_username", password: "your_password") {
    token
  }
}
```

Use the obtained token to authenticate your GraphQL requests by adding an `Authorization` header with the value `JWT your_token_here`.

## 5. Securing the SOAP API
Securing SOAP APIs in Django using `spyne` requires custom authentication mechanisms because `spyne` doesn't directly integrate with Django's authentication system.

### 5.1. Implementing Basic Authentication for SOAP
We will implement HTTP Basic Authentication for the SOAP service.

#### 5.1.1. Update the SOAP Service
In `dogapp/views.py`, update the `DogService` class to check for authentication:

```python
from spyne import Application, rpc, ServiceBase, Integer, Unicode
from spyne.protocol.soap import Soap11
from spyne.server.django import DjangoApplication
from dogapp.models import Dog
from django.views.decorators.csrf import csrf_exempt
from django.contrib.auth import authenticate

class DogService(ServiceBase):
    @rpc(Integer, _returns=Unicode)
    def get_dog(ctx, dog_id):
        request = ctx.transport.req
        user = None

        # Get the Basic Auth credentials
        if 'HTTP_AUTHORIZATION' in request.META:
            auth = request.META['HTTP_AUTHORIZATION'].split()
            if len(auth) == 2 and auth[0].lower() == 'basic':
                import base64
                username, password = base64.b64decode(auth[1]).decode('utf-8').split(':')
                user = authenticate(username=username, password=password)

        if user is None or not user.is_authenticated:
            ctx.transport.resp_code = 401
            ctx.transport.resp_headers['WWW-Authenticate'] = 'Basic realm="DogService"'
            return "Unauthorized"

        try:
            dog = Dog.objects.get(pk=dog_id)
            if dog.owner != user:
                ctx.transport.resp_code = 403
                return "You do not have permission to view this dog."
            return f"Dog: {dog.name}, Age: {dog.age}, Breed: {dog.breed}"
        except Dog.DoesNotExist:
            ctx.transport.resp_code = 404
            return "Dog not found."

soap_app = Application([DogService], 'dogapp.soap',
                       in_protocol=Soap11(validator='lxml'),
                       out_protocol=Soap11())

dog_service = csrf_exempt(DjangoApplication(soap_app))
```

### 5.2. Test the SOAP API
#### 5.2.1. Using Postman to Send Authenticated Requests
- Open Postman and create a new request.
- Set the method to `POST` and the URL to `http://localhost:8000/soap/dogservice/`.
- In the Authorization tab, select `Basic Auth`.
    - Enter your username and password.
- In the Body tab, select `raw` and set the type to `XML`.
- Enter the SOAP request XML:

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:dog="dogapp.soap">
   <soapenv:Header/>
   <soapenv:Body>
      <dog:get_dog>
         <dog:dog_id>1</dog:dog_id> 
      </dog:get_dog>
   </soapenv:Body>
</soapenv:Envelope>
```
- Send the request.

You should receive the dog's information if you own it, or an error message if you do not.

## 6. Implementing Object-Level Permissions as a centralized permission
We have already implemented object-level permissions in our API views by checking if the `dog.owner` matches the `request.user`. For better code organization, more efficient use of your time, and better maintainability (and therefore better security) you can create a custom permission class that can be reused across endpoints for your APIs. In general it doesn't make sense to write the same code over and over again, so why not write it once and reuse it? This also prevents a lot of errors that can occur if you need to make updates to your permissioning functions. Instead of copy pasting the same code-level patch for every endpoint, it is a single fix for all of them.

### 6.1. For REST Framework
Create a custom permission in `dogapp/permissions.py`:

```python
from rest_framework import permissions

class IsOwner(permissions.BasePermission):
    """
    Custom permission to only allow owners of an object to view it.
    """

    def has_object_permission(self, request, view, obj):
        return obj.owner == request.user
```

Update the `rest_get_dog` view to use this permission:

```python
from .permissions import IsOwner
from rest_framework import generics

class DogDetailAPIView(generics.RetrieveAPIView):
    queryset = Dog.objects.all()
    serializer_class = DogSerializer
    permission_classes = [permissions.IsAuthenticated, IsOwner]
```

Update `urls.py`:

```python
from dogapp.views import DogDetailAPIView

urlpatterns = [
    # ... existing URLs ...
    path('rest/dog/<int:pk>/', DogDetailAPIView.as_view(), name='rest_get_dog'),
]
```

#### 6.2. For GraphQL
We can create a similar `IsOwner` permission function for graphQL that can be reused across all GraphQL endpoints.

In `dogapp/permissions.py`

```python
from graphql import GraphQLError

def IsOwnerGQL(user, obj):
    if obj.owner != user:
        raise GraphQLError('You do not have appropriate permissions.')
```

And in `dogapp/schema.py`:

```python
import graphene
from graphene_django.types import DjangoObjectType
from dogapp.models import Dog
from dogapp.permissions import IsOwner  # Import the permission

class DogType(DjangoObjectType):
    class Meta:
        model = Dog

class Query(graphene.ObjectType):
    dog = graphene.Field(DogType, id=graphene.Int())

    def resolve_dog(self, info, id):
        user = info.context.user
        if user.is_anonymous:
            raise Exception('You do not have appropriate permissions.')
        try:
            dog = Dog.objects.get(pk=id)
            # Use the IsOwnerGQL permission
            IsOwnerGQL(user, dog)
            return dog
        except Dog.DoesNotExist:
            return None
```
Now, the permission logic is centralized, and any changes to permission checks can be made in one place.

### 6.3 SOAP
You can do the same sort of thing for SOAP, but this will probably be the last SOAP example I put you through, so that exercise is left to your own imagination. 



## 7. Summary
In this lab, you enhanced the security of your Django APIs by implementing authentication, access control, and object-level permissions. You learned how to:

- Secure your REST API using Django REST Framework's authentication and permissions
- Secure your GraphQL API using JWT authentication and GraphQL permissions
- Secure your SOAP API by implementing HTTP Basic Authentication in spyne
- Enforce object-level permissions to ensure users can only access data they are authorized to view
- Centralize and reuse permissioning functions to increase the maintainability and security of your code. 

## License
Lesson content: Copyright (C) Dr. Matthew Hale 2024-present.
<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a><br /><span xmlns:dct="http://purl.org/dc/terms/" property="dct:title">This lesson</span> is licensed by the author under a <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License</a>.
