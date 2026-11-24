# Student Grade Improvement Recommendation System

A Streamlit-based academic support application that analyzes a student's overall CGPA and recent CAT1/CAT2 exam marks. It provides AI-generated study advice, personalized evening study schedules, grade classifications, and links to relevant YouTube learning resources.

## Features

- In-app login and account registration
- CGPA entry and subject-wise marks entry
- Support for CAT1 and CAT2 examinations
- Automatic grade calculation:
  - A: above 45
  - B: 41-45
  - C: 36-40
  - D: 31-35
  - E: 26-30
  - F: 25 or below
- Overall performance classification: Excellent, Good, Average, or Below Average
- OpenRouter-powered personalized study recommendations
- Personalized 7:00 PM-12:00 AM study timetable for relevant performance levels
- YouTube/NPTEL course recommendations for weaker subjects
- Local SQLite storage for user accounts and submitted results
- Custom background image and Streamlit interface styling

## Tech Stack

| Area | Technology |
|---|---|
| Application framework | Streamlit |
| Programming language | Python 3 |
| Database | SQLite |
| Data handling | Pandas |
| AI recommendation service | OpenRouter API |
| Learning-resource search | YouTube Data API v3 |
| HTTP requests | Requests |
| UI assets | HTML/CSS and a local PNG background |

## Project Structure

```text
.
|-- app.py                    # Streamlit application and main logic
|-- background.png            # Application background image
|-- student_data.db           # SQLite database created and used by the app
|-- .streamlit/
|   `-- secrets.toml          # Local API configuration (do not commit)
|-- .gitignore
`-- README.md
```

## Prerequisites

- Python 3.9 or newer
- `pip`
- An OpenRouter API key for AI recommendations
- A YouTube Data API key for direct course searches

YouTube search is optional. If no YouTube key is configured, the app opens a YouTube search URL for the subject.

## Installation

1. Clone the repository:

```bash
git clone https://github.com/ericjbejoy-ux/STUDENT-GRADE-IMPORVEMENT-RECOMMENDATION-SYSTEM.git
cd STUDENT-GRADE-IMPORVEMENT-RECOMMENDATION-SYSTEM
```

2. Create and activate a virtual environment:

Windows:

```powershell
python -m venv .venv
.venv\Scripts\activate
```

macOS/Linux:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

3. Install the required packages:

```bash
python -m pip install streamlit pandas requests
```

For repeatable installations, create a `requirements.txt` containing:

```text
streamlit
pandas
requests
```

Then install from it:

```bash
python -m pip install -r requirements.txt
```

## API Configuration

Create `.streamlit/secrets.toml` in the project root and add:

```toml
YOUTUBE_API_KEY="your-youtube-data-api-key"
OPENROUTER_API_KEY="your-openrouter-api-key"
```

When deploying to Streamlit Community Cloud, add the same values under **App Settings > Secrets** instead of uploading them as plain files.

Never commit real API keys or expose them in screenshots, issues, logs, or source code. If a key has already been shared or committed, revoke it and create a replacement.

## Running the Application

From the project root, run:

```bash
streamlit run app.py
```

Streamlit will print a local URL, usually `http://localhost:8501`.

## Using the System

1. Open the application in your browser.
2. Create an account or log in.
3. Enter the historical CGPA.
4. Select the number of subjects.
5. Choose CAT1 or CAT2.
6. Enter each subject name and marks out of 50.
7. Submit the form to receive:
   - a performance classification
   - AI-generated improvement tips
   - a study schedule when applicable
   - remedial YouTube/NPTEL course links

The application initializes with an administrator account:

- Username: `admin`
- Password: `1234`

Change or remove the development credentials before production use.

## Data Storage

The application uses `student_data.db`, a local SQLite database. It creates and uses:

- `users` - usernames and passwords
- `student_marks` - submitted CGPA, subject details, grades, and exam information

The database is initialized automatically when the application starts.

> **Security notice:** This educational prototype stores passwords as plain text and is not intended for production authentication. For a real deployment, use password hashing and a production-ready database and authentication provider.

## OpenRouter and YouTube Behavior

- OpenRouter is queried with a subject-wise marks summary and historical CGPA.
- The model prompt requests advice and, for eligible CGPA ranges, a Markdown schedule.
- YouTube searches for university engineering courses and NPTEL resources.
- API failures and missing keys are shown within the application, while the YouTube feature falls back to a general search link.

## Troubleshooting

### Streamlit does not start

Confirm that the virtual environment is active and packages are installed:

```bash
python -m pip install streamlit pandas requests
```

### AI recommendations show a missing API-key message

Confirm that `.streamlit/secrets.toml` exists and contains `OPENROUTER_API_KEY`.

### YouTube course links do not find direct resources

Confirm that `YOUTUBE_API_KEY` is valid and enabled for the YouTube Data API v3. The app will otherwise use a YouTube search fallback.

### Database changes are missing

Run the app from the project root. SQLite resolves `student_data.db` relative to the process working directory.

## Suggested Improvements

- Hash passwords using bcrypt or Argon2
- Add environment-aware configuration and database migrations
- Export student performance reports as CSV or PDF
- Add subject and semester history dashboards
- Add automated tests for grading, authentication, and API fallbacks
- Improve API error handling and add retry/rate-limit handling
- Move production data to PostgreSQL or a managed database

## License

No license is currently specified. Add a `LICENSE` file if you intend to grant reuse or distribution permissions.
