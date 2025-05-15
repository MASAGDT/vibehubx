
# VibeHubX

![License](https://img.shields.io/badge/license-OGI--VibeHubX-blueviolet)
![Status](https://img.shields.io/badge/status-v0.1--alpha-yellow)
![Framework](https://img.shields.io/badge/built%20with-Flask-ff69b4)
![PayPal](https://img.shields.io/badge/integration-PayPal-00457C?logo=paypal)
![GitHub](https://img.shields.io/badge/version--control-GitHub-181717?style=flat&logo=github&logoColor=white)
![BuiltBy](https://img.shields.io/badge/built%20by-Overlord%20Grid,%20Inc.-purple)

VibeHubX is a freelance platform connecting customers with "Vibe Coders" who share their aesthetic and workflow preferences. Customers pay upfront via PayPal, and gigs are claimed on a first-come-first-served basis by coders. The platform takes a 10% fee per transaction, with the remaining 90% paid to the coder upon completion. All work is delivered via GitHub repositories for transparency.

## Features

- **User Roles**:
  - **Customers**: Post gigs, pay upfront, and provide a GitHub repo for deliverables.
  - **Vibe Coders**: Claim gigs, complete them, and deliver via GitHub commits.
- **Gig Workflow**:
  - Customers post gigs with a title, description, budget, and GitHub repo URL.
  - Gigs expire after 24 hours if not claimed.
  - First coder to claim a gig owns it—no bidding wars.
  - Coders complete gigs by submitting a commit SHA, triggering an automatic payout.
- **Payment**:
  - PayPal integration for upfront payments.
  - 10% platform fee, 90% to the coder.
  - No refunds or warranties—equal risk for both sides.
- **Transparency**:
  - Mandatory GitHub repo for all gigs.
  - Commits verified via GitHub API.
- **SLA**: Work is completed at the coder’s earliest convenience unless a deadline is specified.

## Tech Stack

- **Backend**: Flask, SQLAlchemy, Flask-Migrate, Flask-Login, Flask-Bcrypt
- **Frontend**: HTML5, CSS3 (Bootstrap 5, custom styles with Poppins font)
- **Integrations**:
  - PayPal API for payments and payouts
  - GitHub API for repo validation and commit verification
- **Deployment**: Gunicorn (WSGI server), PostgreSQL (production database)
- **Development**: SQLite (development database), pytest for testing

## Directory Structure

```
vibehubx/
├── app/
│   ├── init.py
│   ├── models/
│   │   ├── user.py
│   │   ├── gig.py
│   │   ├── transaction.py
│   │   └── init.py
│   ├── routes/
│   │   ├── auth.py
│   │   ├── gigs.py
│   │   ├── dashboard.py
│   │   └── init.py
│   ├── services/
│   │   ├── payment_service.py
│   │   ├── github_service.py
│   │   └── init.py
│   ├── templates/
│   │   ├── auth/
│   │   │   ├── login.html
│   │   │   └── register.html
│   │   ├── gigs/
│   │   │   ├── post_gig.html
│   │   │   ├── list_gigs.html
│   │   │   └── gig_detail.html
│   │   ├── dashboard/
│   │   │   └── coder_dashboard.html
│   │   └── base.html
│   ├── static/
│   │   ├── css/
│   │   │   └── main.css
│   │   ├── js/
│   │   │   └── main.js
│   │   └── images/
│   │       └── logo.png
│   └── utils/
│       ├── auth_helpers.py
│       ├── decorators.py
│       ├── helpers.py
│       └── init.py
├── migrations/
├── requirements.txt
├── config.py
├── .env
├── .gitignore
└── run.py
```

## Setup Instructions

### Prerequisites

- Python 3.9+
- PostgreSQL (for production; SQLite is used for development by default)
- PayPal Developer Account (for API credentials)
- GitHub Personal Access Token (for API access)

### Installation

```bash
git clone <repository-url>
cd vibehubx
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt

# Create .env
echo "SECRET_KEY=your-secret-key-here" > .env
echo "DATABASE_URL=sqlite:///vibehubx.db" >> .env
echo "PAYPAL_MODE=sandbox" >> .env
echo "PAYPAL_CLIENT_ID=your-paypal-client-id" >> .env
echo "PAYPAL_CLIENT_SECRET=your-paypal-client-secret" >> .env
echo "GITHUB_TOKEN=your-github-token" >> .env

# Set environment for Flask
export FLASK_APP=run.py  # On Windows: set FLASK_APP=run.py

# Initialize database
flask db init
flask db migrate -m "Initial migration"
flask db upgrade
```

### Production Setup (PostgreSQL)

```bash
pip install psycopg2-binary
# update .env:
DATABASE_URL=postgresql://user:password@localhost:5432/vibehubx

# re-run setup
flask db init
flask db migrate -m "Initial migration"
flask db upgrade
```

### Running the Application

**Development**
```bash
export FLASK_ENV=development  # Windows: set FLASK_ENV=development
python run.py
```

**Production**
```bash
export FLASK_ENV=production  # Windows: set FLASK_ENV=production
gunicorn -w 4 -b 0.0.0.0:8000 run:app
```

### Testing

```bash
pytest
```

## Usage

- **Register/Login** at `/register` and `/login`
- **Post a Gig** at `/post`
- **Claim a Gig** at `/`
- **Complete a Gig** via `/dashboard` (submit commit SHA)

## Troubleshooting

### .env Not Loading

```bash
pip show python-dotenv
pip install python-dotenv==1.0.0
```

Ensure `.env` is in the project root and `load_dotenv()` is used in `config.py` or `__init__.py`.

### Can't find migrations\env.py

```bash
rmdir /S /Q migrations
flask db init
```

### module 'config' has no attribute 'Config'

Ensure `config.py` exists and defines a class `Config`. Check no conflicting `config.pyc` or modules exist.

```bash
python -c "import config; print(config.__file__)"
```

### PayPal Errors

Verify client credentials. Confirm sandbox account in PayPal Developer Dashboard.

### GitHub Errors

Ensure token has `repo` scope.

## Notes

- No refunds/warranties: shared risk model
- Mandatory GitHub repo for all gigs

## Future Improvements

- WebSockets for real-time gig claims
- Filter support (community, vibe, budget)
- Logging and error reporting enhancements

## Contributing

Fork → Branch → PR

## License

© 2025 VibeHubX | A division of Overlord Grid, Inc. All rights reserved.
