# unemployment-2023-testing-prep

# unemployment-inclass-summer-2023
![tests](https://github.com/s2t2/unemployment-2023-testing-prep/actions/workflows/python-app.yml/badge.svg)
## Setup
Obtain an [AlphaVantage API Key](https://www.alphavantage.co/support/#api-key). A normal key should be fine, but alternatively you can use one of the prof's "premium" keys. Then create a file called ".env" and place it inside (like the following example):
```sh
# this is the ".env" file (in the root directory of the repo)
ALPHAVANTAGE_API_KEY="____________"
```
Create a virtual environment:
```sh
conda create -n unemployment-env python=3.10
```
```sh
conda activate unemployment-env
```
Install third-party packages:
```sh
pip install -r requirements.txt
```
## Usage
Run the report:
```sh
python app/unemployment.py
python -m app.unemployment
```

Run the web app:

```sh
# Mac OS:
FLASK_APP=web_app flask run

# Windows OS:
# ... if `export` doesn't work for you, try `set` instead
# ... or try a ".env" file approach
export FLASK_APP=web_app
flask run
```


## Testing

```sh
pytest
```

## Deployment
  6 changes: 5 additions & 1 deletion6  
requirements.txt
@@ -6,9 +6,13 @@ pandas

plotly


python-dotenv

# for web app:
flask
#gunicorn



# for testing:
pytest
 19 changes: 19 additions & 0 deletions19  
web_app/__init__.py
@@ -0,0 +1,19 @@

# this is the "web_app/__init__.py" file...

from flask import Flask

from web_app.routes.home_routes import home_routes
#from web_app.routes.book_routes import book_routes
#from web_app.routes.weather_routes import weather_routes

def create_app():
    app = Flask(__name__)
    app.register_blueprint(home_routes)
    #app.register_blueprint(book_routes)
    #app.register_blueprint(weather_routes)
    return app

if __name__ == "__main__":
    my_app = create_app()
    my_app.run(debug=True)
 35 changes: 35 additions & 0 deletions35  
web_app/routes/home_routes.py
@@ -0,0 +1,35 @@
# this is the "web_app/routes/home_routes.py" file...

from flask import Blueprint, request, render_template

home_routes = Blueprint("home_routes", __name__)

@home_routes.route("/")
@home_routes.route("/home")
def index():
    print("HOME...")
    return "Welcome Home"
    #return render_template("home.html")

@home_routes.route("/about")
def about():
    print("ABOUT...")
    return "About Me"
    #return render_template("about.html")

@home_routes.route("/hello")
def hello_world():
    print("HELLO...")

    # if the request contains url params, for example a request to "/hello?name=Harper"
    # the request object's args property will hold the values in a dictionary-like structure
    url_params = dict(request.args)
    print("URL PARAMS:", url_params) #> can be empty like {} or full of params like {"name":"Harper"}

    # get a specific key called "name" if available, otherwise use some specified default value
    # see also: https://www.w3schools.com/python/ref_dictionary_get.asp
    name = url_params.get("name") or "World"

    message = f"Hello, {name}!"
    return message
    #return render_template("hello.html", message=message)
