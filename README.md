MLSA workshop, April 24th, 2022
# webapp_translator_Azure
In this module, you'll build a website using Flask and Cognitive Services to translate text.<br>

* Learn how to set up a Flask development environment<br>
* Learn how to use Flask to build a form<br>
* Learn how to use the Translator service to translate text<br>

## Prerequisites
An Azure account. Learn how to create a free account in Create an [Azure account for students](https://azure.microsoft.com/en-us/free/students/?WT.mc_id=academic-0000-cxa)
<br>[Python3.6](https://www.python.org/downloads/) or later and VS code installed on your computer. <br> At the top of the article, choose the instructions for your configuration: Windows, Linux, or macOS.<br>
[Visual Studio Code](https://code.visualstudio.com/)<br>

## PART 1
1. check python version, must be > 3.6:<br>
```
python --version
```

2. create the project directory<br>
```
# Windows
md contoso
cd contoso

## macOS or Linux
mkdir contoso
cd contoso
```
3. Create a Python virtual environment<br>
```
# Windows
# Create the environment
python -m venv venv
# Activate the environment
.\venv\scripts\activate

# macOS or Linux
# Create the environment
python -m venv venv
# Activate the environment
source ./venv/bin/activate
```
4. Install Flask and libraries<br>
* from the current terminal, open VS code:<br>
```
code .
```
* In Visual Studio Code, in the Explorer window, select New File next to the contoso directory<br>

* Name the file requirements.txt, and add the following text:<br>

```
flask
python-dotenv
requests
```
* Save the file by clicking Ctrl-S, or Cmd-S on a Mac<br>
* Return to the command or terminal window and perform the installation by using pip to run the following command:<br>
```
pip install -r requirements.txt
```
The command downloads the necessary libraries and their dependencies.

## PART 2

1. Returning to the instance of Visual Studio Code we were using previously, create a new file named app.py by clicking New file in the Explorer tab.<br>
2. set the python interpreter by clicking CTRL + shift + P to the following:<br>
```
.venv\Scripts\python.exe
```
this resolves any dependancies issuies that your local interpreter might have<br>

3.Add the code to create your Flask application<br>

```
from flask import Flask, redirect, url_for, request, render_template, session

app = Flask(__name__)
```
The import statement includes references to Flask, which is the core of any Flask application. The "app" variable will be our core application. We'll use it when we register our routes in the next step.<br>

4. Add the route<br>

```
@app.route('/', methods=['GET'])
def index():
    return render_template('index.html')
```
This route is sometimes called either the default or the index route, because it's the one that will be used if the user doesn't provide anything beyond the name of the domain or server.<br>

By using @app.route, we indicate the route we want to create. The path will be /, which is the default route. We indicate this will be used for GET. If a GET request comes in for /, Flask will automatically call the function declared immediately below the decorator, index in our case. In the body of index, we indicate that we'll return an HTML template named index.html to the user.<br>

5. Create the HTML template for our form<br>
Jinja, the templating engine for Flask, focuses quite heavily on HTML. As a result, we can use all the existing HTML skills and tools we already have. We're going to use Bootstrap to lay out our page, to make it a little prettier. By using Bootstrap, we'll use different CSS classes on our HTML. <br>
* Create a new folder named templates by selecting New Folder in the Explorer tool in Visual Studio Code<br>
* Create a new folder named templates by selecting New Folder in the Explorer tool in Visual Studio Code<br>
* Name the file index.html<br>
* Add the following HTML
```<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/css/bootstrap.min.css"
        integrity="sha384-TX8t27EcRE3e/ihU7zmQxVncDAy5uIKz4rEkgIXeMed4M0jlfIDPvg6uqKI2xXr2" crossorigin="anonymous">
    <title>Translator</title>
</head>
<body>
    <div class="container">
        <h1>Translation service</h1>
        <div>Enter the text you wish to translate, choose the language, and click Translate!</div>
        <div>
            <form method="POST">
                <div class="form-group">
                    <textarea name="text" cols="20" rows="10" class="form-control"></textarea>
                </div>
                <div class="form-group">
                    <label for="language">Language:</label>
                    <select name="language" class="form-control">
                        <option value="en">English</option>
                        <option value="it">Italian</option>
                        <option value="ja">Japanese</option>
                        <option value="ru">Russian</option>
                        <option value="de">German</option>
                    </select>
                </div>
                <div>
                    <button type="submit" class="btn btn-success">Translate!</button>
                </div>
            </form>
        </div>
    </div>
</body>
</html>
```
The core components in the HTML above are the textarea for the text the user wishes to translate, and the dropdown list (select), which the user will use to indicate the target language.<br>
6. Test the application<br>
* on your terminal, run the following:<br>
```
# Windows
set FLASK_ENV=development

# Linux/macOS
export FLASK_ENV=development
```
* Run the application!
```
flask run
```
* copy the URL and paste to your browser. You should see the form displayed! Congratulations!

## PART 3
1. Browse to the [Azure portal](https://azure.microsoft.com/en-us/features/azure-portal/)<br>
2. Select Create a resource<br>
3. In the Search box, enter Translator<br>
4. Select Translator<br>
5. Select Create<br>
6. Complete the Create Translator form with the following values:<br>

* Subscription: Your subscription
* Resource group:
* Select Create new
* Name: flask-ai
* Resource group region: Select a region near you
* Resource region: Select the same region as above
* Name: A unique value, such as ai-yourname
* Pricing tier: Free F0
7. Select Review + create<br>
8. Select Create<br>
9. Select Go to resource<br>
10. Select Keys and Endpoint on the left side under RESOURCE MANAGEMENT <br>
11. Next to KEY 1, select Copy to clipboard<br>
