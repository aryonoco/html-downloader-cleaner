# html-downloader-cleaner

A simple Python script to download HTML content from a list of URLs and clean it, focusing on extracting the main textual content while removing clutter like navigation, ads, scripts, and styles.

This project uses `uv` for fast dependency management and provides an `install` script to simplify setup by creating a self-contained environment.

## Features

*   Downloads HTML from a list of URLs provided in `urls.txt`.
*   Attempts to identify and extract the main content block of the page.
*   Removes common non-content elements (scripts, styles, nav, footer, ads, etc.).
*   Filters tags and attributes to a predefined allow-list optimized for content
*   Resolves relative links within the cleaned content to absolute URLs.
*   Normalizes whitespace and basic HTML structure.
*   Adds basic metadata (Source URL, Retrieval Time) to the cleaned HTML.
*   Uses `httpx` for HTTP requests and `BeautifulSoup4` with `lxml` for parsing.

## Requirements

*   A Unix-like environment (Linux, macOS, WSL on Windows) to run the `install` shell script.
*   `curl` (usually pre-installed) to download `uv` if it's not already present.
*   Python 3.13 or newer (The `install` script will try to use `uv` to set this up, but having it available is recommended).
*   Internet connection (for installing `uv` and Python dependencies).

## Installation (The `uv` and `install` approach)

This project uses a simple `install` script to handle setup, leveraging the fast `uv` package manager. This avoids polluting your global Python environment and makes setup easier.

**What the `install` script does:**

1.  **Checks for/Installs `uv`:** Downloads and installs the `uv` tool if it's not found in your PATH.
2.  **Creates Virtual Environment:** Uses `uv` to create a dedicated, isolated Python virtual environment (`.venv_url_cleaner`) specifically for this script, attempting to use Python 3.13+.
3.  **Installs Dependencies:** Uses `uv pip install` to quickly install the required Python packages (`httpx`, `beautifulsoup4`, `lxml`, etc. from `requirements.txt`) into the dedicated virtual environment.
4.  **Creates Executable Wrapper:** Generates a wrapper script named `url-cleaner`. This script automatically uses the Python interpreter and packages from the `.venv_url_cleaner` environment, so you don't need to activate it manually.

**To install:**

1.  Make the install script executable:
    ```bash
    chmod +x install
    ```
2.  Run the install script:
    ```bash
    ./install
    ```

Follow any prompts from the script. If successful, it will create the `.venv_url_cleaner` directory and the `url-cleaner` executable file.

## Usage

1.  **Edit `urls.txt`:** Add the URLs you want to download and clean, one URL per line. Lines starting with `#` are ignored.
    ```txt
    # Example URLs
    https://example.com/article1
    https://another-site.org/some/page.html
    ```
2.  **Run the Cleaner:** Execute the wrapper script created during installation:
    ```bash
    ./url-cleaner
    ```
3.  **Find Output:** The script will process each URL. Cleaned HTML files will be saved in the `_output/` directory. Filenames are generated based on the URL's domain and path, plus a hash segment to avoid collisions (e.g., `example_com_article1_...hash....html`).

## Why this Installation Method?

Using the `install` script with `uv` provides several benefits, especially when distributing Python command-line tools:

*   **Isolation:** Dependencies are installed in a dedicated virtual environment (`.venv_url_cleaner`), preventing conflicts with other Python projects or system packages.
*   **Simplicity for Users:** Users don't need to manually create virtual environments or manage Python paths. They just run `./install` once, then use the simple `./url-cleaner` command.
*   **Reproducibility:** The `requirements.txt` file ensures the correct versions of dependencies are installed via `uv`.
*   **Speed:** `uv` is significantly faster than traditional `pip` and `venv` for creating environments and installing packages.
*   **No Global Installs:** Keeps the user's system clean.

The generated `url-cleaner` script is a "polyglot" script – it contains a shell command that finds and executes the correct Python interpreter within the hidden virtual environment, passing the rest of the script's content (the actual Python code) to it.

## License

Copyright Aryan Ameri. This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
