# uwsgi nginx



## environment 

````
 virtualenv env
 source env/bin/activate
 pip install uwsgi
 
````

## wsgi.py

nano wsgi.py

````
def application(env, start_response):
    start_response('200 OK', [('Content-Type', 'text/html')])
    return ["Hello!"]
````


## run

````
uwsgi --socket 127.0.0.1:7100 --protocol=http -w wsgi
````

## nginx config

````
worker_processes 1;

events {

    worker_connections 1024;

}

http {

    sendfile on;
    
    gzip              on;
    gzip_http_version 1.0;
    gzip_proxied      any;
    gzip_min_length   500;
    gzip_disable      "MSIE [1-6]\.";
    gzip_types        text/plain text/xml text/css
                      text/comma-separated-values
                      text/javascript
                      application/x-javascript
                      application/atom+xml;

    # Configuration containing list of application servers
    upstream uwsgicluster {
    
        server 127.0.0.1:7100;
        # server 127.0.0.1:8081;
        # ..
        # .
    
    }

    # Configuration for Nginx
    server {
    
        # Running port
        listen 80;

        # Settings to by-pass for static files 
        location ^~ /static/  {
        
            # Example:
            # root /full/path/to/application/static/file/dir;
            root /app/static/;
        
        }
        
        # Serve a static file (ex. favico) outside static dir.
        location = /favico.ico  {
        
            root /app/favico.ico;
        
        }

        # Proxying connections to application servers
        location / {
        
            include            uwsgi_params;
            uwsgi_pass         uwsgicluster;

            proxy_redirect     off;
            proxy_set_header   Host $host;
            proxy_set_header   X-Real-IP $remote_addr;
            proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header   X-Forwarded-Host $server_name;
        
        }
    }
}
````

## Usage 

````
# uwsgi [option] [option 2] .. -w [wsgi.py with application callable]

# Simple server running *wsgi*
uwsgi --socket 127.0.0.1:8080 -w wsgi

# Running Pyramid (Paster) applications
uwsgi --ini-paste production.ini

# Running web2py applications
uwsgi --pythonpath /path/to/app --module wsgihandler

# Running WSGI application with specific module / callable names
uwsgi --module wsgi_module_name --callable application_callable_name
uwsgi -w wsgi_module_name:application_callable_name
````

## example_config.ini  

nano example_config.ini

````
[uwsgi]
# -------------
# Settings:
# key = value
# Comments >> #
# -------------

# socket = [addr:port]
socket = 127.0.0.1:7100

# Base application directory
# chdir = /full/path
chdir  = /my_app

# WSGI module and callable
# module = [wsgi_module_name]:[application_callable_name]
module = app:application

# master = [master process (true of false)]
master = true

# processes = [number of processes]
processes = 5
````

