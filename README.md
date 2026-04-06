# air150adsbb
python -m venv .venv .venv\Scripts\activate python -m pip install -r requirements.txt del /s *.db python -m flask --app wsgi:app init-db python -m flask --app wsgi:app seed python -m flask --app wsgi:app create-admin python wsgi.py
