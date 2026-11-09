# NovaDesk Request Tracker

Interne demoapplicatie voor het opvolgen van klantaanvragen bij NovaDesk.

## Doel van de applicatie
NovaDesk Request Tracker is een applicatie waarmee gebruikers en supportmedewerkers klantaanvragen kunnen registreren en opvolgen. Aanvragen worden ingedeeld in categorieën (sales, general, technical en billing), krijgen een prioriteit (low tot urgent) en een status. Afhankelijk van hun rol kunnen medewerkers aanvragen bewerken en toewijzen.

## Gebruikte technologieën

- Backend: Python, Flask, Flask-Login
- ORM en databases: Flask-SQLAlchemy, SQLite (lokaal/tests), PostgreSQL (Docker)
- Configuratie: python-dotenv
- Monitoring: Prometheus-client
- Server en containerisatie: Gunicorn, Docker, Docker Compose
- Tests: pytest, pytest-flask
- CI/CD: GitHub Actions Workflow 

## Projectstructuur 

- .env.example
- .gitignore
- Dockerfile
- docker-compose.yml
- config.py
- requirements.txt
- run.py
- seed.py
- instance/
	- .gitkeep
	- novadesk.db
- app/
	- __init__.py
	- auth.py
	- errors.py
	- models.py
	- monitoring.py
	- routes.py
	- services.py
	- static/
		- styles.css
	- templates/
		- base.html
		- dashboard.html
		- edit_request.html
		- error.html
		- login.html
		- request_detail.html
		- request_form.html
		- requests.html
		- users.html
- tests/
	- conftest.py
	- test_auth.py
	- test_health.py
	- test_permissions.py
	- test_requests.py

## Lokaal opstarten 

### Stappenplan voor Linux

1. **Open de map in de terminal:** Navigeer met `cd` naar de map van het project:

```bash
cd pad/novadesk-request-tracker
```

2. **Virtuele omgeving aanmaken:**

```bash
python3 -m venv .venv
```

3. **Virtuele omgeving activeren:**

```bash
source .venv/bin/activate
```

*Verificatie:* Je ziet `(.venv)` voor je prompt staan.

4. **Afhankelijkheden installeren:** Installeer de pakketten uit `requirements.txt`:

```bash
python3 -m pip install -r requirements.txt
```

5. **Database vullen (seeden):** Het seed-script maakt de database en demoaanvragen aan:

```bash
python3 seed.py
```

6. **Start de Flask-applicatie:** Geef Flask de locatie van de applicatie in `run.py` en start de ontwikkelserver:

```bash
export FLASK_APP=run.py
flask run
```

*Verificatie:* De server draait op `http://127.0.0.1:5000`.

### Stappenplan voor Windows

1. **Open de map in de terminal:** Navigeer naar je projectmap.
Open **Command Prompt** (cmd) of **PowerShell**, en ga naar de map van je project met het commando `cd`:

```cmd
cd pad\novadesk-request-tracker

```


2. **Virtuele omgeving aanmaken:**
Maak de virtuele omgeving aan met `python`:

```cmd
python -m venv .venv

```


3. **Virtuele omgeving activeren:**
Het activeringsscript bevindt zich op Windows in een andere map (`Scripts` in plaats van `bin`).

* **In Command Prompt (cmd):**

```cmd
.venv\Scripts\activate.bat

```

* **In PowerShell:**

```powershell
.venv\Scripts\Activate.ps1

```

*(Krijg je een foutmelding in PowerShell over uitvoeringsbeleid? Voer dan eerst `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process` uit).*

*Verificatie:* Je ziet nu `(.venv)` voor je regel in de terminal staan.


4. **Afhankelijkheden installeren:**
Installeer de pakketten uit je `requirements.txt`:

```cmd
pip install -r requirements.txt

```

*Verificatie:* De packages worden gedownload en geïnstalleerd zonder rode foutmeldingen.


5. **Database vullen (Seeden):**
Voer het seed-script uit:

```cmd
python seed.py

```


6. **Start de Flask-applicatie:** Start de applicatie vanuit `run.py`:

```cmd
python run.py

```

*Verificatie:* Je ziet de server draaien op `[http://127.0.0.1:5000](http://127.0.0.1:5000)`.

## Opstarten met Dockerfile
1. Bouw het image:
```cmd
docker build -t novadesk-app .
```

2. Maak een benoemd volume aan voor de SQLite-database:
```cmd
docker volume create novadesk_instance
```

3. Maak de tabellen aan en laad de demo-inhoud:
```cmd
docker run --rm -v novadesk_instance:/app/instance novadesk-app python seed.py
```

4. Start de container:
```cmd
docker run --rm -p 5000:5000 -v novadesk_instance:/app/instance novadesk-app
```

De applicatie is daarna bereikbaar op `http://localhost:5000`.

## Opstarten met Docker Compose
1. Bouw de images en start de containers op de achtergrond:
```cmd
docker compose up --build -d
```

2. Maak de tabellen aan en laad de demo-inhoud:
```cmd
docker compose exec web python seed.py
```

De applicatie is daarna bereikbaar op `http://localhost:3000`. Stop de containers met `docker compose down`. De PostgreSQL-data blijft in de Docker-volume `db_data` bewaard. Het seed-script wist bestaande gegevens; voer het niet opnieuw uit als je data wilt behouden.

## Environment variables

Maak voor lokaal gebruik een `.env`-bestand op basis van `.env.example`:

```bash
cp .env.example .env
```

Op Windows kun je het bestand in Command Prompt kopiëren met `copy .env.example .env`, of in PowerShell met `Copy-Item .env.example .env`.

| Variabele | Voorbeeldwaarde | Functie |
| --- | --- | --- |
| `FLASK_APP` | `run.py` | Geeft aan welk bestand Flask start wanneer je `flask run` uitvoert. |
| `SECRET_KEY` | Zelfgekozen geheime waarde | Wordt gebruikt om onder andere sessies te beveiligen. Stel een unieke waarde in; de standaardwaarde in de code is alleen geschikt voor lokaal ontwikkelen. |
| `DATABASE_URL` | `sqlite:///novadesk.db` | De databaseverbinding. Als deze variabele ontbreekt, gebruikt de applicatie `instance/novadesk.db`. Docker Compose stelt deze in op PostgreSQL. |
| `FLASK_ENV` | `development` | Staat nog in `.env.example`, maar wordt niet meer gebruikt door Flask 3.0. |

Docker Compose gebruikt ook de volgende variabelen voor de PostgreSQL-container. De waarden staan momenteel rechtstreeks in `docker-compose.yml`:

| Variabele | Voorbeeldwaarde | Functie |
| --- | --- | --- |
| `POSTGRES_USER` | `myuser` | Gebruikersnaam voor de PostgreSQL-database. |
| `POSTGRES_PASSWORD` | `mypassword` | Wachtwoord voor de PostgreSQL-database. Vervang dit buiten lokaal gebruik door een geheim wachtwoord. |
| `POSTGRES_DB` | `mydb` | Naam van de database die PostgreSQL aanmaakt. |

## Database en seed data

De applicatie gebruikt Flask-SQLAlchemy om met de database te werken. De database wordt gekozen met `DATABASE_URL`:

| Situatie | Database |
| --- | --- |
| Lokaal, met `DATABASE_URL` uit `.env.example` | SQLite in `instance/novadesk.db`. Relatieve SQLite-paden worden onder de Flask-instance-map geplaatst. |
| Lokaal, zonder `DATABASE_URL` | SQLite in `instance/novadesk.db`. |
| Tests | SQLite in het geheugen; de testconfiguratie overschrijft `DATABASE_URL`. |
| Docker Compose | PostgreSQL 17; Compose stelt de `DATABASE_URL` voor de webcontainer in. De databasegegevens worden bewaard in de Docker-volume `db_data`. |

De applicatie gebruikt vier databasetabellen:

| Tabel | Inhoud |
| --- | --- |
| `users` | Gebruikers, wachtwoordhashes, rollen en accountstatus. |
| `support_requests` | Klantaanvragen, categorie, prioriteit, status en eventuele toegewezen medewerker. |
| `comments` | Reacties bij aanvragen, gekoppeld aan een aanvraag en gebruiker. |
| `audit_logs` | Registraties van acties op aanvragen en in de applicatie. |

### Seed data

Vul de database met demo-inhoud vanuit de projectmap:

```bash
python seed.py
```

Op Linux kan ook `python3 seed.py` worden gebruikt. Het script maakt de tabellen opnieuw aan en voegt 5 gebruikers, 25 klantaanvragen, 8 reacties en 4 auditlogs toe. De aanvragen gebruiken verschillende categorieën, prioriteiten en statussen.

> **Let op:** het seed-script verwijdert eerst alle bestaande tabellen en gegevens in de database die via `DATABASE_URL` is ingesteld. Gebruik het alleen voor een lege of wegwerpdatabase, niet voor gegevens die je wilt bewaren.

De testaccounts en het standaardwachtwoord staan in [Testaccounts](#testaccounts).

## Testaccounts

De seed data bevat enkele gebruikers. Het standaardwachtwoord is Password123 

Voorbeelden:

- alice.admin@novadesk.local
- amir.agent@novadesk.local
- sara.support@novadesk.local
- victor.viewer@novadesk.local

## Tests
Om geautomatiseerde tests uit te voeren, gebruik het volgende commando: 
```bash
python -m pytest tests/
```

## CI/CD pipeline
Het project gebruikt [GitHub Actions](../.github/workflows/ci-cd.yml). De workflow draait bij pushes naar `main`, versietags in het formaat `vX.Y.Z` en pull requests naar `main`:
1. wordt de code opgehaald en Python geïnstalleerd
2. worden alle dependencies geïnstalleerd
3. worden alle tests automatisch gedraaid via pytest
4. wordt een Docker-image gebouwd; bij pushes en tags wordt het image ook gepubliceerd naar GitHub Container Registry (GHCR). Bij pull requests wordt het image niet gepubliceerd.

## Logs en troubleshooting
- Docker logs bekijken: 
```cmd
docker compose logs -f web
```
- Healthcheck: controleer de status van de applicatie via het endpoint /health, bijvoorbeeld http://localhost:3000/health (bij gebruik van Docker Compose)

## Gekende problemen 
- Formuliervalidatie bij lege invoer https://github.com/MartheVerelstUCLL/it-landscape-project/issues/1
- Geen bevestiging bij aanmaken nieuwe aanvraag https://github.com/MartheVerelstUCLL/it-landscape-project/issues/3
- Zoekfunctie is hoofdlettergevoelig en zoekt enkel op titel https://github.com/MartheVerelstUCLL/it-landscape-project/issues/2
- Viewer ziet klikbare 'Edit'-knop https://github.com/MartheVerelstUCLL/it-landscape-project/issues/5
- Errorlogs hebben geen gestructureerde context en tijdsaanduiding https://github.com/MartheVerelstUCLL/it-landscape-project/issues/4
- Healthcheck is niet compleet genoeg https://github.com/MartheVerelstUCLL/it-landscape-project/issues/8 

## Projectboard
https://github.com/users/MartheVerelstUCLL/projects/2/views/1?system_template=kanban 
## Relevante issues 
https://github.com/MartheVerelstUCLL/it-landscape-project/issues