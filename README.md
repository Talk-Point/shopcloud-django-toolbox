# shopcloud-django-toolbox

## install

```sh
$ pip install shopcloud-django-toolbox
```

## deploy

```sh
# change version Number in setup.py ändern und dann ...
# delete build and dist-directory
$ rm -rf build
$ rm -rf dist
$ pip3 install wheel twine
$ python3 setup.py sdist bdist_wheel
$ twine upload dist/* 
```

### Usage example

Allow usage of GID Features

```python3
from django.db import models
from shopcloud_django_toolbox import GID


class FooBarModel(models.Model, GID):
    ...
```

SetupClass creates admin user and migrate database for tests.

```python3
from shopcloud_django_toolbox import SetupClass


class TestClass(SetupClass):
    ...
```

Checks all basic admin menu functions.

```python3
from shopcloud_django_toolbox import TestAdminTestCase


class TestAdminPages(TestAdminTestCase):
    MODULE = 'url-name'

    def test_admin_easylineitem(self):
        self.run_for_model(
            'url-model-name',
            is_check_add=False,  # deactivate when add function is deactivated
            is_check_template=False,  # deactivate generic template check
            is_check_search=True,  # activate searchbar check
        )
```

Check if '.../docs' Endpoint is rendering correct.

```python3
from shopcloud_django_toolbox.tests import BaseTestAPIEndpointDoc


class TestDocEntpoint(BaseTestAPIEndpointDoc):
    pass
```

Checks all given endpoints require authorization.

```python3
from shopcloud_django_toolbox.tests import BaseTestApiAuthorization


class TestApiAuthorization(BaseTestApiAuthorization):
    app_name = "url-module-name"

    def test_model_foo(self):
        self.run_test_endpoint("url-model-name")

    def test_model_bar(self):
        self.run_test_endpoint("url-model-name")
```

Event usage example

```python3
from shopcloud_django_toolbox import Event


class FooBarModel(models.Model):
    ...

    def do_event(self):
        event = Event(
            name="de.talk-point.platform/module/model/sync",
            model=self,
        )
        event.add_task(
            queue="default",
            url=f"moduke/api/model/{self.id}/action/",
            json={}
        )
        event.fire()
```