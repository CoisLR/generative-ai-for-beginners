# Code Summary

## aoai-app-recipe.py

**File: recipe_generator.py**

- **Description**: This script uses the Azure OpenAI service to generate recipes based on user-provided ingredients and dietary filters. It then generates a shopping list based on the generated recipes and the ingredients the user already has.
- **High-Level Overview**:
  - **External Libraries**:
    - `openai`: Used to interact with the Azure OpenAI service.
    - `os`: Used to access environment variables.
    - `dotenv`: Used to load environment variables from a `.env` file.
  - **Control Flow**:
    1.  Loads environment variables from a `.env` file.
    2.  Configures the Azure OpenAI client using environment variables for the endpoint, API key, and API version.
    3.  Prompts the user for the number of recipes, a list of ingredients, and a dietary filter.
    4.  Constructs a prompt for the OpenAI service using the user's input.
    5.  Calls the OpenAI service to generate recipes based on the prompt.
    6.  Prints the generated recipes.
    7.  Constructs a second prompt to generate a shopping list based on the generated recipes and the ingredients the user already has.
    8.  Calls the OpenAI service to generate a shopping list based on the prompt.
    9.  Prints the generated shopping list.
  - **Functions**:
    - No functions are explicitly defined; the script uses procedural logic.
  - **Error Handling**:
    - The script relies on the `openai` library to handle errors related to the OpenAI service. There is no explicit error handling for invalid user input or issues with the OpenAI service.
  - **File System Operations**:
    - Reads environment variables from a `.env` file using `dotenv`.
  - **User Instructions**:
    1.  Ensure you have a `.env` file in the same directory as the script, containing the following variables:
        -   `AZURE_OPENAI_ENDPOINT`: The endpoint for your Azure OpenAI service.
        -   `AZURE_OPENAI_API_KEY`: Your Azure OpenAI API key.
        -   `AZURE_OPENAI_DEPLOYMENT`: The deployment name for your OpenAI model.
    2.  Run the script.
    3.  The script will prompt you for:
        -   The number of recipes to generate.
        -   A comma-separated list of ingredients you have.
        -   A dietary filter (e.g., vegetarian, vegan, gluten-free).
    4.  The script will then print the generated recipes and a shopping list.

## aoai-app.py

**File: azure_openai_example.py**

- **Description**: This script demonstrates how to use the Azure OpenAI service to generate text completions based on a given prompt. It initializes the Azure OpenAI client using environment variables for authentication and endpoint configuration, then sends a prompt to the service and prints the generated completion.
- **High-Level Overview**:
    - **External Libraries**:
        - `openai`: Used to interact with the Azure OpenAI service.
        - `os`: Used to access environment variables.
        - `dotenv`: Used to load environment variables from a `.env` file.
    - **Control Flow**:
        1. Loads environment variables from the `.env` file using `load_dotenv()`.
        2. Initializes the `AzureOpenAI` client with the endpoint, API key, and API version obtained from environment variables.
        3. Defines a prompt for text completion.
        4. Creates a message structure with the prompt.
        5. Calls the `client.chat.completions.create()` method to get a text completion from the Azure OpenAI service, specifying the deployment name (model).
        6. Prints the content of the generated completion from the response.
    - **Functions**:
        - No custom functions are defined in this script. It relies on the `AzureOpenAI` class and its methods.
    - **Error Handling**:
        - The script does not explicitly include error handling. However, exceptions from the `openai` library might be raised if there are issues with authentication, the network, or the Azure OpenAI service itself.
    - **File System Operations**:
        - Reads environment variables from a `.env` file (if it exists) using `load_dotenv()`.
    - **User Instructions**:
        1. Ensure you have the `openai`, `python-dotenv` packages installed (`pip install openai python-dotenv`).
        2. Create a `.env` file in the same directory as the script.
        3. Add the following environment variables to the `.env` file:
            - `AZURE_OPENAI_ENDPOINT`: Your Azure OpenAI endpoint URL.
            - `AZURE_OPENAI_API_KEY`: Your Azure OpenAI API key.
            - `AZURE_OPENAI_DEPLOYMENT`: The name of your deployed model.
        4. Run the script. The generated text completion will be printed to the console.

## aoai-history-bot.py

**File: main.py**

- **Description**: This script leverages the Azure OpenAI service to simulate conversations with historical characters. It takes user input for the desired historical persona and a question, then formulates a prompt for the OpenAI model to respond as that character.
- **High-Level Overview**:
    - **External Libraries**:
        - `openai`: Used to interact with the Azure OpenAI service.
        - `os`: Used to access environment variables.
        - `dotenv`: Used to load environment variables from a `.env` file.
    - **Control Flow**:
        1.  Loads environment variables from the `.env` file.
        2.  Configures the Azure OpenAI client using the loaded environment variables (endpoint, API key, and API version).
        3.  Takes user input for the historical persona and the question to ask.
        4.  Constructs a prompt that instructs the OpenAI model to act as the specified historical character and answer the given question, emphasizing the importance of factual accuracy and avoiding fabrication.
        5.  Sends the prompt to the Azure OpenAI service using the `client.chat.completions.create` method.
        6.  Prints the response received from the OpenAI model.
    - **Functions**:
        - No custom functions are defined in this script. It relies on the `AzureOpenAI` class from the `openai` library and built-in functions like `input()` and `print()`.
    - **Error Handling**:
        - The script does not explicitly include error handling. If environment variables are missing or invalid, or if there are issues with the OpenAI service, the script may crash.
    - **File System Operations**:
        - Reads environment variables from a `.env` file using `dotenv`.
    - **User Instructions**:
        1.  Ensure you have an Azure OpenAI service account and have deployed a model.
        2.  Create a `.env` file in the same directory as the script.
        3.  Add the following variables to the `.env` file, replacing the placeholders with your actual values:
            ```
            AZURE_OPENAI_ENDPOINT="your_azure_openai_endpoint"
            AZURE_OPENAI_API_KEY="your_azure_openai_api_key"
            AZURE_OPENAI_DEPLOYMENT="your_azure_openai_deployment_name"
            ```
        4.  Install the required Python packages: `pip install openai python-dotenv`.
        5.  Run the script. It will prompt you to enter the historical character you want to simulate and the question you want to ask.
        6.  The script will then print the response from the Azure OpenAI service, attempting to answer the question in the style of the specified historical character.

## aoai-study-buddy.py

**File: main.py**

- **Description**: This script uses the Azure OpenAI service to answer questions about the Python programming language. It takes a user's question as input, constructs a prompt that instructs the OpenAI model to act as a Python expert, and then prints the model's response, formatted with a concept, example code, and explanation.
- **High-Level Overview**:
  - **External Libraries**:
    - `openai`: Used to interact with the Azure OpenAI service.
    - `os`: Used to access environment variables.
    - `dotenv`: Used to load environment variables from a `.env` file.
  - **Control Flow**:
    1. Loads environment variables from a `.env` file.
    2. Configures the Azure OpenAI client using environment variables for the endpoint, API key, and API version.
    3. Takes user input as a question about Python.
    4. Constructs a prompt that instructs the OpenAI model to act as a Python expert and provide answers in a specific format (concept, example code, explanation).
    5. Sends the prompt to the Azure OpenAI service using the `client.chat.completions.create` method.
    6. Prints the content of the model's response.
  - **Functions**:
    - No custom functions are defined in this script. It relies on the `AzureOpenAI` class from the `openai` library and the `os` and `dotenv` libraries.
  - **Error Handling**:
    - The script assumes that the environment variables (`AZURE_OPENAI_ENDPOINT`, `AZURE_OPENAI_API_KEY`, `AZURE_OPENAI_DEPLOYMENT`) are correctly set. If these variables are missing or incorrect, the script will likely raise an exception when initializing the `AzureOpenAI` client or when calling the `client.chat.completions.create` method.  No explicit error handling is implemented.
  - **File System Operations**:
    - Reads environment variables from a `.env` file using `load_dotenv()`. The `.env` file should be in the same directory as the script.
  - **User Instructions**:
    1. Ensure you have an Azure OpenAI service account and have deployed a model.
    2. Create a `.env` file in the same directory as the script and add the following environment variables:
        - `AZURE_OPENAI_ENDPOINT`: The endpoint of your Azure OpenAI service.
        - `AZURE_OPENAI_API_KEY`: Your Azure OpenAI API key.
        - `AZURE_OPENAI_DEPLOYMENT`: The name of your deployed model.
    3. Install the required Python packages: `pip install openai python-dotenv`.
    4. Run the script. It will prompt you to enter a question about the Python language.
    5. The script will then print the response from the Azure OpenAI service.

## githubmodels-app.py

**File: recipe_generator.py**

- **Description**: This script uses the Azure AI Inference Chat Completions API to generate five recipes for a dish containing chicken, potatoes, and carrots. It formulates a prompt, sends it to the specified model ("gpt-4o"), and prints the generated recipes to the console.
- **High-Level Overview**:
    - **External Libraries**:
        - `os`: Used to retrieve the API token from environment variables.
        - `azure.ai.inference.ChatCompletionsClient`: Used to interact with the Azure AI Inference Chat Completions API.
        - `azure.ai.inference.models.SystemMessage`, `azure.ai.inference.models.UserMessage`: Used to structure the messages sent to the API.
        - `azure.core.credentials.AzureKeyCredential`: Used for authenticating with the Azure AI Inference service.
    - **Control Flow**:
        1.  Retrieves the API token from the environment variable `GITHUB_TOKEN`.
        2.  Initializes the `ChatCompletionsClient` with the endpoint and the API token.
        3.  Defines the prompt requesting five recipes for a dish with chicken, potatoes, and carrots.
        4.  Calls the `complete` method of the `ChatCompletionsClient` to get the response from the model.
        5.  Prints the content of the first choice in the response to the console.
    - **Functions**:
        - The script primarily uses the `complete` method of the `ChatCompletionsClient` to interact with the Azure AI Inference API.
    - **Error Handling**:
        - The script assumes that the `GITHUB_TOKEN` environment variable is set and contains a valid API token. If the environment variable is not set, the script will raise an error.
        - The script does not explicitly handle errors from the API call.
    - **File System Operations**:
        - The script does not perform any file system operations.
    - **User Instructions**:
        1.  Ensure that the `GITHUB_TOKEN` environment variable is set with a valid Azure AI Inference API token.
        2.  Run the script. The generated recipes will be printed to the console.

## oai-app-recipe.py

**File: recipe_generator.py**

- **Description**: This script uses the OpenAI API to generate recipes based on user-provided ingredients and dietary restrictions. It then generates a shopping list based on the generated recipes and the ingredients the user already has.
- **High-Level Overview**:
  - **External Libraries**:
    - `openai`: Used to interact with the OpenAI API for generating recipe suggestions and shopping lists.
    - `os`: Used to interact with the operating system (not explicitly used in the provided code, but often used with `dotenv`).
    - `dotenv`: Used to load environment variables from a `.env` file, which is useful for storing API keys and other sensitive information.
  - **Control Flow**:
    1.  Loads environment variables from the `.env` file.
    2.  Prompts the user for the number of recipes, a list of ingredients they have, and any dietary filters.
    3.  Constructs a prompt for the OpenAI API using the user's input.
    4.  Calls the OpenAI API to generate recipes based on the prompt.
    5.  Prints the generated recipes to the console.
    6.  Constructs a second prompt for the OpenAI API to generate a shopping list, considering the ingredients the user already has and the generated recipes.
    7.  Calls the OpenAI API again to generate the shopping list.
    8.  Prints the generated shopping list to the console.
  - **Functions**:
    - The script does not define any explicit functions but relies on the `openai` library to interact with the OpenAI API.
  - **Error Handling**:
    - The script does not include explicit error handling. If the OpenAI API call fails (e.g., due to network issues or invalid API key), the script will likely crash.
  - **File System Operations**:
    - The script reads environment variables from a `.env` file using the `dotenv` library.
  - **User Instructions**:
    1.  Ensure you have an OpenAI API key and it is stored in a `.env` file in the same directory as the script. The `.env` file should contain a line like `OPENAI_API_KEY=your_api_key`.
    2.  Install the required Python packages: `openai`, `python-dotenv`. You can install them using pip: `pip install openai python-dotenv`.
    3.  Run the script. It will prompt you for the number of recipes, a list of ingredients you have, and any dietary filters.
    4.  The script will then print the generated recipes and a shopping list to the console.

## oai-app.py

**File: main.py**

- **Description**: This script uses the OpenAI API to generate a completion for a given prompt. It loads API keys from a `.env` file, configures the OpenAI client, sends a prompt to the GPT-3.5-turbo model, and prints the generated completion to the console.
- **High-Level Overview**:
  - **External Libraries**:
    - `openai`: Used to interact with the OpenAI API.
    - `os`: Used to interact with the operating system (implicitly used by `dotenv`).
    - `dotenv`: Used to load environment variables from a `.env` file.
  - **Control Flow**:
    1. Loads environment variables from the `.env` file.
    2. Initializes the OpenAI client.
    3. Defines a prompt and structures it into a message format suitable for the OpenAI API.
    4. Calls the OpenAI API to generate a completion based on the prompt.
    5. Prints the content of the generated completion to the console.
  - **Functions**:
    - Relies on the `openai` library's `OpenAI()` and `client.chat.completions.create()` functions to interact with the OpenAI API.
  - **Error Handling**:
    - The script does not explicitly include error handling. Errors from the OpenAI API (e.g., authentication issues, rate limits) will propagate and may cause the script to terminate.
  - **File System Operations**:
    - Reads environment variables from a `.env` file using `dotenv`.
  - **User Instructions**:
    1. Ensure you have an OpenAI API key and it is stored in a `.env` file in the same directory as the script. The `.env` file should contain a line like `OPENAI_API_KEY=your_api_key`.
    2. Make sure you have the `openai` and `python-dotenv` packages installed. You can install them using pip: `pip install openai python-dotenv`.
    3. Run the script using `python main.py`. The generated completion will be printed to the console.

## oai-history-bot.py

**File: main.py**

- **Description**: This script uses the OpenAI API to simulate a conversation with a historical character. It takes user input for the character's persona and the question to ask, then formulates a prompt for the OpenAI model to generate a response as that character.
- **High-Level Overview**:
  - **External Libraries**:
    - `openai`: Used to interact with the OpenAI API.
    - `os`: Used to interact with the operating system (implicitly through `dotenv`).
    - `dotenv`: Used to load environment variables from a `.env` file.
  - **Control Flow**:
    1. Loads environment variables from the `.env` file.
    2. Initializes the OpenAI client.
    3. Prompts the user for the historical character persona.
    4. Prompts the user for the question to ask the character.
    5. Constructs a prompt string that includes instructions for the OpenAI model to act as the specified historical character and answer the given question based on factual information.
    6. Calls the OpenAI API to generate a response.
    7. Prints the generated response to the console.
  - **Functions**:
    - No custom functions are defined; the script relies on the OpenAI library.
  - **Error Handling**:
    - No explicit error handling is present in the code. Errors from the OpenAI API (e.g., invalid API key, rate limits) will propagate and may cause the script to crash.
  - **File System Operations**:
    - Reads environment variables from a `.env` file using `dotenv`.
  - **User Instructions**:
    1. Ensure you have an OpenAI API key and it is stored in a `.env` file.
    2. Run the script.
    3. Enter the name of the historical character when prompted.
    4. Enter the question you want to ask the character when prompted.
    5. The script will print the response generated by the OpenAI model.

## oai-study-buddy.py

**File: main.py**

- **Description**: This script uses the OpenAI API to answer questions about the Python programming language. It takes a user's question as input, formulates a prompt for the OpenAI API, sends the prompt to the API, and prints the API's response. The script is designed to act as a study buddy, providing explanations, code examples, and conceptual overviews.
- **High-Level Overview**:
    - **External Libraries**:
        - `openai`: Used to interact with the OpenAI API.
        - `os`: Used for interacting with the operating system (though not explicitly used in the visible code, it's often used with `dotenv`).
        - `dotenv`: Used to load environment variables from a `.env` file.
    - **Control Flow**:
        1.  Loads environment variables from a `.env` file using `load_dotenv()`.
        2.  Initializes the OpenAI client.
        3.  Takes user input as a question about Python.
        4.  Constructs a prompt that instructs the OpenAI model to act as a Python expert and provide answers in a specific format (Concept, Example code, Explanation).
        5.  Sends the prompt to the OpenAI API using `client.chat.completions.create()`.
        6.  Prints the content of the API's response.
    - **Functions**:
        - No functions are explicitly defined in the script, but it relies on the `openai` library's functions for interacting with the OpenAI API.
    - **Error Handling**:
        - The script does not include explicit error handling. If the OpenAI API call fails (e.g., due to network issues, invalid API key), the script will likely crash.
    - **File System Operations**:
        - The script reads environment variables from a `.env` file using `load_dotenv()`. This file is expected to be in the same directory as the script.
    - **User Instructions**:
        1.  Ensure you have an OpenAI API key and it is stored in a `.env` file in the same directory as the script. The `.env` file should contain a line like `OPENAI_API_KEY=your_api_key`.
        2.  Install the required Python packages: `openai`, `python-dotenv`. You can install them using pip: `pip install openai python-dotenv`.
        3.  Run the script. It will prompt you to enter a question about the Python language.
        4.  The script will send the question to the OpenAI API and print the response, which should include a concept explanation, example code, and an explanation of the example.

