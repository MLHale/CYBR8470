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

## Running the test cases
Lets try out test cases locally. To run them using our containerized django app, we can simply do the following:

```bash
docker compose run web python manage.py test
```

This tells docker compose to run the service named web and then pipe in the command "python manage.py test". This gets run just as if you ran it in your CLI on your local machine. You should see some output on command line indicating that the tests ran.

...ok cool - so we have some code tests, but how do we integrate them with github?

## Defining continuous integration tests in GitHub Actions

### Step 1: Create a Workflow Directory
In your project directory, create a directory for GitHub workflows.

```bash
mkdir -p .github/workflows
```

### Step 2: Create a Workflow File
Create a new file named `ci.yml` inside `.github/workflows/.` Edit the file to include the following steps:

```yml
# .github/workflows/ci.yml

name: CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v3

    - name: Check Docker version
      run: docker version
      
    - name: Build Docker Image
      run: docker compose up -d --build

    - name: Run Migrations
      run: docker compose exec web python manage.py migrate

    - name: Run Tests
      run: docker compose exec web python manage.py test

    - name: Shut down services
      run: docker compose down
```

This file specifies the triggers, i.e. the `on` block and the actions, i.e. the `jobs` block that should occur on the repo. This file tells GitHub to run all of the jobs, including multi-step setup, testing, and teardown, whenever a repo user pushes to the repo or makes a pull request between branches. The second to last step in the test job actually runs the django tests we wrote for our dogapi.

Go and and save the file and then lets test it out on Github by making a new commit and pushing it to our repository (the one you forked earlier).

```bash
git add -A
git commit -m "added test cases and a github actions ci workflow"
git push
```

- Now go to github
- click on the repository you forked
- click on `actions` tab
- click on the `test` job button to view the details. You should see everything complete successfully and eventually create a green checkmark next to the tests.

## What happens when tests fail?
To find out what failing tests look like, lets create a new test that will always fail. Open `webservices/dogapi/tests.py` and add the following to the bottom of the file

```python
   def test_fail_on_purpose(self):
        """This test is designed to fail."""
        self.assertEqual(1, 0, "Intentional failure to test CI pipeline")
```

This tests claims that 1=0, which will always be false. It will generate a failed test.

We can test it locally using docker compose again:

```bash
docker compose run web python manage.py test
```

Now lets see what happens on github:
```bash
git add -A
git commit -m "added an auto-failing test"
git push
```

- Now go to github
- click on the repository you forked
- click on `actions` tab
- click on the `test` job button to view the details. You should see everything complete successfully and eventually create a `red checkmark` next to the tests.

Pretty cool yeah? This is how you can help do automate your testing workflow and check your code continuously as you make edits.
