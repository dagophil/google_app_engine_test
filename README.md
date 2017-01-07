# Basic instructions for the Google App Engine

This repository contains a hello world python example for the Google App Engine.

### Setup
* Register for the Google Cloud Platform.
* Create a Google Cloud Platform Console project.
* Install the Google Cloud SDK.
* Initialize the `gcloud` configuration: `gcloud init`
* From now on all `gcloud` commands respect that configuration. Some commands make changes to the current project, so
  you need to be careful that the correct project is specified in the configuration. You can use `gcloud init` to create
  several project-specific configurations and easily switch between them.
* Useful command: List existing projects: `gcloud projects list`

### Hello world
A simple hello world python program (`main.py`) for the Google App Engine can look like this:
```python
import webapp2


class MainPage(webapp2.RequestHandler):
    def get(self):
        self.response.headers['Content-Type'] = 'text/plain'
        self.response.write('Hello, World!')


app = webapp2.WSGIApplication([
    ('/', MainPage),
], debug=True)
```

The project needs a description file `app.yaml`, which can look like this:
```yaml
runtime: python27
api_version: 1
threadsafe: true

handlers:
- url: /.*
  script: main.app
```

### Run the app locally
If the default file association for python files is set to the python interpreter that came with the Google Cloud SDK,
you can run the app locally using `dev_appserver.py app.yaml`.

Otherwise you need call the python interpreter yourself using `python full/path/to/dev_appserver.py app.yaml`.

### Upload the app
The app can be uploaded to the Google Cloud Platform using `gcloud app deploy`.

### Test framework
The app in `main.py` can be tested with a simple python program `main_test.py` that looks like this:
```python
import webtest

import main


def test_get():
    app = webtest.TestApp(main.app)

    response = app.get('/')

    assert response.status_int == 200
    assert response.body == 'Hello, World!'


if __name__ == "__main__":
    test_get()
```
