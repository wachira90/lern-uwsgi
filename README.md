# lern-uwsgi

## install uwsgi

````
pip install uwsgi
````

## file app.py

nano app.py

````
def application(env, start_response):
	start_response('200 OK', [('Content-Type','text/html')])
	return [b"Hello World"]
````  

## start service

````  
uwsgi --http :7100 --wsgi-file app.py --master --processes 4 --threads 2 --thunder-lock
````  

#### OR 

````
uwsgi --http 172.16.1.5:7100 --chdir /home/django/rest-test/appmain --wsgi-file appmain/wsgi.py 
uwsgi --http 172.16.1.5:7100 --chdir /home/django/rest-test/appmain --wsgi-file appmain/wsgi.py --master --processes 4 --threads 2
uwsgi --http :7100 --wsgi-file wsgi.py --master --processes 4 --threads 2
````



````
import os
STATIC_URL = '/static/'
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, 'static')
]
STATIC_ROOT = os.path.join(BASE_DIR, 'static/')
````
````
python manage.py collectstatic
````
