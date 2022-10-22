# install-on-windows


## Step 1: 

Download this stable release of uWSGI

````
https://projects.unbit.it/downloads/uwsgi-2.0.19.1.tar.gz
````

## Step 2: 

Extract the `tar` file inside the `site-packages` folder of the virtual environment.

For example the extracted path to uwsgi should be:

`\my_env\lib\site-packages\uwsgi-2.0.19.1`

## Step 3: 

Open `uwsgi-2.0.19.1\uwsgiconfig.py` And do the following edits:

````
import platform
...
````

Then wherever you encounter
````
...
os.uname()[x-index]
...
````

modify it with

````
...
platform.uname()[x-index]
...
````

## Step 4: 

Finally, Open powershell and cd into `\my_env\lib\site-packages\uwsgi-2.0.19.1` and run:

````
python setup.py install
````

Got an error? Check out this

## Step 5: 

Run `pip install uwsgi` you'll get Requirement already satisfied:. Try pip freeze you'll see uwsgi is listed. Which means you have now successfully installed uwsgi. Congrats!

