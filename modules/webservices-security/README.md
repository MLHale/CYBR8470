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
    owner = models.ForeignKey(User, on_delete=models.CASCADE, null=True, blank=True)
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

### 1.5 Update your rest endpoint to show the owner id
Since our `rest_get_dog` view is using a serializer to serialize the python model into json, we just need to update our serializer to tell it to send the new owner id field.

```python
from rest_framework import serializers
from .models import Dog

class DogSerializer(serializers.ModelSerializer):
    class Meta:
        model = Dog
        fields = ['id', 'name', 'age', 'breed', 'owner']
```
In a real API, we would also want an endpoint to fetch information about the related `owner` user object...e.g. `/rest/user/<int:user_id>/`.

## 2. Implementing Authentication
We will now implement authentication mechanisms for our APIs to ensure that only authenticated users can access them.

### 2.1. Setting Up Django Authentication
Django provides built-in authentication mechanisms. Ensure that the `django.contrib.auth` and `django.contrib.sessions` apps are included in your `INSTALLED_APPS` in `webservices/settings.py` (they are included by default).

### 2.2. Enabling Authentication in REST Framework
Since we are using Django REST Framework (DRF) for the REST API, we can use DRF's authentication mechanisms.

#### 2.2.1. Update settings.py
In `webservices/settings.py`, add REST framework settings:

```python
# ... other settings properties  ...
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
# ... other imports here ...
from django.contrib.auth import views as auth_views

urlpatterns = [
    # ... other URLs here ...
    path('accounts/login/', auth_views.LoginView.as_view(), name='login'),
    path('accounts/logout/', auth_views.LogoutView.as_view(), name='logout'),
]
```

#### 2.3.2. Create Login Template
Create a directory named `templates` in your project root (where `manage.py` is located), and inside it, create a directory another directory called `registration/`.

Create a file named `login.html` inside `registration/`:
<!-- {% raw %}-->
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
<!-- {% endraw %} -->

#### 2.3.3 Tell settings.py where to find your templates
In `settings.py` update the `TEMPLATES` section to the following:

```python
# ... other imports here ...
import os

# ... other settings properties here ...
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, 'templates'),],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```
This will tell django that the `/templates/` directory is a place where it can look for particular template paths...including the `registration/login.html` template.

### 2.4. Test Authentication
Run the server, navigate to `http://localhost:8000/accounts/login/`, and try to login using your username and password to ensure the login page is working. If all goes well, you will be authenticated, receive a session token, and redirected to a profile page (that doesn't exist). You could create one, but it is outside the scope of this lesson. 

Once you've logged in, try to logout at `http://localhost:8000/accounts/logout`. Now test to see what happens when you try to login with an incorrect username/password or both. Is the error message a good one? What might be better?

Notice, since we didnt create our own logout page, django defaults to using the admin interface logout page (which ships with django by default). We could override that by writing out own logout html template, but again that is outside the scope of this lesson.

## 3. Securing the REST API
Now, we will secure the REST API so that only authenticated users can access it, and users can only access their own `Dog` objects.

### 3.1. Update the REST API View
In `dogapp/views.py`, update the `rest_get_dog` view to enforce permissions:

```python
# ... other imports here ...
from rest_framework import status, permissions, renderers
from rest_framework.decorators import api_view, permission_classes, renderer_classes

# ... other views here ...

@api_view(['GET'])
@permission_classes([permissions.IsAuthenticated])
@renderer_classes([renderers.JSONRenderer])
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
#### 3.2.1. Using Session Authentication in Browser
Since we are using `SessionAuthentication`, we need to log in to obtain a session cookie.

- Open a new browswer tab and navigate to `http://localhost:8000/accounts/login/`.
- Add the following key-value pairs:
  - `username`: your username
  - `password`: your password
- Send the request.
- If login is successful, your browser will capture a session token for the rest of your session. It will get sent in subsequent requests.

#### 3.2.2. Use the Session Cookie for API Requests
The session cookie will automatically be sent with future requests in the same browser tab. Lets try using it to access our dog objects.

Try visiting `localhost:8000/rest/dog/1` does it work? How about `localhost:8000/rest/dog/2`?

if you have assigned those dogs to different users, you should only be able to view the ones you own.

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

# ... other settings properties here ...

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

# ... other settings properties here ...

INSTALLED_APPS = [
    # ... other apps ...
    'rest_framework',
    'rest_framework_simplejwt',
]
```

#### 3.3.4. Run Migrations
```bash
python manage.py migrate
```
#### 3.3.5. Update `urls.py` to Include Token Paths
Add the following imports and URL patterns to `webservices/urls.py`:

```python
# ... other imports here ...
from rest_framework_simplejwt.views import (
    TokenObtainPairView,
    TokenRefreshView,
)

urlpatterns = [
    # ... other URLs here ...
    path('api/token/', TokenObtainPairView.as_view(), name='token_obtain_pair'),
    path('api/token/refresh/', TokenRefreshView.as_view(), name='token_refresh'),
]
```

#### 3.3.6. Test with Postman
- Obtain a JWT token by making a POST request to `http://localhost:8000/api/token/` with your username and password in the body (make sure the request is `raw` with contentype `application/json`.

```json
{
    "username": "your_username",
    "password": "your_password"
}
```

- Use the obtained token to authenticate your requests to the REST API by adding an `Authorization` header with the value `Bearer your_token_here`.

#### 3.3.7 Using the token
Make a new request to `http://localhost:8000/rest/dog/1`. 
- Add the key `Authorization` header to the `headers` tab in POSTMAN.
- For the value set it to `Bearer your_token_here`, replacing your_token with the value returned from the `access` token retrieved in the earlier request to `/api/token`
- Once you have the header set, send the request to `http://localhost:8000/rest/dog/1` and `http://localhost:8000/rest/dog/2`, the API should only allow you access to the dog you own, not the others owned by other owners.


## 4. Securing the GraphQL API
In this section, we will secure the GraphQL API using JSON Web Tokens (JWT) for authentication instead of session-based authentication. JWT is a stateless authentication mechanism suitable for APIs, allowing clients to authenticate without the need for CSRF tokens or session cookies.

### 4.1. Install `django-graphql-jwt`
We will use the `django-graphql-jwt` library to implement JWT authentication in our GraphQL API.

#### 4.1.1 Install the library
Run the following command to install `django-graphql-jwt`:

```bash
pip install django-graphql-jwt
```

#### 4.1.2 Configure django
Configure your Django project to use JWT authentication for GraphQL.

Add `graphql_jwt` to `INSTALLED_APPS`
In `webservices/settings.py`, add `graphql_jwt` to the list of installed apps:

```python
# ... other settings properties here ...
INSTALLED_APPS = [
    # ... other installed apps ...
    'graphene_django',
    'graphql_jwt',
]
```
#### 4.1.3. Configure Authentication Backends
Update the AUTHENTICATION_BACKENDS setting to include JSONWebTokenBackend:

```python
# ... other settings properties here ...
AUTHENTICATION_BACKENDS = [
    'graphql_jwt.backends.JSONWebTokenBackend',
    'django.contrib.auth.backends.ModelBackend',
]
```

#### 4.1.4. Configure `GRAPHENE`
```python
# ... other settings properties here ...

GRAPHENE = {
    'SCHEMA': 'dogapp.schema.schema',
    'MIDDLEWARE': [
        'graphql_jwt.middleware.JSONWebTokenMiddleware',
    ],
}
```
### 4.1. Update the GraphQL Schema
In `dogapp/schema.py`, import the required modules and enforce authentication:

```python
import graphene
from graphene_django.types import DjangoObjectType
from dogapp.models import Dog
from graphql import GraphQLError
from django.contrib.auth import get_user_model
import graphql_jwt

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
        
class Mutation(graphene.ObjectType):
    token_auth = graphql_jwt.ObtainJSONWebToken.Field()
    verify_token = graphql_jwt.Verify.Field()
    refresh_token = graphql_jwt.Refresh.Field()

schema = graphene.Schema(query=Query, mutation=Mutation)
```

In GraphQL, a mutation is a type of operation that allows clients to create, update, or delete data on the server. Mutations enable clients to send data to the server and define precisely how the data should change. Each mutation can also specify what data should be returned after the operation, which can be particularly useful for updating the client-side state immediately after the mutation.

Again - thinks about the kinds of error's you give. Is this object-level error sufficient? What might it tell an attacker about the existence of the dog they are searching for? Think like an adversary and adjust your error messages to reveal as little as possible (preferrably nothing).


### 4.2. Testing with Postman

#### 4.2.1
Obtain a JWT token by making a mutation request. Make a post request to `localhost:8000/graphql/`, set the `BODY` content type to `GraphQL`, then send the following request:

```graphql
mutation {
  tokenAuth(username: "your_username", password: "your_password") {
    token
  }
}
```
#### 4.2.2
Use the obtained token to authenticate your GraphQL requests by adding an `Authorization` header with the value `JWT your_token_here`.

- make a new request to `localhost:8000/graphql/`
- include the key/value `Authorization` and `JWT your_token_here` replacing the `your_token_here` part with the token you received from the prior request
- Now try the following query to see what you see:

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

Try it for id 2 instead. Do you get a permission issue?

## 5. Securing the SOAP API
Securing SOAP APIs in Django using `spyne` requires custom authentication mechanisms because `spyne` doesn't directly integrate with Django's authentication system.

### 5.1. Implementing Basic Authentication for SOAP
We will implement HTTP Basic Authentication for the SOAP service.

#### 5.1.1. Update the SOAP Service
In `dogapp/views.py`, update the `DogService` class to check for authentication:

```python
# ... other imports here ...
from django.contrib.auth import authenticate

# ... other views here ...

def authenticate_user(request):
    """
    Authenticates a user using HTTP Basic Authentication.

    Returns:
        user (User): The authenticated user object, or None if authentication fails.
        error_response (tuple): A tuple containing the HTTP status code and message if authentication fails.
    """
    if 'HTTP_AUTHORIZATION' in request:
        auth = request['HTTP_AUTHORIZATION'].split()
        if len(auth) == 2 and auth[0].lower() == 'basic':
            import base64
            try:
                username, password = base64.b64decode(auth[1]).decode('utf-8').split(':')
                user = authenticate(username=username, password=password)
                if user is not None and user.is_authenticated:
                    return user, None
            except (UnicodeDecodeError, ValueError):
                pass

    # Authentication failed
    return None, (401, 'Unauthorized', {'WWW-Authenticate': 'Basic realm="DogService"'})

class DogService(ServiceBase):
    @rpc(Integer, _returns=Unicode)
    def get_dog(ctx, dog_id):
        request = ctx.transport.req
        user = None
        
        user, error_response = authenticate_user(request)
        # Get the Basic Auth credentials
        if user is None:
            status_code, message, headers = error_response
            ctx.transport.resp_code = status_code
            for header, value in headers.items():
                ctx.transport.resp_headers[header] = value
            return message

        try:
            dog = Dog.objects.get(pk=dog_id)
            if dog.owner != user:
                return f"You do not have permission to view this dog."
            return f"Dog: {dog.name}, Age: {dog.age}, Breed: {dog.breed}"
        except Dog.DoesNotExist:
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
# ... other imports here ...
from .permissions import IsOwner
from rest_framework import generics

# ... other views here ...
class DogDetailAPIView(generics.RetrieveAPIView):
    queryset = Dog.objects.all()
    serializer_class = DogSerializer
    permission_classes = [permissions.IsAuthenticated, IsOwner]
```

Update `urls.py`:

```python
# ... other imports here ...
from dogapp.views import DogDetailAPIView

urlpatterns = [
    # ... other URLs here ...
    #path('rest/dog/<int:dog_id>/', rest_get_dog, name='rest_get_dog'),
    path('rest/dog/<int:pk>/', DogDetailAPIView.as_view(), name='rest_get_dog'),
]
```

#### 6.2. For GraphQL
We can create a similar `IsOwner` permission function for graphQL that can be reused across all GraphQL endpoints.

In `dogapp/permissions.py`

```python
# ... other imports here ...
from graphql import GraphQLError

# ... other permissions here ...
def IsOwnerGQL(user, obj):
    if obj.owner != user:
        raise GraphQLError('You do not have appropriate permissions.')
```

And in `dogapp/schema.py`:

```python
# ... other imports here ...
from dogapp.permissions import IsOwnerGQL  # Import the permission

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

# ... mutations and other stuff here ...
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
