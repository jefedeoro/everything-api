# everything-api

Flask OpenAI GPT-4 API
This is a Flask application that utilizes the OpenAI GPT-4 API to generate dynamic content based on the requested URL path. The app can handle both GET and POST requests and generates responses in various content types such as HTML, JSON, or plain text.

Requirements
Python 3.8+
Flask
OpenAI Python library
Installation
Clone this repository:

bash
```
git clone https://github.com/yourusername/flask-openai-gpt4.git
cd flask-openai-gpt4
Create a virtual environment and activate it:

bash
```
python3 -m venv .venv
source .venv/bin/activate
```
For Windows, use:

```
python -m venv .venv
.venv\Scripts\activate
```
Install the required packages:

```
pip install -r requirements.txt
```
Add your OpenAI API key:

Create a file named openai.key in the root directory of the project.
Add your API key to the file as a single line without any extra characters or spaces.
Usage
Set the FLASK_APP environment variable:

arduino
```
export FLASK_APP=main
```
For Windows, use:

arduino
```
set FLASK_APP=main
```
Run the Flask application:

arduino
```
flask run
```
The application will start on http://localhost:5000. You can make GET or POST requests to the desired URL path to see the generated content.

Code Overview
The main code of the application can be found in the main.py file. The application uses the Flask framework to create a single route that catches all incoming requests. This route then processes the request, generates a prompt for the GPT-4 API based on the URL path and optional form data, and sends the prompt to the API to generate a response. The response's content type and data are then returned as the result of the request.

Here is an overview of the main code sections:

Import required libraries:

python
```
from flask import Flask, request
import os
import openai
import json

```
Read the OpenAI API key from the openai.key file:

python
```
with open('../openai.key') as f:
    openai.api_key = f.read().strip()
    ```
Initialize the Flask application:

python
```
app = Flask(__name__)
```
Define the base prompt for the GPT-4 API:

python
```
BASE_PROMPT = """Create a response document with content that matches the following URL path: 
    `{{URL_PATH}}`

The first line is the Content-Type of the response.
The following lines is the returned data.
In case of a html response, add relative href links with to related topics.
{{OPTIONAL_DATA}}

Content-Type:
"""
Create the catch-all route:

python
```
@app.route("/", methods=['POST', 'GET'])
@app.route("/<path:path>", methods=['POST', 'GET'])
def catch_all(path=""):
    # ... (processing and API call)
```
Process the request and generate the GPT-4 API prompt:

python
```
if request.form:
    prompt = BASE_PROMPT.replace("{{OPTIONAL_DATA}}", f"form data: {json.dumps(request.form)}")
else: prompt = BASE_PROMPT.replace("{{OPTIONAL_DATA}}", f"")

prompt = prompt.replace("{{URL_PATH}}", path)

sql
Copy code

7. Call the GPT-4 API with the generated prompt:

```python
response = openai.Completion.create(
    model="text-davinci-003",
    prompt=prompt,
    temperature=0.7,
    max_tokens=512,
    top_p=1,
    frequency_penalty=0,
    presence_penalty=0
)
```

Extract the content type and response data from the API's response:

python
```Copy code```
ai_data = response.choices[0].text

print(ai_data)

content_type = ai_data.splitlines()[0]
response_data = "\n".join(ai_data.splitlines()[1:])
```

Return the generated content as the result of the request:

python
```
return response_data, 200, {'Content-Type': content_type}```

## Testing
To test the application, you can use tools like curl or Postman to make GET or POST requests to different URL paths. For example:

bash
```
curl http://localhost:5000/some/path
```
Or, to make a POST request with form data:

bash
```
curl -X POST -d "key=value" http://localhost:5000/some/path
```

Alternatively, you can also use a web browser to navigate to various URL paths to see the generated content.
'http://localhost:5000/some/path'
