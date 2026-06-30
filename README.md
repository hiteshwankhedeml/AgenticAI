# 🟢 UV

* <mark style="color:purple;background-color:purple;">**Package manager**</mark>
* <mark style="color:purple;background-color:purple;">**Project manager:**</mark>
  * <mark style="color:purple;background-color:purple;">**uv init <\<proj\_name>>**</mark>
    * It will have&#x20;
    * main.py
    * <mark style="color:purple;background-color:purple;">**pyproject.toml**</mark>
      * <mark style="color:purple;background-color:purple;">**This will have version, dependencies etc**</mark>
    * If we do uv add numpy ⇒ This will add numpy in dependencies in toml
    * If we do uv add -r requirements.txt ⇒ It will also add all the libraries in toml
* 10 to 100x faster than pip
*

    ```
    # On macOS and Linux.
    curl -LsSf https://astral.sh/uv/install.sh | sh
    ```
*

    ```
    powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
    ```
*

    ```
    # With pip.
    pip install uv
    ```



**Create venv:**

* uv venv --python 3.12
* source .venv/bin/activate



