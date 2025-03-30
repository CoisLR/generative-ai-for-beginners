# Code Summary

## transcript_download.py

**File: download_youtube_transcripts.py**

- **Description**: This script downloads transcripts and metadata for all videos in a specified YouTube playlist. It uses the YouTube Data API to retrieve the list of videos in the playlist and the `youtube-transcript-api` library to fetch the transcripts. The script supports multithreading to speed up the download process.

- **High-Level Overview**:
    - **External Libraries**:
        - `os`: Used for interacting with the operating system, such as creating directories and accessing environment variables.
        - `json`: Used for reading and writing JSON files, which store video metadata and transcripts.
        - `logging`: Used for logging information, warnings, and errors during the script's execution.
        - `time`: Used for measuring the execution time of the script.
        - `threading`: Used for creating and managing multiple threads to download transcripts concurrently.
        - `argparse`: Used for parsing command-line arguments, such as the playlist ID and output folder.
        - `queue`: Used for creating a thread-safe queue to store video items to be processed.
        - `googleapiclient.discovery`: Used for interacting with the YouTube Data API.
        - `googleapiclient.errors`: Used for handling errors from the YouTube Data API.
        - `youtube_transcript_api`: Used for fetching YouTube video transcripts.
        - `youtube_transcript_api.formatters`: Used for formatting the transcript into different formats (WebVTT in this case, though it is not used).
    - **Control Flow**:
        1. **Argument Parsing**: The script starts by parsing command-line arguments for the transcript folder and playlist ID.
        2. **Initialization**: It initializes the YouTube Data API client and sets up logging.
        3. **Playlist Retrieval**: It retrieves the list of videos in the specified playlist using the YouTube Data API.  It handles pagination to retrieve all videos, even if the playlist has more than the maximum allowed results per page.
        4. **Queueing**: Each video item is added to a queue.
        5. **Multithreading**: Multiple threads are created to process the queue concurrently.
        6. **Transcript Download**: Each thread retrieves a video item from the queue, downloads the transcript (if it doesn't already exist), and saves it to a file.  It also generates and saves metadata for each video.
        7. **Completion**: The script waits for all threads to finish processing the queue and logs the total execution time.
    - **Functions**:
        - `gen_metadata(playlist_item)`: Generates metadata for a video (title, description, videoId) and saves it as a JSON file.
        - `get_transcript(playlist_item, counter_id)`: Downloads the transcript for a video using the `youtube-transcript-api` library. It saves the transcript as a JSON file.  It also handles cases where a transcript is not available for a video.
        - `process_queue()`: This function is executed by each thread. It retrieves video items from the queue and calls `get_transcript` and `gen_metadata` for each video.
    - **Error Handling**:
        - The script checks if the transcript folder and playlist ID are provided. If not, it logs an error and exits.
        - The `get_transcript` function includes a try-except block to handle exceptions that may occur during transcript download (e.g., if a transcript is not available for a video).
    - **File System Operations**:
        - The script creates a directory for storing the transcripts if it doesn't already exist.
        - It saves the transcripts and metadata as JSON files in the specified directory.
    - **User Instructions**:
        1.  Set the `GOOGLE_DEVELOPER_API_KEY` environment variable with your Google Developer API key.
        2.  Install the required Python packages: `pip install google-api-python-client youtube-transcript-api`
        3.  Run the script from the command line, providing the transcript folder and playlist ID as arguments:
            ```bash
            python download_youtube_transcripts.py -f <transcript_folder> -p <playlist_id>
            ```
            For example:
            ```bash
            python download_youtube_transcripts.py -f transcripts -p PLF0eU3VLK1nOkenk-JgtWw6VRjJjLems
            ```
        4.  The transcripts and metadata will be saved in the specified transcript folder.

## transcript_enrich_bucket.py

**File: master_transcription.py**

- **Description**: This script processes JSON metadata files and corresponding JSON VTT (subtitle) files to generate a master JSON file containing segmented transcriptions. It reads metadata from JSON files, extracts transcript segments from associated VTT files, and combines them based on a specified segment length and token limit, handling text cleaning and overlap to ensure smooth transitions between segments.

- **High-Level Overview**:
    - **External Libraries**:
        - `datetime`: Used for time manipulations, specifically to convert seconds into HH:MM:SS format.
        - `glob`: Used to find all the pathnames matching a specified pattern (used to find all JSON files in a directory).
        - `os`: Provides a way of using operating system dependent functionality (used for file path manipulation).
        - `json`: Used for encoding and decoding JSON data.
        - `argparse`: Used for parsing command-line arguments.
        - `tiktoken`: Used for tokenizing text, specifically for OpenAI models, to ensure segments stay within token limits.
        - `logging`: Used for logging information, warnings, and errors.
        - `rich.progress`: Used for displaying a progress bar during file processing.
    - **Control Flow**:
        1. **Argument Parsing**: The script starts by parsing command-line arguments for the transcript folder and segment length.
        2. **File Iteration**: It then iterates through all JSON files in the specified transcript folder using `glob`.
        3. **Data Extraction**: For each JSON file, it extracts metadata and uses the video ID to locate the corresponding VTT file.
        4. **VTT Parsing**: The VTT file is parsed, and its segments are combined into larger segments based on the specified segment length and token limit.
        5. **Segment Creation**: New segments are created, ensuring that they do not exceed the maximum token limit and that a small percentage of text overlaps between segments.
        6. **Output**: Finally, the generated segments are saved to a master JSON file.
    - **Functions**:
        - `gen_metadata_master(metadata)`: Cleans and prepares metadata text, adding a default description if none exists.
        - `clean_text(text)`: Removes unwanted characters and patterns from the text.
        - `append_text_to_previous_segment(text)`: Appends a percentage of the current segment's text to the previous segment to create a smooth transition.
        - `add_new_segment(metadata, text, segment_begin_seconds)`: Creates a new segment dictionary and appends it to the segments list.
        - `parse_json_vtt_transcript(vtt, metadata)`: Parses the JSON VTT file, segments the transcript based on time and token limits, and adds the segments to the segments list.
        - `get_transcript(metadata)`: Locates and processes the VTT file associated with the given metadata.
    - **Error Handling**:
        - The script checks if the transcript folder is provided and exits if it is not.
        - It also checks if the VTT file exists before attempting to parse it.
        - Logging is used to provide information about missing files or other issues.
    - **File System Operations**:
        - Reads JSON metadata files from the specified folder.
        - Reads JSON VTT files.
        - Writes the combined transcript segments to a master JSON file in the "output" subfolder of the transcript folder.
    - **User Instructions**:
        1.  Save the script to a `.py` file (e.g., `master_transcription.py`).
        2.  Ensure you have the required libraries installed (`datetime`, `glob`, `os`, `json`, `argparse`, `tiktoken`, `logging`, and `rich`).  Install any missing libraries using pip (e.g., `pip install tiktoken rich`).
        3.  Organize your transcript files:
            - Create a folder containing your JSON metadata files and JSON VTT files.  The VTT files should be named according to the `videoId` field in the corresponding JSON metadata file (e.g., `videoId.json.vtt`).
            - Create an `output` subfolder inside the transcript folder to store the `master_transcriptions.json` file.
        4.  Run the script from the command line, providing the path to the transcript folder using the `-f` or `--folder` argument:
        ```bash
        python master_transcription.py -f /path/to/transcripts
        ```
        5.  You can also specify the segment length in minutes using the `-m` or `--minutes` argument:
        ```bash
        python master_transcription.py -f /path/to/transcripts -m 10
        ```
        6.  To enable verbose logging, use the `--verbose` flag:
        ```bash
        python master_transcription.py -f /path/to/transcripts --verbose
        ```
        7.  The script will generate a `master_transcriptions.json` file in the `output` subfolder of the transcript folder, containing the combined transcript segments.

## transcript_enrich_embeddings.py

**File: enrich_embeddings.py**

- **Description**: This script enriches a JSON file containing text segments with embeddings generated using the OpenAI API. It reads the text from each segment, normalizes it, retrieves its embedding, and adds the embedding to the segment. The script uses multithreading to speed up the embedding generation process.

- **High-Level Overview**:
  - **External Libraries**:
    - `argparse`: For parsing command-line arguments.
    - `logging`: For logging information, warnings, and errors.
    - `re`: For regular expression operations (text normalization).
    - `os`: For interacting with the operating system (file paths, environment variables).
    - `json`: For reading and writing JSON files.
    - `threading`: For creating and managing multiple threads.
    - `queue`: For managing a queue of text segments to be processed by threads.
    - `time`: For adding delays and measuring time.
    - `dotenv`: For loading environment variables from a `.env` file.
    - `openai`: For interacting with the OpenAI API to generate embeddings.
    - `tiktoken`: For tokenizing text to check its length.
    - `tenacity`: For retrying API calls with exponential backoff.
    - `rich`: For displaying a progress bar in the console.

  - **Control Flow**:
    1. **Argument Parsing**: Parses command-line arguments for the input folder and verbosity.
    2. **Configuration**: Loads API keys and endpoint from environment variables, configures the OpenAI API.
    3. **Data Loading**: Reads the input JSON file containing text segments.
    4. **Queue Initialization**: Creates a queue and adds all segments to it.
    5. **Multithreading**: Creates multiple threads to process the queue concurrently.
    6. **Embedding Generation**: Each thread retrieves a segment from the queue, normalizes the text, generates an embedding using the OpenAI API, and adds the embedding to the segment.
    7. **Output**: Saves the enriched segments (with embeddings) to a new JSON file.

  - **Functions**:
    - `normalize_text(s, sep_token=" \n ")`: Normalizes text by removing extra spaces, newlines, and other unwanted characters.
    - `get_text_embedding(text: str)`: Retrieves the embedding for a given text using the OpenAI API. It retries the API call with exponential backoff if it fails.
    - `process_queue(progress, task)`: Processes segments from the queue, normalizes text, gets embeddings, and updates the progress bar.
    - `convert_time_to_seconds(value)`: Converts a time string in the format "HH:MM:SS" to seconds.

  - **Error Handling**:
    - Uses `try...except` blocks within the `get_text_embedding` function to handle potential errors from the OpenAI API.
    - Uses `tenacity` library to retry API calls with exponential backoff.
    - Logs errors and warnings using the `logging` module.
    - Checks if the transcript folder is provided and exits if not.
    - Skips segments that exceed the maximum token length.

  - **File System Operations**:
    - Reads the input JSON file from the specified folder.
    - Writes the output JSON file to the same folder.

  - **User Instructions**:
    1.  Set the `AZURE_OPENAI_API_KEY` and `AZURE_OPENAI_ENDPOINT` environment variables.
    2.  Run the script with the `-f` or `--folder` argument to specify the folder containing the input JSON file.
    3.  Use the `--verbose` flag for more detailed logging.

## transcript_enrich_lite.py

**File: remove_text_from_enriched_transcript.py**

- **Description**: This script processes an enriched transcript JSON file, removes the "text" and "description" keys from each segment, and saves the modified data to a new JSON file. This is useful for creating a lighter version of the transcript that excludes the actual text content, focusing on other metadata.
- **High-Level Overview**:
    - **External Libraries**:
        - `json`: Used for loading and saving JSON data.
        - `os`: Used for constructing file paths.
        - `argparse`: Used for parsing command-line arguments.
        - `logging`: Used for logging errors and warnings.
    - **Control Flow**:
        1.  Parses command-line arguments to get the transcript folder path.
        2.  Loads the enriched transcript from `master_enriched.json`.
        3.  Removes the "text" and "description" keys from each segment in the transcript.
        4.  Saves the modified transcript to `master_enriched_lite.json`.
    - **Functions**:
        - `remove_text(video_segments)`: Takes a list of video segments (dictionaries) as input and returns a new list where each segment has the "text" and "description" keys removed. It uses a list comprehension for efficient processing.
    - **Error Handling**:
        - Checks if the transcript folder is provided as a command-line argument. If not, it logs an error and exits.
    - **File System Operations**:
        - Reads the input JSON file (`master_enriched.json`) from the specified transcript folder.
        - Writes the modified JSON data to a new file (`master_enriched_lite.json`) in the same folder.
    - **User Instructions**:
        - The script requires the transcript folder to be specified using the `-f` or `--folder` command-line argument.
        - Example usage: `python remove_text_from_enriched_transcript.py -f /path/to/transcript/folder`

## transcript_enrich_speaker.py

```markdown
**File: speaker_update.py**

- **Description**: This script extracts speaker names from YouTube video metadata and the first minute of the video's transcript using OpenAI's function calling capabilities. It processes multiple transcript files in a specified folder, updates the metadata with the extracted speaker names, and saves the modified metadata back to the files.

- **High-Level Overview**:
  - **External Libraries**:
    - `json`: For reading and writing JSON files (transcript and metadata).
    - `os`: For interacting with the operating system (e.g., file paths, environment variables).
    - `glob`: For finding files matching a specified pattern.
    - `threading`: For parallel processing of transcript files.
    - `logging`: For logging errors and debugging information.
    - `queue`: For managing a queue of files to be processed by multiple threads.
    - `time`: For measuring execution time and adding delays.
    - `argparse`: For parsing command-line arguments.
    - `dotenv`: For loading environment variables from a `.env` file.
    - `openai`: For interacting with the OpenAI API.
    - `openai.embeddings_utils`: For handling embeddings (though not directly used in the provided code, it's imported).
    - `rich.progress`: For displaying a progress bar during processing.
    - `tenacity`: For implementing retry logic when calling the OpenAI API.

  - **Control Flow**:
    1. **Initialization**:
       - Loads environment variables (API keys, endpoints).
       - Sets up argument parsing for specifying the transcript folder and verbosity.
       - Configures logging.
    2. **File Processing**:
       - Locates all JSON transcript files in the specified folder using `glob`.
       - Adds each filename to a queue (`q`).
    3. **Multithreading**:
       - Creates a thread pool of `PROCESSING_THREADS` (defaulting to 10) threads.
       - Each thread executes the `process_queue` function.
    4. **`process_queue` Function**:
       - Dequeues a filename from the queue.
       - Reads the JSON metadata from the file.
       - Extracts the first minute of the transcript from the corresponding `.json.vtt` file using `get_first_segment`.
       - Constructs a text prompt including the title, description, and the first minute of the transcript.
       - Calls the OpenAI API via the `get_speaker_info` function to extract speaker names.
       - Updates the metadata with the extracted speaker names.
       - Writes the updated metadata back to the JSON file.
    5. **OpenAI API Interaction (`get_speaker_info` Function)**:
       - Uses the OpenAI `ChatCompletion.create` method with function calling.
       - Sends a prompt to the OpenAI model, instructing it to extract speaker names from the provided text.
       - Defines a function schema (`get_speaker_name`) that the model uses to format its response.
       - Parses the function call from the OpenAI response to extract the speaker names.
       - Implements retry logic using the `tenacity` library to handle potential API errors.
    6. **Cleanup**:
       - Waits for all threads to complete.
       - Logs the total execution time.

  - **Functions**:
    - `get_speaker_info(text)`: Calls the OpenAI API to extract speaker names from the given text using function calling. Retries on failure.
    - `clean_text(text)`: Cleans the input text by removing newlines, HTML entities, and extra spaces.
    - `get_first_segment(file_name)`: Extracts the first `SEGMENT_MIN_LENGTH_MINUTES` (defaulting to 3) minutes of transcript text from the VTT file associated with the given JSON file.
    - `process_queue(progress, task)`: Processes files from the queue, extracts speaker information, and updates the JSON files.
    - `Counter`: A thread-safe counter class to track progress.

  - **Error Handling**:
    - The `get_speaker_info` function uses `tenacity` to retry API calls with exponential backoff.
    - The `process_queue` function checks for excessive errors and exits if the error count exceeds 100.
    - Logging is used to record errors and debugging information.

  - **File System Operations**:
    - Reads JSON metadata files from the specified folder.
    - Reads corresponding `.json.vtt` transcript files.
    - Writes updated JSON metadata files back to the same location.

  - **User Instructions**:
    1.  Set the environment variables `AZURE_OPENAI_API_KEY` and `AZURE_OPENAI_ENDPOINT`.
    2.  Optionally, set `AZURE_OPENAI_MODEL_DEPLOYMENT_NAME` to specify the OpenAI model deployment name.
    3.  Run the script with the `-f` or `--folder` argument to specify the folder containing the transcript JSON files: `python speaker_update.py -f <transcript_folder>`.
    4.  Use the `--verbose` flag for more detailed logging output: `python speaker_update.py -f <transcript_folder> --verbose`.
```

## transcript_enrich_summaries.py

**File: summarize_youtube_transcript.py**

- **Description**: This script processes a JSON file containing YouTube transcript segments, generates summaries for each segment using the OpenAI API (Azure OpenAI Service), and saves the enriched segments (with summaries) to a new JSON file. It uses multithreading to speed up the summarization process.

- **High-Level Overview**:
  - **External Libraries**:
    - `json`: For reading and writing JSON files.
    - `os`: For interacting with the operating system, such as accessing environment variables and constructing file paths.
    - `queue`: For creating a thread-safe queue to hold transcript segments.
    - `threading`: For implementing multithreading to process segments concurrently.
    - `logging`: For logging errors, warnings, and debug information.
    - `argparse`: For parsing command-line arguments.
    - `dotenv`: For loading environment variables from a `.env` file.
    - `openai`: For interacting with the OpenAI API (Azure OpenAI Service).
    - `tenacity`: For adding retry logic to the OpenAI API calls.
    - `rich`: For displaying a progress bar in the console.

  - **Control Flow**:
    1. **Initialization**:
       - Loads environment variables (API key, endpoint, model deployment name) from a `.env` file.
       - Configures the OpenAI API.
       - Sets up argument parsing for command-line arguments (verbose mode, transcript folder).
       - Configures logging based on the verbose argument.
    2. **Loading Transcript Segments**:
       - Reads transcript segments from a JSON file (`master_transcriptions.json`) located in the specified transcript folder.
    3. **Summarization**:
       - Creates a thread-safe queue and adds all transcript segments to it.
       - Starts multiple threads (number defined by `PROCESSOR_THREADS`) to process the queue concurrently.
       - Each thread:
         - Dequeues a segment from the queue.
         - Calls the OpenAI API to generate a summary for the segment's text.
         - Adds the generated summary to the segment dictionary.
         - Appends the updated segment to a list of output segments.
       - A progress bar is displayed to track the summarization progress.
    4. **Sorting and Saving**:
       - Sorts the output segments by `videoId` and `start` time.
       - Saves the enriched segments (with summaries) to a new JSON file (`master_enriched.json`) in the same output folder.

  - **Functions**:
    - `chatgpt_summary(text)`:
      - Takes a text segment as input.
      - Calls the OpenAI API to generate a summary.
      - Includes retry logic using the `tenacity` library to handle potential API errors.
      - Returns the generated summary.
    - `process_queue(progress, task)`:
      - Processes segments from the queue.
      - Calls `chatgpt_summary` to generate summaries.
      - Updates the progress bar.
    - `convert_time_to_seconds(value)`:
      - Converts a time string in the format "HH:MM:SS" to seconds.

  - **Error Handling**:
    - Handles `openai.InvalidRequestError` exceptions during API calls and logs a warning.
    - Handles other exceptions during API calls and logs a warning.
    - Implements a counter to track the number of errors and exits if the error count exceeds a threshold.

  - **File System Operations**:
    - Reads transcript segments from `TRANSCRIPT_FOLDER/output/master_transcriptions.json`.
    - Writes enriched segments to `TRANSCRIPT_FOLDER/output/master_enriched.json`.

  - **User Instructions**:
    1.  Set the `AZURE_OPENAI_API_KEY` and `AZURE_OPENAI_ENDPOINT` environment variables. Optionally, set `AZURE_OPENAI_MODEL_DEPLOYMENT_NAME`.
    2.  Place the YouTube transcript JSON file (`master_transcriptions.json`) in a folder.
    3.  Run the script from the command line, providing the folder path as an argument: `python summarize_youtube_transcript.py -f /path/to/transcript/folder`.
    4.  Use the `--verbose` flag for debug logging.

