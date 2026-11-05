# NovaDesk Request Tracker

Interne demoapplicatie voor het opvolgen van klantaanvragen bij NovaDesk.

Deze starterapp hoort bij de projectopdracht IT Landscape. De applicatie is bewust niet perfect. Je opdracht is om de applicatie professioneel te verkennen, problemen te onderzoeken, issues te maken, Docker/CI/CD op te zetten en documentatie te schrijven.

## Technologie

- Python
- Flask
- SQLite
- Flask-Login
- pytest

## Setup
### Commands voor Linux
```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python3 seed.py
flask run
```
### Stappenplan voor Windows

1. **Open de map in de terminal:** Navigeer naar je projectmap.
Open **Command Prompt** (cmd) of **PowerShell**, en ga naar de map van je project met het commando `cd`:

```cmd
cd pad\naar\je\projectmap

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


6. **Start de Flask-applicatie:**
Zet de Flask-omgevingsvariabele (optioneel, handig voor Windows) en start de server:

```cmd
set FLASK_APP=app.py    (of de naam van je hoofdbestand, optioneel)
flask run

```

*Verificatie:* Je ziet de server draaien op `[http://127.0.0.1:5000](http://127.0.0.1:5000)`.

Op Windows kan het activeren van de virtual environment anders verlopen.

## Testaccounts

De seed data bevat enkele gebruikers. Het standaardwachtwoord is ??? (Vraag maar aan Wim)

Voorbeelden:

- alice.admin@novadesk.local
- amir.agent@novadesk.local
- sara.support@novadesk.local
- victor.viewer@novadesk.local

## Tests

```bash
pytest
```

## Opmerking

Deze documentatie is nog niet volledig. Vul ze aan tijdens de projectopdracht.
