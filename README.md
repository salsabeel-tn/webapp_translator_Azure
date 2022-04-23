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
* In your terminal, run the following:<br>
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
### Create .env file to store the key
1. Return to Visual Studio Code and create a new file in the root of the application by selecting New file and naming it .env (has to be named .env)<br>
2. Paste the following text into .env <br>
```
KEY=your_key
ENDPOINT=your_endpoint
LOCATION=your_location
```
3. Replace the placeholders

* your_key with the key you copied above
* your_endpoint with the endpoint from Azure
* your_location with the location from Azure

## PART 4 
1. At the very top of app.py, add the following lines of code:<br>
```
import requests, os, uuid, json
from dotenv import load_dotenv
load_dotenv()
```
The top line will import libraries that we'll use later, when making the call to the Translator service. We also import load_dotenv from dotenv and execute the function, which will load the values from .env.<br>

2. At the bottom of app.py, add the following lines of code to create the route and logic for translating text:<br>
```
@app.route('/', methods=['POST'])
def index_post():
    # Read the values from the form
    original_text = request.form['text']
    target_language = request.form['language']

    # Load the values from .env
    key = os.environ['KEY']
    endpoint = os.environ['ENDPOINT']
    location = os.environ['LOCATION']

    # Indicate that we want to translate and the API version (3.0) and the target language
    path = '/translate?api-version=3.0'
    # Add the target language parameter
    target_language_parameter = '&to=' + target_language
    # Create the full URL
    constructed_url = endpoint + path + target_language_parameter

    # Set up the header information, which includes our subscription key
    headers = {
        'Ocp-Apim-Subscription-Key': key,
        'Ocp-Apim-Subscription-Region': location,
        'Content-type': 'application/json',
        'X-ClientTraceId': str(uuid.uuid4())
    }

    # Create the body of the request with the text to be translated
    body = [{ 'text': original_text }]

    # Make the call using post
    translator_request = requests.post(constructed_url, headers=headers, json=body)
    # Retrieve the JSON response
    translator_response = translator_request.json()
    # Retrieve the translation
    translated_text = translator_response[0]['translations'][0]['text']

    # Call render template, passing the translated text,
    # original text, and target language to the template
    return render_template(
        'results.html',
        translated_text=translated_text,
        original_text=original_text,
        target_language=target_language
    )
```
    
The code is commented to describe the steps that are being taken. At a high level, here's what our code does:<br>

1. Reads the text the user entered and the language they selected on the form<br>
2. Reads the environmental variables we created earlier from our .env file<br>
3. Creates the necessary path to call the Translator service, which includes the target language (the source language is automatically detected)<br>
4. Creates the header information, which includes the key for the Translator service, the location of the service, and an arbitrary ID for the translation<br>
5. Creates the body of the request, which includes the text we want to translate<br>
6. Calls post on requests to call the Translator service<br>
7. Retrieves the JSON response from the server, which includes the translated text<br>
8. Retrieves the translated text (see the following note)<br>
9. Calls render_template to display the response page<br>

The JSON file returned from the Translation service looks like:<br>
```[
  {
    "detectedLanguage": {
      "language": "en",
      "score": 1.0
    },
    "translations": [
      {
        "text": "これはテストです",
        "to": "ja"
      }
    ]
  }
]
```
Specifically, we need to read the first result, then to the collection of translations, the first translation, and then to the text. This is done by the call: translator_response[0]['translations'][0]['text']<br>

### Create the template to display results
Let's create the HTML template for the results page.

<br>
1. Create a new file in templates by selecting templates in the Explorer tool in Visual Studio Code. Then select New file<br>
2. Name the file results.html<br>
3. Add the following HTML to results.html<br>
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/css/bootstrap.min.css"
        integrity="sha384-TX8t27EcRE3e/ihU7zmQxVncDAy5uIKz4rEkgIXeMed4M0jlfIDPvg6uqKI2xXr2" crossorigin="anonymous">
    <title>Result</title>
</head>
<body>
    <div class="container">
        <h2>Results</h2>
        <div>
            <strong>Original text:</strong> {{ original_text }}
        </div>
        <div>
            <strong>Translated text:</strong> {{ translated_text }}
        </div>
        <div>
            <strong>Target language code:</strong> {{ target_language }}
        </div>
        <div>
            <button type="submit" class="btn btn-success" ><a href="{{ url_for('index') }}" style="color:white">Try another one!</a></button>
        </div>
    </div>
</body>
</html>
```

### Test the page
1. Use Ctrl-C to stop the Flask application<br>
2. Execute the command ```flask run``` to restart the service<br>
3. browse to the URL<br>
4. Translate some text!<br>

That's All!



