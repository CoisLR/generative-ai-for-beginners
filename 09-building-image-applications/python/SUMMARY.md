# Code Summary

## aoai-app-variation.py

**File: image_variation.py**

- **Description**: This script uses the Azure OpenAI service to create a variation of an existing image. It reads an image from a specified directory, sends it to the Azure OpenAI API, retrieves the generated variation, saves it to a file, and displays both the original and the variation.
- **High-Level Overview**:
  - **External Libraries**:
    - `openai`: Used to interact with the Azure OpenAI service.
    - `os`: Used for interacting with the operating system, such as accessing environment variables and constructing file paths.
    - `requests`: Used to download the generated image from a URL.
    - `PIL (Pillow)`: Used for image processing tasks such as opening and displaying images.
    - `dotenv`: Used to load environment variables from a `.env` file.
    - `json`: Used to parse the JSON response from the OpenAI API.
  - **Control Flow**:
    1. **Setup**: Loads environment variables, initializes the Azure OpenAI client, and defines the image paths.
    2. **Image Loading and Display**: Opens and displays the original image.
    3. **Image Variation Generation**: Calls the Azure OpenAI API to create a variation of the image.
    4. **Image Download and Save**: Downloads the generated image from the URL provided in the API response and saves it to a file.
    5. **Variation Display**: Opens and displays the generated image variation.
    6. **Completion**: Prints a "completed!" message.
  - **Functions**:
    - `AzureOpenAI(...)`: Initializes the Azure OpenAI client with API key, API version, and endpoint from environment variables.
    - `client.images.create_variation(...)`: Sends the image to the Azure OpenAI service to generate a variation.
    - `requests.get(image_url)`: Downloads the image from the given URL.
    - `Image.open(image_path)`: Opens an image file using the Pillow library.
    - `image.show()`: Displays the image using the default image viewer.
  - **Error Handling**:
    - A `try...finally` block is used to ensure that the "completed!" message is always printed, even if an error occurs.
    - There is a commented-out `except` block that was likely intended to catch `openai.InvalidRequestError` exceptions, but it is currently inactive.
  - **File System Operations**:
    - `os.path.join(...)`: Constructs file paths for the original and generated images.
    - `Image.open(image_path)`: Opens the original image from the file system.
    - `open(image_path, "rb")`: Opens the original image in binary read mode for sending to the OpenAI API.
    - `open(image_path, "wb")`: Opens a file in binary write mode to save the downloaded image.
  - **User Instructions**:
    1. Ensure that you have the `openai`, `requests`, `Pillow`, and `python-dotenv` packages installed (`pip install openai requests Pillow python-dotenv`).
    2. Set the `AZURE_OPENAI_API_KEY`, `AZURE_OPENAI_ENDPOINT`, and `AZURE_OPENAI_DEPLOYMENT` environment variables.  These can be set in a `.env` file in the same directory as the script.
    3. Place the original image named `generated-image.png` in the `images` subdirectory.
    4. Run the script. The script will display the original image, generate a variation using the Azure OpenAI service, save the variation as `generated_variation.png` in the `images` subdirectory, and display the generated variation.

## aoai-app.py

**File: generate_image.py**

- **Description**: This script uses the Azure OpenAI service to generate an image based on a text prompt using the DALL-E model. It then saves the generated image to a local directory and displays it.
- **High-Level Overview**:
  - **External Libraries**:
    - `openai`: Used to interact with the Azure OpenAI service.
    - `os`: Used for interacting with the operating system, such as accessing environment variables and creating directories.
    - `requests`: Used to download the generated image from the URL provided by the OpenAI service.
    - `PIL (Pillow)`: Used to open and display the downloaded image.
    - `dotenv`: Used to load environment variables from a `.env` file.
    - `json`: Used to parse the JSON response from the OpenAI service.
  - **Control Flow**:
    1.  Loads environment variables from a `.env` file using `dotenv.load_dotenv()`.
    2.  Initializes the Azure OpenAI client with the API key, API version, and endpoint from environment variables.
    3.  Calls the `client.images.generate` method to generate an image based on the provided prompt, size, and number of images.
    4.  Parses the JSON response to extract the image URL.
    5.  Downloads the image from the URL using the `requests` library.
    6.  Saves the downloaded image to a file in the `images` directory.
    7.  Opens and displays the image using the `PIL` library.
    8.  Prints "completed!" to the console in the `finally` block, ensuring it always executes.
  - **Functions**:
    - No custom functions are defined in this script. It primarily uses methods from the imported libraries.
  - **Error Handling**:
    - A `try...finally` block is used to ensure that "completed!" is always printed, even if an error occurs.
    - The commented-out `except` block shows an example of how to catch `InvalidRequestError` from the OpenAI client, but it's currently not active.
  - **File System Operations**:
    - Creates a directory named `images` in the current working directory if it doesn't already exist using `os.mkdir()`.
    - Saves the generated image to a file named `generated-image.png` inside the `images` directory using `open()` and `image_file.write()`.
  - **User Instructions**:
    1.  Ensure you have the required libraries installed (`openai`, `requests`, `Pillow`, `python-dotenv`).
    2.  Set the `AZURE_OPENAI_API_KEY`, `AZURE_OPENAI_ENDPOINT`, and `AZURE_OPENAI_DEPLOYMENT` environment variables with your Azure OpenAI credentials and deployment name.  These can be set in a `.env` file in the same directory as the script.
    3.  Run the script. The generated image will be saved in the `images` directory and displayed using your default image viewer.

## aoai-solution.py

**File: image_generation.py**

- **Description**: This script uses the Azure OpenAI service to generate an image based on a provided prompt. It configures the OpenAI client, sets up a prompt tailored for generating children-friendly images, calls the image generation API, retrieves the generated image, saves it to a local directory, and displays it. The script also includes commented-out code for creating variations of an existing image.
- **High-Level Overview**:
  - **External Libraries**:
    - `openai`: Used to interact with the Azure OpenAI service.
    - `os`: Used for interacting with the operating system, such as accessing environment variables and creating directories.
    - `requests`: Used to download the generated image from the URL provided by the OpenAI service.
    - `PIL (Pillow)`: Used to open and display the downloaded image.
    - `dotenv`: Used to load environment variables from a `.env` file.
    - `json`: Used to parse the JSON response from the OpenAI service.
  - **Control Flow**:
    1. **Setup**: Loads environment variables, initializes the Azure OpenAI client with API key, endpoint, and API version.
    2. **Prompt Definition**: Defines a meta-prompt to guide the image generation towards children-friendly content and combines it with a specific image request.
    3. **Image Generation**: Calls the `client.images.generate` method to generate an image based on the prompt.
    4. **Image Retrieval and Storage**: Extracts the image URL from the response, downloads the image using `requests`, and saves it to a local file.
    5. **Image Display**: Opens the saved image using PIL and displays it.
    6. **Error Handling**: Includes a `try...finally` block to ensure that a "completed!" message is always printed, but the `except` block is commented out.
    7. **Variation Generation (Commented Out)**: Contains commented-out code that would generate a variation of the generated image, save it, and display it.
  - **Functions**:
    - No explicit functions are defined; the script operates at the top level.
  - **Error Handling**:
    - The script includes a `try...finally` block. The `try` block attempts to generate, retrieve, save, and display the image. The `finally` block ensures that "completed!" is printed regardless of whether an exception occurred. The `except` block is commented out, so errors during image generation will not be caught and handled, potentially causing the script to terminate abruptly.
  - **File System Operations**:
    - Creates a directory named `images` in the current working directory if it doesn't exist.
    - Saves the generated image as `ch9-sol-generated-image.png` inside the `images` directory.
  - **User Instructions**:
    - Ensure that the `.env` file exists in the same directory as the script and contains the following variables:
      - `AZURE_OPENAI_API_KEY`: Your Azure OpenAI API key.
      - `AZURE_OPENAI_ENDPOINT`: Your Azure OpenAI endpoint.
      - `AZURE_OPENAI_DEPLOYMENT`: The deployment name for the DALL-E model.
    - Install the required Python packages using `pip install openai requests Pillow python-dotenv`.
    - Run the script using `python image_generation.py`.
    - The generated image will be saved in the `images` directory and displayed on the screen.

## oai-app-variation.py

**File: image_variation.py**

- **Description**: This script uses the OpenAI API to create a variation of an existing image and saves the generated variation to a file. It then displays the generated image using the default image viewer.
- **High-Level Overview**:
  - **External Libraries**:
    - `openai`: Used to interact with the OpenAI API for image generation.
    - `os`: Used for file path manipulation.
    - `requests`: Used to download the generated image from a URL.
    - `PIL (Pillow)`: Used to open and display the downloaded image.
    - `dotenv`: Used to load environment variables from a `.env` file.
  - **Control Flow**:
    1. Loads environment variables from a `.env` file.
    2. Initializes the OpenAI client.
    3. Defines the directory where images will be stored.
    4. Attempts to create a variation of an image named "generated-image.png" using the OpenAI API.
    5. Downloads the generated image from the URL provided in the API response.
    6. Saves the downloaded image as "generated_variation.png" in the specified directory.
    7. Opens and displays the saved image using the default image viewer.
    8. Catches and prints any `openai.InvalidRequestError` exceptions that occur during the image variation creation process.
  - **Functions**:
    - None explicitly defined, but the script uses the `openai.images.create_variation` method to generate the image variation.
  - **Error Handling**:
    - Includes a `try...except` block to catch `openai.InvalidRequestError` exceptions, which may occur if the image or request is invalid.
  - **File System Operations**:
    - Creates a path to the image directory using `os.path.join` and `os.curdir`.
    - Saves the downloaded image to a file using `open` with the `"wb"` (write binary) mode.
    - Opens the saved image using `PIL.Image.open`.
  - **User Instructions**:
    - Ensure that you have an OpenAI API key set as an environment variable.
    - Make sure that an image named "generated-image.png" exists in the current directory before running the script.
    - The generated image variation will be saved as "generated_variation.png" in the "images" subdirectory.

## oai-app.py

**File: image_generation.py**

- **Description**: This script uses the OpenAI API to generate an image based on a text prompt using the DALL-E 3 model. It then saves the generated image to a local directory and displays it. Additionally, it creates a variation of the generated image using the OpenAI API.
- **High-Level Overview**:
  - **External Libraries**:
    - `openai`: Used to interact with the OpenAI API for image generation and variation creation.
    - `os`: Used for interacting with the operating system, specifically for creating directories and joining paths.
    - `requests`: Used to download the generated image from the URL provided by the OpenAI API.
    - `PIL (Pillow)`: Used to open and display the downloaded image.
    - `dotenv`: Used to load environment variables from a `.env` file.
  - **Control Flow**:
    1.  Loads environment variables from a `.env` file using `dotenv.load_dotenv()`.
    2.  Initializes the OpenAI client.
    3.  Calls the OpenAI image generation API (`client.images.generate`) with a specified prompt, size, and number of images.
    4.  Creates an `images` directory if it doesn't exist.
    5.  Extracts the image URL from the API response.
    6.  Downloads the image from the URL using the `requests` library.
    7.  Saves the downloaded image to the `images` directory as `generated-image.png`.
    8.  Opens and displays the saved image using the `PIL` library.
    9.  Creates a variation of the generated image using the OpenAI API (`client.images.create_variation`).
  - **Functions**:
    - No custom functions are defined in this script. It relies on the functions provided by the imported libraries.
  - **Error Handling**:
    - Includes a `try...except` block to catch `openai.InvalidRequestError` exceptions that may occur during the API call. If an error occurs, the error message is printed to the console.
  - **File System Operations**:
    - Creates a directory named `images` in the current working directory if it doesn't exist using `os.mkdir()`.
    - Saves the generated image to the `images` directory as `generated-image.png` using file I/O operations.
    - Opens the saved image using `PIL.Image.open()` for display.
  - **User Instructions**:
    - Ensure that you have an OpenAI API key set as an environment variable.
    - The script saves the generated image in the `images` directory in the current working directory.
    - The script displays the generated image using the default image viewer.
    - The prompt can be modified directly in the `prompt` parameter of the `client.images.generate` function.

