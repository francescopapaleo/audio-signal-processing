# Audio Signal Processing with Python

Updating and Deploying the Jupyter Book with GitHub Pages

This repository hosts a Jupyter Book, deployed via GitHub Pages on the gh-pages branch. The following steps outline how to update the book and refresh the hosted webpage.

## Prerequisites

Ensure you have Jupyter Book installed:

    pip install jupyter-book

### Use cookiecutter-jupyter-book to Generate the Project

If you are starting a new Jupyter Book project, you can use the official cookiecutter template:

    pip install cookiecutter
    
    cookiecutter https://github.com/executablebooks/cookiecutter-jupyter-book

It will prompt you for options (book name, author, include CI/CD, etc.).
Important: Choose GitHub when prompted for include_ci so that it sets up the GitHub Actions workflow.

If you already have an existing Jupyter Book repository, you can manually add the GitHub Actions workflow in the next step.

## Add the GitHub Actions Workflow

If the cookiecutter template was not used, create the workflow manually:

In your GitHub repository, navigate to:

    .github/workflows/

If the folder does not exist, create it.

Create a new YAML file for the workflow:

    .github/workflows/deploy-jupyter-book.yml

Add the following contents:

```
name: Deploy Jupyter Book

on:
  push:
    branches:
      - main  # This runs the workflow whenever changes are pushed to `main`

jobs:
  build-deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout repository
      uses: actions/checkout@v4

    - name: Set up Python
      uses: actions/setup-python@v4
      with:
        python-version: "3.9"

    - name: Install dependencies
      run: |
        pip install jupyter-book ghp-import

    - name: Build the Jupyter Book
      run: |
        jupyter-book build .

    - name: Deploy to GitHub Pages
      run: |
        ghp-import -n -p -f _build/html
```

## Push Changes to GitHub

Once the workflow file is added, commit and push the changes:

    git add .github/workflows/deploy-jupyter-book.yml

    git commit -m "Add GitHub Actions workflow for Jupyter Book deployment"

    git push origin main
