# webapp_translator_Azure
In this module, you'll build a website using Flask and Cognitive Services to translate text.<br>

* Learn how to set up a Flask development environment<br>
* Learn how to use Flask to build a form<br>
* Learn how to use the Translator service to translate text<br>

## Prerequisites
An Azure account. Learn how to create a free account in Create an [Azure account for students](https://azure.microsoft.com/en-us/free/students/?WT.mc_id=academic-0000-cxa)
<br>[Python3.6](https://www.python.org/downloads/) or later and VS code installed on your computer. <br> At the top of the article, choose the instructions for your configuration: Windows, Linux, or macOS.<br>
[Visual Studio Code](https://code.visualstudio.com/)<br>

## Implementation
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
4.Install Flask and librariesL<br>
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

