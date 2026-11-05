# AI-Powered Academic Notes Summarizer

A beginner-friendly Flask web application that turns student-provided lecture notes and searchable PDF documents into structured study material using the Groq API.

## Features

- Paste academic notes, upload a PDF, or provide both.
- Generate a main summary, key concepts, definitions, important points, exam questions, quick revision notes, and key takeaways.
- Choose Short Summary, Detailed Summary, or Exam Preparation Notes.
- Choose English or Urdu output.
- Copy the study guide or download it as a `.txt` file.
- Responsive dashboard with loading, success, and error states.
- PDF text extraction with PyMuPDF.
- API key stored on the backend in `.env`; it is never sent to frontend JavaScript.
- No database. Uploaded content is processed in memory and is not saved by this app.

## Important accuracy note

The AI is instructed to use only the supplied material and to flag missing information rather than fill gaps. Language models can still make mistakes, so students should compare important facts and definitions with their original notes. The app does not claim that AI output is guaranteed to be error-free.

Scanned PDFs that contain only page images need OCR before upload. PyMuPDF extracts selectable text; it does not perform OCR in this project.

## Technologies

- Python 3.10+
- Flask
- Groq Python SDK
- Groq-hosted language model (default: `llama-3.3-70b-versatile`)
- PyMuPDF (`fitz`)
- python-dotenv
- HTML5, CSS3, JavaScript, Bootstrap 5

## Project structure

```text
academic-notes-summarizer/
├── app.py
├── requirements.txt
├── .env.example
├── .gitignore
├── README.md
├── templates/
│   └── index.html
├── static/
│   ├── style.css
│   └── script.js
└── uploads/
    └── .gitkeep
```

## Requirements

- Windows 10/11, macOS, or Linux
- Python 3.10 or newer
- Internet connection for Groq API requests
- A Groq API key

## Windows PowerShell setup

Open PowerShell in the project folder.

### 1. Create a virtual environment

```powershell
py -m venv venv
```

### 2. Activate it

```powershell
venv\Scripts\activate
```

If PowerShell blocks activation, run this command for the current terminal session, then activate again:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
venv\Scripts\activate
```

### 3. Install dependencies

```powershell
py -m pip install --upgrade pip
py -m pip install -r requirements.txt
```

### 4. Get a Groq API key

Create or sign in to your Groq account and create an API key from the Groq Console:
https://console.groq.com/keys

Keep the key private. Do not paste it into `app.py`, `script.js`, screenshots, or a public GitHub repository.

### 5. Create your `.env` file

In PowerShell:

```powershell
Copy-Item .env.example .env
notepad .env
```

Replace the placeholder with your real key:

```env
GROQ_API_KEY=your_real_groq_api_key
GROQ_MODEL=llama-3.3-70b-versatile
```

Save and close Notepad. The `.gitignore` file prevents `.env` from being committed to Git.

### 6. Run the application

```powershell
py app.py
```

### 7. Open it in your browser

Visit:

http://127.0.0.1:5000

Keep the PowerShell window open while using the application. Press `Ctrl+C` in that window to stop the server.

## How to use

1. Paste your notes in the text area, select a PDF, or do both.
2. Choose the summary style and output language.
3. Select **Generate Summary**.
4. Review the seven study-guide sections.
5. Use **Copy Summary** or **Download .txt** to keep a copy.
6. Use **Generate Again** to return to the input form, or **Clear All** to reset the app.

## Limits and privacy

- Maximum HTTP upload size: 10 MB.
- Maximum extracted/pasted source text: 80,000 characters.
- PDF upload accepts `.pdf` files only.
- The app does not save source documents or generated results to a database or upload folder.
- Source text is sent to Groq for processing. Do not submit confidential, personal, or restricted material unless you are permitted to share it with that service.
- The API key is read by Flask from the server-side `.env` file.

## Common errors and solutions

| Error | What to do |
|---|---|
| `GROQ_API_KEY is not configured` | Create `.env` from `.env.example`, add your key, and restart Flask. |
| Groq authentication failed | Check the key for typing mistakes, revoke exposed keys, create a replacement if needed, and restart the app. |
| Rate limit reached | Wait briefly and retry. |
| Could not connect to Groq | Check internet access and try again. |
| PDF could not be read | Try opening the PDF in a reader and saving a fresh copy. |
| No selectable text found | The PDF may be scanned; OCR it to create a searchable PDF first. |
| Upload too large | Use a PDF under 10 MB and keep extracted text under 80,000 characters. |
| `py` is not recognized | Install Python from https://www.python.org/downloads/ and enable the option to add Python to PATH. |
| `ModuleNotFoundError` | Activate `venv` and run `py -m pip install -r requirements.txt` again. |
| Port 5000 is already in use | Close the other app using port 5000, then run this app again. |

## Quick developer checks

Check Python syntax:

```powershell
py -m py_compile app.py
```

Check installed packages:

```powershell
py -m pip check
```

The live Groq integration requires a valid API key and internet connection. The app intentionally does not show fake or sample AI results when the API is unavailable.

## Troubleshooting and model choice

The default model is set in `.env` as `GROQ_MODEL`. If Groq changes model availability or your account does not have access to the default, select a currently supported chat model in the Groq Console and update that value. Restart Flask after changing `.env`.
