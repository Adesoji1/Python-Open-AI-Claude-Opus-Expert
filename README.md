# Python-Open-AI-Claude-Opus-Expert
I want to automate these 4 prompts instead of me doing it manually so i want a pyhton script that can do it for me
### Option 1

To integrate OpenAI's GPT (like ChatGPT) or Claude Opus into a Python script to automate document analysis and content adaptation, you would typically use the OpenAI Python API. This allows for more sophisticated natural language understanding and generation, tailored to tasks such as extracting insights, summarizing content, or even generating new text based on a specific structure.

Below is a step-by-step guide and example script to set this up using the OpenAI API:

### Step 1: Install OpenAI Python Package
You need to install the OpenAI Python client library. You can do this using pip:

```bash
pip install openai
```

### Step 2: Setup Your API Key
Ensure you have an API key from OpenAI. This key is required to authenticate your requests to the OpenAI servers. You should set this API key in your environment variables for security reasons.

```bash
export OPENAI_API_KEY='your-api-key-here'
```

### Step 3: Python Script Using OpenAI API
Here’s a Python script that integrates the OpenAI API to perform the tasks you outlined:

```python
import openai
import os

# Load your API key from an environment variable
api_key = os.getenv('OPENAI_API_KEY')
openai.api_key = api_key

def analyze_document(text):
    """ Analyze the document to extract the desired insights using OpenAI's API """
    response = openai.Completion.create(
        engine="text-davinci-002",  # or use "code-davinci-002" for Claude Opus
        prompt=f"Analyze this document and provide insights: {text}",
        max_tokens=150
    )
    return response.choices[0].text.strip()

def generate_adapted_content(template, context):
    """ Generate content based on a template and specific insights """
    adapted_prompt = f"Rewrite the following script to focus on {context['new_topic']}, following the structure but changing relevant details: {template}"
    response = openai.Completion.create(
        engine="text-davinci-002",  # or use "code-davinci-002" for Claude Opus
        prompt=adapted_prompt,
        max_tokens=500
    )
    return response.choices[0].text.strip()

def main():
    source_text = "Here is your source text from the first document."
    template_text = "75% of the population are slowly destroying their neck every single day."

    # Analyze the document
    insights = analyze_document(source_text)
    print("Extracted Insights:", insights)

    # Adapt content using the template
    new_context = {"new_topic": "foot neuropathy"}
    adapted_content = generate_adapted_content(template_text, new_context)
    print("Adapted Content:\n", adapted_content)

if __name__ == "__main__":
    main()
```

### Explanation:
- **analyze_document**: This function sends the text to the OpenAI API and asks it to analyze the document, extracting relevant insights.
- **generate_adapted_content**: This function uses the OpenAI API to adapt a given template into new content based on the insights and the specific context.

### Note:
- Replace `"text-davinci-002"` with `"code-davinci-002"` or another suitable model if you are using Claude Opus specifically for more advanced or technical text transformations.
- You might need to adjust the `prompt` structure and `max_tokens` according to your specific requirements to ensure the generated text meets your expectations.
- Always handle API keys securely and avoid hard-coding them into your scripts.

This script demonstrates basic usage and will need to be adapted and thoroughly tested with actual document contents to ensure it performs as expected for your specific use cases.

### OPtion 2

To automate the four prompts you mentioned using OpenAI's GPT or Claude Opus API, we'll create a Python script. This script will read input from files, interact with the OpenAI API to process and transform the text, and produce outputs that align with each prompt. I'll structure the Python script to handle these tasks efficiently.

### Step-by-Step Python Script Using OpenAI API

First, make sure you have installed the OpenAI Python package and set up your API key correctly as shown in the previous examples.

Here's a Python script structure that fits your requirements:

```python
import openai
import os

# Configure the API Key
api_key = os.getenv('OPENAI_API_KEY')
openai.api_key = api_key

def read_file(file_path):
    """ Helper function to read a file """
    with open(file_path, 'r') as file:
        return file.read()

def ask_openai(prompt, max_tokens=500):
    """ General function to query OpenAI API """
    response = openai.Completion.create(
        engine="text-davinci-002",
        prompt=prompt,
        max_tokens=max_tokens,
        temperature=0.5
    )
    return response.choices[0].text.strip()

def main():
    # Paths to your source and template documents
    source_file_path = 'source_document.txt'
    template_file_path = 'template_document.txt'

    # Read the source and template files
    source_text = read_file(source_file_path)
    template_text = read_file(template_file_path)

    # Prompt 1: Analyze the document
    analysis = ask_openai(f"Analyze this please: {source_text}")

    # Prompt 2: Extract specific insights
    pain_points = ask_openai(f"What are the pain points in this text: {source_text}")
    core_problem = ask_openai(f"What is the core problem in this text: {source_text}")
    symptoms = ask_openai(f"What are the symptoms in this text: {source_text}")
    emotions = ask_openai(f"What do they feel emotionally about the problem in this text: {source_text}")
    situation = ask_openai(f"Describe a situation of how they experience their problem based on this text: {source_text}")

    # Prompt 3: Identify unique mechanism problem and solution
    mechanism = ask_openai(f"Based on the script, identify the unique mechanism problem and solution in this text: {source_text}")

    # Prompt 4: Adapt the content of the new script
    adapted_content = ask_openai(
        f"Adapt the following script to focus on neuropathy using insights from another analysis. Keep the structure but change relevant details. Here is the script: {template_text}",
        max_tokens=1000  # May need more tokens for a longer adaptation
    )

    # Print the results
    print("Analysis:", analysis)
    print("Pain Points:", pain_points)
    print("Core Problem:", core_problem)
    print("Symptoms:", symptoms)
    print("Emotional Feelings:", emotions)
    print("Situation:", situation)
    print("Unique Mechanism:", mechanism)
    print("Adapted Content:\n", adapted_content)

if __name__ == "__main__":
    main()
```

### Explanation:
1. **read_file:** Helper function to read content from a file.
2. **ask_openai:** Generalized function to send prompts to the OpenAI API and retrieve responses.
3. **main:** Orchestrates reading from files, generating queries based on prompts, and printing results.

### Note:
- This script requires actual file paths to `source_document.txt` and `template_document.txt` to function. You will need to replace placeholders with actual paths or filenames.
- Ensure you manage API key security appropriately, preferably using environment variables.
- Adjust `max_tokens` as necessary based on the complexity and length of responses needed.

This setup should effectively automate the analysis and content adaptation processes you described, leveraging OpenAI's API to handle complex text transformations.

### Option 3

Well this is less efficient


To create a Python script that automates the tasks you've described, we'll set up a script that can:
1. Analyze a document to extract specific insights.
2. Extract pain points, core problems, symptoms, emotional impacts, and situational experiences from a text based on predefined prompts.
3. Identify a unique mechanism problem and solution from a text.
4. Use a template structure from one document to reframe another document's content to fit a specific context (in this case, neuropathy) while maintaining the structure of the original.

Here's an example Python script structure that addresses these requirements:

```python
import os
from textblob import TextBlob

# Function to analyze a document and extract basic insights
def analyze_document(file_path):
    try:
        with open(file_path, 'r') as file:
            text = file.read()
            blob = TextBlob(text)
            return blob.sentences  # Using TextBlob for simple NLP tasks
    except Exception as e:
        return str(e)

# Function to extract specific insights from the text
def extract_insights(sentences):
    pain_points = []
    core_problem = []
    symptoms = []
    emotions = []
    situations = []

    # Dummy functions to simulate insight extraction
    for sentence in sentences:
        if "pain point" in sentence.lower():
            pain_points.append(sentence)
        if "core problem" in sentence.lower():
            core_problem.append(sentence)
        if "symptom" in sentence.lower():
            symptoms.append(sentence)
        if "emotion" in sentence.lower():
            emotions.append(sentence)
        if "situation" in sentence.lower():
            situations.append(sentence)

    return pain_points, core_problem, symptoms, emotions, situations

# Function to adapt content based on a template
def adapt_content(template_path, insights, new_context):
    try:
        with open(template_path, 'r') as file:
            template_text = file.read()
            # Simulate content adaptation
            adapted_text = template_text.replace("neck", new_context["body_part"])
            # Further replacements based on insights and other context could be added here
            return adapted_text
    except Exception as e:
        return str(e)

def main():
    # Example file paths
    source_file = 'source_document.txt'
    template_file = 'template_document.txt'
    new_context = {
        "body_part": "feet"
    }

    # Analyze the document
    sentences = analyze_document(source_file)
    if isinstance(sentences, list):
        insights = extract_insights(sentences)
        adapted_content = adapt_content(template_file, insights, new_context)
        print("Adapted Content:\n", adapted_content)
    else:
        print("Error:", sentences)

if __name__ == "__main__":
    main()
```

### Explanation:
1. **analyze_document:** Opens a document and performs basic natural language processing to split the text into sentences.
2. **extract_insights:** Extracts specific details such as pain points and symptoms from the sentences based on keywords.
3. **adapt_content:** Uses a template to adapt the text of one document based on another's structure, simulating how you'd rewrite content.

### Note:
- The `TextBlob` library is used here for simple NLP tasks. For more complex analysis, consider using libraries like `spaCy` or `nltk`.
- This script is a basic framework and needs actual logic for processing based on your specific criteria and input structure.
- Python libraries like `TextBlob` may require installation via pip (`pip install textblob`).

This script should be adapted further based on your exact data and the complexity of the text processing required.
