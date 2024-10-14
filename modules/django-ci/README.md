# Django CI Lab

## Introduction
Continuous Integration (CI) is a development practice where developers frequently integrate code into a shared repository. Each integration is verified by an automated build and tests, allowing teams to detect problems early.

In this lab, we'll import a simplified **Dog API** that has already been dockerized. Next, we will write test cases for it, and then set up a CI pipeline using GitHub Actions to automatically build and test our Dockerized Django application whenever changes are pushed to the repository. We'll introduce a failing test case to demonstrate how GitHub Actions reports failures in the CI pipeline.

## Prerequisites
- Git: Installed on your local machine.
- Docker: Installed and running on your local machine.
- GitHub Account: You'll need an account to host your repository.
- Basic Knowledge:
  - Django framework
  - REST Framework (Django REST Framework)
  - Docker and Docker Compose
  - Git version control system
    - Optional: Text editor or IDE (e.g., VSCode, PyCharm)

## Setting Up the Django App  
On Github, fork the repository here: https://github.com/MLHale/django-ci-lab

Once you have your own copy, clone the repository to your local machine using git:

```bash
git clone <url-to-your-forked-repository
cd django-ci-lab
```

Once in this folder, consider adding the directory to your favorite IDE (e.g. VScode). Take a look over the files, the structure should be really familiar - this is a django project called webservices, that has an app called dogapi that has been created inside it. The dogapi has one `api endpoint` written in DjangoREST framework.

To run the app and confirm that it is working, do the following:

```bash
docker compose up
```

You should see that the server is running on `localhost:8000` and you should be able to visit it in the browser to see the django rest interface.

## Writing some test cases:
Lets open up webservices/dogapi/tests.py and write some test cases:

```python
from django.test import TestCase
from rest_framework.test import APIClient
from rest_framework import status
from .models import Dog

class DogAPITestCase(TestCase):
    def setUp(self):
        self.client = APIClient()
        self.dog_data = {'name': 'Buddy', 'breed': 'Golden Retriever', 'age': 5}
        self.response = self.client.post('/dogs/', self.dog_data, format='json')

    def test_create_dog(self):
        self.assertEqual(self.response.status_code, status.HTTP_201_CREATED)
        self.assertEqual(Dog.objects.count(), 1)
        self.assertEqual(Dog.objects.get().name, 'Buddy')

    def test_get_dog(self):
        dog = Dog.objects.create(name='Max', breed='Labrador', age=3)
        response = self.client.get(f'/dogs/{dog.id}/')
        self.assertEqual(response.status_code, status.HTTP_200_OK)
        self.assertEqual(response.data['name'], 'Max')

    def test_update_dog(self):
        dog = Dog.objects.get()
        new_data = {'name': 'Buddy', 'breed': 'Golden Retriever', 'age': 6}
        response = self.client.put(f'/dogs/{dog.id}/', new_data, format='json')
        self.assertEqual(response.status_code, status.HTTP_200_OK)
        self.assertEqual(response.data['age'], 6)

    def test_delete_dog(self):
        dog = Dog.objects.get()
        response = self.client.delete(f'/dogs/{dog.id}/')
        self.assertEqual(response.status_code, status.HTTP_204_NO_CONTENT)
        self.assertEqual(Dog.objects.count(), 0)
```

