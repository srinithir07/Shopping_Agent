# AI Shopping Assistant

This folder contains the `6_PROJECT_SHOPPING_AGENT` project: a Streamlit-powered AI shopping assistant that searches a local product database, reads product ratings, and can place simple orders using a LangChain agent.

## Overview

The project includes:

- `app.py` — the Streamlit web UI for the shopping assistant.
- `shopping_agent.py` — the LangChain agent logic and tool definitions.
- `reviews_api.py` — helper functions for querying product ratings.
- `setup_db.py` — creates the local `store.db` SQLite database and populates products, reviews, and orders.
- `store.db` — local SQLite database file containing seed data.
- `.env` — environment variables used by the agent (API keys).

## Features

- Search products by keyword, price, and organic status.
- Analyze uploaded product images and recommend matching store items.
- Retrieve average review ratings for products.
- Place orders and save them to the local database.
- Chat-style interface powered by a LangChain agent.

## Requirements

This project assumes a Python 3.12+ environment.

Recommended packages include:

- `streamlit`
- `python-dotenv`
- `langchain`
- `langchain-core`
- `langchain-groq`
- `langchain-huggingface`
- `langchain-chroma`
- `chromadb`
- `torch`
- `sentence-transformers`

If your workspace already has a root `pyproject.toml` with dependencies, you can install them from there.

## Environment Variables

The app requires the following environment variables:

- `GROQ_API_KEY` — API key for Groq.
- `GOOGLE_API_KEY` — optional, may be used by other components.
- `LANGSMITH_API_KEY` — optional if the LangSmith integration is used.

The project loads variables from the `.env` file located in the `6_PROJECT_SHOPPING_AGENT` folder.

### Example `.env`

```dotenv
GROQ_API_KEY=your_groq_api_key_here
GOOGLE_API_KEY=your_google_api_key_here
LANGSMITH_API_KEY=your_langsmith_api_key_here
```

> Do not commit actual secret keys to source control.

## Setup

1. Open a terminal in `6_PROJECT_SHOPPING_AGENT`.
2. Create and activate a virtual environment if you don’t already have one:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

3. Install dependencies:

```powershell
python -m pip install --upgrade pip
python -m pip install streamlit python-dotenv langchain langchain-core langchain-groq langchain-huggingface langchain-chroma chromadb torch sentence-transformers
```

4. Create or update `.env` with the required API keys.

5. Initialize the database (only needed if `store.db` is missing or must be recreated):

```powershell
python setup_db.py
```

## Running the App

Start the Streamlit interface from the project folder:

```powershell
streamlit run app.py
```

Then open the displayed URL in your browser.

## Usage

- Type shopping requests like `I want organic honey under $15 with a 4+ rating`.
- Upload a product image in the sidebar to search by photo.
- The assistant returns search results as a numbered list with IDs.
- Confirm an order by replying with `yes` or selecting the item number.

## Project Structure

- `app.py`
  - Streamlit chat UI
  - image upload handling
  - calls the LangChain agent to answer user queries
- `shopping_agent.py`
  - defines database tools for searching, rating, checkout, and image description
  - configures the LangChain agent
- `reviews_api.py`
  - includes helper functions for querying review averages from `store.db`
- `setup_db.py`
  - builds the database schema and inserts sample product and review data
- `store.db`
  - local SQLite database storing product, review, and order tables

## Troubleshooting

### `GROQ_API_KEY is not set`

If you see this error when importing `shopping_agent.py`:

- Verify `.env` exists in `6_PROJECT_SHOPPING_AGENT`.
- Confirm the file contains `GROQ_API_KEY=...`.
- Ensure the app is started from the project folder or that `.env` is explicitly loaded.
- You can also set it in the shell before running the app:

```powershell
$env:GROQ_API_KEY="your_groq_api_key_here"
streamlit run app.py
```

### Database errors

If the app reports `no such table: reviews` or similar:

```powershell
python setup_db.py
```

Recreate the database file so the expected tables exist.

### Missing dependencies

If the app fails because a module is not found, install the missing package in the active venv.

## Notes

- `store.db` is shipped with this project, so the app can run immediately once dependencies and environment variables are configured.
- If you change `.env`, restart the Streamlit app so it reloads the new values.
- The current implementation expects the Groq API key to be available before `shopping_agent.py` initializes.

## Optional enhancements

- Add a dedicated `requirements.txt` for this folder.
- Add input validation and error handling for image uploads.
- Add a checkout confirmation screen in the Streamlit UI.
- Add more product categories and richer review data.
