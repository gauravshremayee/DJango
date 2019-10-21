# DJango
Python Django Projects

# DJANGO FULL STACK


https://www.digitalocean.com/community/tutorials/how-to-install-anaconda-on-ubuntu-18-04-quickstart


$curl -O https://repo.anaconda.com/archive/Anaconda3-2019.03-Linux-x86_64.sh


$sha256sum Anaconda3-2019.03-Linux-x86_64.sh


45c851b7497cc14d5ca060064394569f724b67d9b5f98a926ed49b834a6bb73a  Anaconda3-2019.03-Linux-x86_64.sh



$bash Anaconda3-2019.03-Linux-x86_64.sh

Anaconda3 will now be installed into this location:
/home/sammy/anaconda3

  - Press ENTER to confirm the location
  - Press CTRL-C to abort the installation
  - Or specify a different location below

[/home/sammy/anaconda3] >>>


It is recommended that you type yes to use the conda command.



On the terminal

source ~/.bashrc

Conda prompt will start 

conda list

conda create --name penguin_env python=3

conda activate penguin_env




FULL STACK DJANGO:


git clone https://github.com/chrisgschon/titanic-django

https://www.kaggle.com/c/titanic


cd titanic-jango
conda create -n titanic-django
conda activate titanic-django
conda install pip
pip install -r requirements.txt


START A NEW DJANGO PROJECT

django-admin startproject titanicapi


python manage.py migrate


CREATE AN API

django-admin startapp api

￼in titanicapi/settings.py

Add api 
INSTALLED_APPS = [

    'django.contrib.admin',

    'django.contrib.auth',

    'django.contrib.contenttypes',

    'django.contrib.sessions',

    'django.contrib.messages',

    'django.contrib.staticfiles',

    'api',

]


In api directory create functions.py


import dill as pickle

import pandas as pd



def classify_passenger(model, data):

    df = pd.read_json(data)

    pred = model.predict(df)

    return(pred)



def load_model(path):

    with open(path,'rb') as f:

        model = pickle.load(f)

    return(model)



api/urls.py


from django.urls import path

from . import views



urlpatterns = [

    path('classification/', views.get_classification.as_view()),

]


Edit
api/views.py 

from django.shortcuts import render, get_object_or_404, redirect

from rest_framework.views import APIView

from rest_framework.response import Response



import json

from .functions import classify_passenger, load_model



class get_classification(APIView):

  def post(self, request):

    model = load_model('./api/titanic_model.pk')

    data = request.data

    prediction = classify_passenger(model = model, data = data)

    return(Response(prediction))



Titanicapi/urls.py

from django.contrib import admin

from django.urls import path, include





urlpatterns = [

    path('admin/', admin.site.urls),

    path('api/', include('api.urls')),

]



$python manage.py runserver


Run this at local machine or remote and edit accordingly
import json

import requests

import pandas as pd



df = pd.read_csv('./titanic_test.csv')



header = {'Content-Type': 'application/json', \

                  'Accept': 'application/json'}

data = df.drop('PassengerId', axis = 1).to_json(orient='records')





resp = requests.post("http://127.0.0.1:8000/api/classification/", \

                    data = json.dumps(data),\

                    headers= header)



df['Survived'] = resp.json()



df.head(10) 











