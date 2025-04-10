# Python Requirements Generator Script

## Description

This Python script automatically generates a `requirements.txt` file for your Python project. It scans all `.py` files within the current directory and its subdirectories, identifies imported external packages, resolves their installed versions, and writes them to `requirements.txt`.

This helps ensure your dependencies are accurately listed, aiding reproducibility and collaboration.

## How it Works

1.  Uses `os.walk` to find all `.py` files in the project.
2.  Parses each `.py` file using the `ast` module to find `import` statements without executing code.
3.  Filters out standard library modules and relative imports.
4.  Uses `importlib.metadata` to find the official package name and installed version corresponding to each import. It includes fallbacks for cases where import names differ from package names (e.g., `bs4` -> `beautifulsoup4`).
5.  Writes the discovered packages and versions in `package==version` format to `requirements.txt` in the current directory.

## Usage

1.  **Place the Script:** Save the script (e.g., `generate_reqs.py`) in the **root directory** of your Python project.
2.  **Activate Environment:** Make sure you are using the correct Python virtual environment for your project, and that all necessary dependencies are installed in it. The script reads versions from the *currently installed* packages in the active environment.
3.  **Run from Root:** Navigate to your project's root directory in your terminal.
4.  **Execute:** Run the script using Python:
    ```bash
    python generate_reqs.py
    ```
5.  **Output:** A `requirements.txt` file will be generated or overwritten in the root directory. Check the terminal output for a success message or any warnings about unresolved packages.

![Run script](run%20generate%20reqs.gif)

## Dependencies

The script itself uses only Python standard library modules (`os`, `sys`, `ast`, `importlib.metadata`). However, its **accuracy depends on having your project's dependencies installed** in the Python environment where you run the script, as it needs to query them for version information.

## Notes & Caveats

* **Run in Project Root:** The script must be run from the main directory of your project to correctly scan all subdirectories.
* **Environment Specific:** The generated `requirements.txt` reflects the package versions installed in the *current* Python environment at the time the script is run.
* **Unresolved Packages:** If a package is imported but not installed (or cannot be resolved correctly by `importlib.metadata`), it might be listed with a placeholder version like `package==0.0.0 # WARNING: Version not found`. You may need to review these entries.
* **Dynamic Imports:** Packages imported dynamically (e.g., using `importlib.import_module()`) will not be detected.
