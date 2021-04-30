.. title: Deploying Dash application to Heroku (for FreeBSD)
.. slug: deploying-dash-application-to-heroku-for-freebsd
.. date: 2021-01-03 10:18:32 UTC+08:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text

Before you begin, ensure that you have the following prerequisite before you begin,

Prerequisite
------------
1. Heroku_ account
2. git
3. python and virtualenv

Creating project from scratch
-----------------------------

Step 1. Create a new folder for your new Dash_ app
==================================================

Here we will create a new folder to put our project in

   .. code-block:: bash

      $ mkdir my_dash_app
      $ cd my_dash_app


Step 2. Init the folder with git and a virtualenv
=================================================

   .. code-block:: bash

      $ git init                          # initializes an empty git repo
      $ virtualenv venv                   # creates a virtualenv called "venv"
      $ source venv/bin/activate          # uses the virtualenv


Step 3. (Pip) install Dash_ and Gunicorn_ package
=================================================

   .. code-block:: bash

      $ pip install dash
      $ pip install plotly
      $ pip install gunicorn

Step 4. Initialize the folder with sample app for deployment
============================================================

Create the following files in the project folder

**app.py**

   .. code-block:: python

      import dash
      import dash_core_components as dcc
      import dash_html_components as html

      external_stylesheets = ['https://codepen.io/chriddyp/pen/bWLwgP.css']

      app = dash.Dash(__name__, external_stylesheets=external_stylesheets)

      server = app.server

      top_markdown_text = '''
      This is my first deployed app
      '''

      app.layout = html.Div([

          dcc.Markdown(children=top_markdown_text),

      ])

      if __name__ == '__main__':
          app.run_server(debug=True)

**.gitignore**

   .. code-block:: text

      venv
      *.pyc
      .env

**Procfile**

(Note that app refers to the filename app.py. server refers to the variable server inside that file).

   .. code-block:: text

      web: gunicorn app:server

**requirements.txt**

This is needed for heroku buildpacks to recognize that this is a python project.
Basic requirements.txt

   .. code-block:: text
   
      dash
      plotly
      gunicorn

or you can fill this with:

   .. code-block:: bash

      $ pip freeze > requirements.txt

Step 5. Installing Heroku CLI (if you have not)
===============================================

The CLI_ tool can be downloaded and installed on your system. There are few ways to install based on your operating system. As for myself, 
I am using FreeBSD, therefore I will just install the `NPM package`_ using yarn_.

Note: The method I am using will install heroku cli tools to the dash project locally.

   .. code-block:: bash

      $ sudo pkg install yarn             # Make sure yarn is installed
      $ yarn add heroku                   # Will add heroku to the local project folder
      $ yarn run heroku                   # this will run heroku and print help messages
      $ yarn run heroku [cmd]             # To run heroku command

Step 6. Initialize Heroku, add files to Git, and deploy
=======================================================

Note: as stated above, as I am running the heroku cli as a npm package installed locally, I need to use yarn run command.

   .. code-block:: bash

      $ yarn run heroku create my-dash-app # change my-dash-app to a unique name
      $ git add .                          # add all files to git
      $ git commit -m 'Initial app commit'
      $ git push heroku master             # deploy code to heroku
      $ yarn run heroku ps:scale web=1     # run the app with a 1 heroku "dyno"

After this, you should be able to view your app at https://my-dash-app.herokuapp.com or whatever the app name
you have set to. 

Step 7. Update the code and redeploy
====================================

When you have made changes to the git repo, you can update heroku again using,

   .. code-block:: bash

      $ git status                        # view the changes
      $ git add .                         # add all the changes
      $ git commit -m 'a description of the changes'
      $ git push heroku master

References
----------

`Deploying Dash Apps`_

`Getting Started on Heroku with Python`_

`dash_heroku_deployment`_

.. _dash_heroku_deployment: https://github.com/francoisstamant/dash_heroku_deployment
.. _Deploying Dash Apps: https://dash.plotly.com/deployment
.. _Getting Started on Heroku with Python: https://devcenter.heroku.com/articles/getting-started-with-python
.. _Dash: https://dash.plotly.com/
.. _Heroku: https://www.heroku.com/
.. _Gunicorn: https://gunicorn.org/
.. _CLI: https://devcenter.heroku.com/articles/heroku-cli
.. _NPM package: https://www.npmjs.com/package/heroku
.. _yarn: https://yarnpkg.com/
