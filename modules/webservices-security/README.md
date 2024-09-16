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
