# Python-PSA-PDS

A portable, offline-capable **Personnel Directory System (PDS)** built with Flask, made for the **Provincial Statistical Office of Benguet**. It runs without an internet connection and supports multiple devices on the same local network accessing it simultaneously from a shared folder or server folder.

## Description

The system stores personnel records (personal information, dependents, educational background, eligibility, service record, and training/development history) in a local SQLite database, with a login-protected web interface for viewing, adding, editing, and deleting entries.

## Features

- Personnel record management (Create, Read, Update, Delete)
- Login-protected access using session-based authentication
- Activity logging (login attempts, successes/failures)
- Runs fully offline — no internet connection required
- Supports simultaneous use by multiple devices on the same network via a shared/server folder
- Bundled portable **DB Browser for SQLite Portable v.3.1.2** for direct database management (viewing/editing users and records)
- Packaged standalone executable (`PSA Personnel Directory System v3.exe`) requiring no separate Python installation
- Includes a documented fix for a common Flask/SQLAlchemy "working outside of application context" runtime error

## Tech Stack

- **Backend:** Python, [Flask](https://flask.palletsprojects.com/) `3.0.3`
- **ORM:** [Flask-SQLAlchemy](https://flask-sqlalchemy.palletsprojects.com/) `3.1.1`, [SQLAlchemy](https://www.sqlalchemy.org/) `2.0.35`
- **HTTP client:** [Requests](https://requests.readthedocs.io/) `2.32.3`
- **Database:** SQLite
- **Frontend:** HTML templates (Flask/Jinja) with static assets (CSS/JS)

## Prerequisites

- Python 3 and pip (only needed if running from source — the packaged `.exe` requires no separate Python installation)

## Installation

Clone the repository:

```bash
git clone https://github.com/paoradox/Python-PSA-PDS.git
cd Python-PSA-PDS
```

Install dependencies:

```bash
pip install -r requirements.txt
```

If the database tables haven't been created yet, initialize them (see **Fix SQLAlchemy Operational-Runtime Error** below for the full steps).

## Usage

### Option 1: Run with the provided batch script (Windows)

From the project root, run:

```
run-app-ms-terminal.bat
```

This starts the Flask server in a terminal window and opens `http://127.0.0.1:5000/` in your default browser. **Do not close the server terminal window while using the app.**

### Option 2: Run the packaged executable (Windows)

A pre-built executable, `PSA Personnel Directory System v3.exe`, is included at the project root and can be run directly without a Python installation.

### Option 3: Run manually

```bash
python app.py
```

Then open `http://127.0.0.1:5000/` in your browser.

### Shared network use

To allow multiple devices on the same network to use the system simultaneously, place the project folder in a shared/server folder accessible to those devices, and run it from there.

### Database management

To open the database directly for management (e.g. modifying users or records), run:

```
mod-app-ms-database.bat
```

This launches the bundled **DB Browser for SQLite Portable v.3.1.2** pointed at the project's `database.db`. Alternatively, run `SQLite Browser.exe` at the project root directly.

## Configuration

- The database connection is configured in `app.py` via `SQLALCHEMY_DATABASE_URI`, pointing to a local `database.db` (SQLite) file in the project folder — no external database server is required.
- A backup of the database and UI templates is included in the `default/back-up db` and `default/back-up ui` folders.

## Troubleshooting

**"Working outside of application context" (Flask/SQLAlchemy runtime error):**

If the database tables aren't created yet, run the app once, then in a terminal in the project's root folder:

```bash
python
```
```python
from app import app, db
app.app_context().push()
db.create_all()
```

(See `Fix SQLAlchemy Operational-Runtime Error/INSTRUCTIONS.rtf` for full details, and the included Stack Overflow reference in the same folder.)

## Project Structure

```
Python-PSA-PDS/
├── DB Browser for SQLite Portable v.3.1.2/   # Portable SQLite database browser
│   ├── App/
│   ├── Data/
│   ├── Other/
│   ├── SQLiteDatabaseBrowserPortable.exe
│   └── help.html
├── Fix SQLAlchemy Operational-Runtime Error/
│   ├── INSTRUCTIONS.rtf
│   └── python - working outside of application context - Flask - Stack Overflow.html
├── default/                                  # Backups of the database and UI templates
│   ├── back-up db/
│   │   ├── instance/database.db
│   │   └── database.db
│   └── back-up ui/
│       ├── db ref.txt
│       ├── index-copy.html
│       ├── modaladd-copy.html
│       ├── modalupdate-copy.html
│       ├── pds_add.html
│       └── pds_update.html
├── instance/
│   └── database.db
├── static/
│   ├── bootstrap-3.3.7-dist/                 # Bootstrap 3 framework
│   ├── css/
│   │   ├── default-login.css
│   │   ├── default.css
│   │   └── signin.css
│   ├── img/
│   ├── jq/                                   # jQuery
│   └── js/
├── templates/
│   ├── index.html
│   └── login.html
├── .gitattributes
├── PSA Personnel Directory System v3.exe     # Packaged executable
├── SQLite Browser.exe                        # Shortcut to the portable DB browser
├── app.py                                    # Flask application
├── mod-app-ms-database.bat                   # Opens DB Browser for SQLite
├── requirements.txt
└── run-app-ms-terminal.bat                   # Starts the Flask server
```

## License

Apache License 2.0
