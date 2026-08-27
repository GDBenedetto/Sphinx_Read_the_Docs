# Eigene technische Dokumentations-Webseite mit Sphinx und GitHub Pages

#### Guide:

- https://www.sphinx-doc.org/en/master/
- https://sphinx-rtd-theme.readthedocs.io/en/stable/

**Page Example:**

- https://docs.sunfounder.com/en/latest/ 
- https://docs.espressif.com/projects/arduino-esp32/en/latest/ 
- https://docs.ros.org/en/humble/index.html

**Tutorial:**

- https://www.youtube.com/watch?v=wPzC1ZTVoJY
---

Diese Anleitung beschreibt, wie du eine technische Dokumentations-Webseite mit dem Sphinx-Layout „Read the Docs“ erstellst, die Quelldateien in GitHub verwaltest und die fertige HTML-Seite kostenlos über GitHub Pages veröffentlichst.

Die Anleitung ist für Windows, Linux und macOS geeignet. Die Befehle funktionieren überwiegend in PowerShell, Windows Terminal oder einer Unix-Shell.

## 1. Ziel und Ergebnis

Am Ende besitzt du:

- ein GitHub-Repository mit deinen Dokumentationsquellen,
- eine Sphinx-Projektstruktur,
- das Layout `sphinx_rtd_theme`,
- Markdown-Unterstützung über `myst-parser`,
- Codeblöcke für Arduino/C++,
- Bilder und Download-Dateien,
- einen automatischen GitHub-Actions-Workflow,
- eine kostenlose Webseite unter einer GitHub-Pages-Adresse.

Die Adresse hat typischerweise dieses Format:

```text
https://DEIN-BENUTZERNAME.github.io/REPOSITORY-NAME/
```

Eine eigene Domain ist nicht erforderlich.

## 2. Benötigte Software

Installiere zunächst:

1. Git: https://git-scm.com/downloads
2. Python 3: https://www.python.org/downloads/
3. Einen Editor, zum Beispiel Visual Studio Code: https://code.visualstudio.com/
4. Ein GitHub-Konto: https://github.com/

Prüfe die Installation:

```bash
git --version
python --version
```

Auf manchen Linux-Systemen lautet der Python-Befehl `python3`:

```bash
python3 --version
```

Unter Windows sollte Python beim Installieren mit der Option `Add Python to PATH` zum Suchpfad hinzugefügt werden.

## 3. GitHub-Repository erstellen

Melde dich bei GitHub an und erstelle ein neues Repository.

Beispiel:

```text
Name: arduino-dokumentation
Sichtbarkeit: Public oder Private
README: optional
.gitignore: Python
Lizenz: nach Bedarf
```

Für eine öffentliche Lehrdokumentation ist `Public` praktisch. Eine private Dokumentation kann nur mit passenden GitHub-Einstellungen genutzt werden; für eine kostenlose öffentlich erreichbare GitHub-Pages-Seite ist ein öffentliches Repository die unkomplizierteste Variante.

Kopiere die HTTPS-Adresse des Repositorys. Sie sieht ungefähr so aus:

```text
https://github.com/DEIN-BENUTZERNAME/arduino-dokumentation.git
```

## 4. Lokales Arbeitsverzeichnis erstellen

Erstelle einen Ordner und klone das Repository:

```bash
git clone https://github.com/DEIN-BENUTZERNAME/arduino-dokumentation.git
cd arduino-dokumentation
```

Wenn das Repository leer ist, kannst du auch direkt in einem neuen Ordner arbeiten:

```bash
mkdir arduino-dokumentation
cd arduino-dokumentation
git init
git branch -M main
git remote add origin https://github.com/DEIN-BENUTZERNAME/arduino-dokumentation.git
```

## 5. Python-virtuelle Umgebung einrichten

Eine virtuelle Umgebung verhindert, dass die Sphinx-Pakete andere Python-Projekte beeinflussen.

### Windows PowerShell

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
```

Falls PowerShell die Aktivierung blockiert, kann einmalig folgender Befehl erforderlich sein:

```powershell
Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
```

Danach erneut:

```powershell
.venv\Scripts\Activate.ps1
```

### Windows Eingabeaufforderung

```bat
python -m venv .venv
.venv\Scripts\activate.bat
```

### Linux oder macOS

```bash
python3 -m venv .venv
source .venv/bin/activate
```

Eine aktivierte Umgebung ist meistens an `(.venv)` am Anfang der Eingabezeile zu erkennen.

## 6. Sphinx und Erweiterungen installieren

Installiere Sphinx, das Read-the-Docs-Theme und den Markdown-Parser:

```bash
python -m pip install --upgrade pip
python -m pip install sphinx sphinx-rtd-theme myst-parser
```

Für eine reproduzierbare Umgebung speicherst du die Pakete:

```bash
python -m pip freeze > docs/requirements.txt
```

Wenn der Ordner `docs` noch nicht existiert, erstelle ihn vorher:

```bash
mkdir docs
python -m pip freeze > docs/requirements.txt
```

Eine bewusst minimale `docs/requirements.txt` ist meist übersichtlicher:

```text
sphinx
sphinx-rtd-theme
myst-parser
```

## 7. Sphinx-Projekt erzeugen

Erstelle ein Sphinx-Projekt im Ordner `docs`:

```bash
sphinx-quickstart docs
```

Beantworte die Fragen beispielsweise so:

```text
Separate source and build directories: yes
Project name: Arduino- und ESP32-Dokumentation
Author name: Dein Name
Project release: 1.0
Project language: de
```

Bei einer getrennten Struktur liegen die Quellen danach normalerweise unter:

```text
docs/source/
```

und die erzeugten Dateien unter:

```text
docs/build/
```

## 8. Empfohlene technische Struktur

Eine sinnvolle Struktur für eine Arduino-/ESP32-Dokumentation ist:

```text
arduino-dokumentation/
│
├── .github/
│   └── workflows/
│       └── sphinx.yml
│
├── docs/
│   ├── requirements.txt
│   ├── Makefile
│   ├── make.bat
│   │
│   ├── source/
│   │   ├── conf.py
│   │   ├── index.rst
│   │   │
│   │   ├── _static/
│   │   │   ├── custom.css
│   │   │   └── images/
│   │   │       └── schaltung.png
│   │   │
│   │   ├── _templates/
│   │   │
│   │   ├── downloads/
│   │   │   └── beispielprogramm.zip
│   │   │
│   │   ├── grundlagen/
│   │   │   ├── index.rst
│   │   │   ├── installation.md
│   │   │   └── erstes_programm.md
│   │   │
│   │   ├── arduino/
│   │   │   ├── index.rst
│   │   │   ├── digitale_ein_ausgaenge.md
│   │   │   ├── analogwerte.md
│   │   │   └── pwm.md
│   │   │
│   │   ├── esp32/
│   │   │   ├── index.rst
│   │   │   ├── wlan.md
│   │   │   └── webserver.md
│   │   │
│   │   └── projekte/
│   │       ├── index.rst
│   │       └── temperaturmessung.md
│   │
│   └── build/
│       └── html/
│
├── .gitignore
├── README.md
└── LICENSE
```

Der Ordner `docs/build/` enthält generierte Dateien. Er wird normalerweise nicht in GitHub gespeichert. Die Quelldateien befinden sich in `docs/source/`.

## 9. Konfiguration in `conf.py`

Öffne:

```text
docs/source/conf.py
```

Verwende zunächst diese Konfiguration:

```python
from datetime import datetime

project = "Arduino- und ESP32-Dokumentation"
author = "Dein Name"
year = datetime.now().year
copyright = f"{year}, {author}"
release = "1.0"

extensions = [
    "myst_parser",
]

templates_path = ["_templates"]
exclude_patterns = []

language = "de"

html_theme = "sphinx_rtd_theme"
html_title = "Arduino- und ESP32-Dokumentation"
html_static_path = ["_static"]
html_logo = "_static/images/logo.png"
html_favicon = "_static/images/favicon.ico"

html_theme_options = {
    "navigation_depth": 4,
    "collapse_navigation": False,
    "sticky_navigation": True,
    "includehidden": True,
}

html_css_files = [
    "custom.css",
]

rst_prolog = r"""
.. role:: cpp(code)
   :language: cpp
"""
```

Wenn du noch kein Logo und kein Favicon besitzt, kommentiere diese Zeilen aus:

```python
# html_logo = "_static/images/logo.png"
# html_favicon = "_static/images/favicon.ico"
```

## 10. Startseite mit reStructuredText

Öffne:

```text
docs/source/index.rst
```

Ersetze den Inhalt durch:

```rst
Arduino- und ESP32-Dokumentation
================================

Willkommen zu meiner technischen Dokumentation über Mikrocontroller,
Arduino, ESP32 und eingebettete Systeme.

Diese Webseite enthält Grundlagen, Programmierbeispiele, Schaltungen
und vollständige technische Projekte.

.. note::

   Die Dokumentation befindet sich im Aufbau. Beispiele sollten vor dem
   praktischen Einsatz an die konkrete Hardware angepasst werden.

Inhalte
-------

.. toctree::
   :maxdepth: 2
   :caption: Dokumentation:

   grundlagen/index
   arduino/index
   esp32/index
   projekte/index

Weitere Bereiche
----------------

* :ref:`genindex`
* :ref:`search`
```

Die Direktive `toctree` erzeugt die Navigation. Die Dateiendung muss bei den Einträgen normalerweise nicht angegeben werden.

## 11. Kapiteldateien erstellen

Erstelle zunächst:

```text
docs/source/grundlagen/index.rst
```

```rst
Grundlagen
==========

.. toctree::
   :maxdepth: 2

   installation
   erstes_programm
```

Erstelle danach:

```text
docs/source/grundlagen/installation.md
```

```markdown
# Installation

## Benötigte Software

Für die ersten Beispiele werden folgende Programme benötigt:

- Arduino IDE
- USB-Treiber für das verwendete Board
- Git für die Verwaltung der Beispiele

## Projektablauf

1. Board anschließen.
2. Passendes Board in der Arduino IDE auswählen.
3. Port auswählen.
4. Beispiel kompilieren.
5. Programm auf das Board übertragen.
```

Erstelle:

```text
docs/source/grundlagen/erstes_programm.md
```

````markdown
# Erstes Arduino-Programm

Das folgende Programm sendet regelmäßig eine Meldung über die serielle
Schnittstelle.

```cpp
void setup()
{
    Serial.begin(115200);
}

void loop()
{
    Serial.println("Hallo Arduino");
    delay(1000);
}
```

Öffne anschließend den seriellen Monitor mit einer Baudrate von `115200`.
````

## 12. Arduino- und ESP32-Navigation

Erstelle:

```text
docs/source/arduino/index.rst
```

```rst
Arduino
=======

.. toctree::
   :maxdepth: 2

   digitale_ein_ausgaenge
   analogwerte
   pwm
```

Erstelle beispielsweise:

```text
docs/source/arduino/pwm.md
```

````markdown
# PWM mit Arduino

PWM steht für Pulsweitenmodulation. Damit lässt sich beispielsweise die
Helligkeit einer LED oder die Drehzahl eines geeigneten Motors steuern.

## Beispiel

```cpp
const int ledPin = 9;

void setup()
{
    pinMode(ledPin, OUTPUT);
}

void loop()
{
    analogWrite(ledPin, 128);
}
```

## Codebeschreibung

- `pinMode()` konfiguriert den Anschluss als Ausgang.
- `analogWrite()` erzeugt ein PWM-Signal.
- Der Wert `128` liegt bei einer 8-Bit-PWM ungefähr in der Mitte des Bereichs.
````

Erstelle:

```text
docs/source/esp32/index.rst
```

```rst
ESP32
=====

.. toctree::
   :maxdepth: 2

   wlan
   webserver
```

## 13. Codeblöcke richtig darstellen

In Markdown wird die Sprache nach drei Backticks angegeben:

````markdown
```cpp
void setup() {
    Serial.begin(115200);
}
```
````

In reStructuredText verwendest du:

```rst
.. code-block:: cpp

   void setup()
   {
       Serial.begin(115200);
   }
```

Andere nützliche Sprachen sind:

```text
cpp
c
python
bash
json
yaml
text
```

## 14. Hinweise, Warnungen und wichtige Informationen

Sphinx stellt verschiedene Hinweise bereit:

```rst
.. note::

   Dies ist ein allgemeiner Hinweis.

.. tip::

   Dieser Tipp erleichtert die Fehlersuche.

.. warning::

   Vor dem Verdrahten die Stromversorgung abschalten.

.. important::

   Die Versorgungsspannung des konkreten Boards prüfen.
```

In MyST-Markdown kannst du dieselben Sphinx-Direktiven verwenden:

```markdown
```{warning}
Vor dem Anschließen die Versorgungsspannung prüfen.
```
```

## 15. Bilder hinzufügen

Lege Bilder beispielsweise hier ab:

```text
docs/source/_static/images/
```

Beispiel für reStructuredText:

```rst
.. image:: _static/images/schaltung.png
   :alt: Schaltung mit Arduino und LED
   :width: 600px
   :align: center
```

Beispiel für Markdown:

```markdown
![Schaltung mit Arduino und LED](_static/images/schaltung.png)
```

Verwende möglichst verständliche Dateinamen ohne Leerzeichen:

```text
arduino-pwm-schaltung.png
esp32-wlan-aufbau.jpg
```

## 16. Downloads verlinken

Lege Dateien hier ab:

```text
docs/source/downloads/
```

In reStructuredText:

```rst
:download:`Arduino-Beispiel herunterladen <downloads/arduino-beispiel.zip>`
```

In Markdown:

```markdown
[Arduino-Beispiel herunterladen](downloads/arduino-beispiel.zip)
```

## 17. Eigene CSS-Anpassungen

Das Standardlayout solltest du zunächst unverändert testen. Kleine Anpassungen kannst du später in folgender Datei vornehmen:

```text
docs/source/_static/custom.css
```

Beispiel:

```css
.wy-nav-content {
    max-width: 1100px;
}

.highlight {
    border-radius: 4px;
}

h1, h2, h3 {
    letter-spacing: 0.01em;
}
```

Die Datei muss in `conf.py` registriert sein:

```python
html_css_files = [
    "custom.css",
]
```

Ändere das Theme möglichst nicht direkt in der installierten Python-Umgebung. Eigene CSS-Regeln sind wartbarer und bleiben bei einem Update des Themes erhalten.

## 18. Lokalen HTML-Build durchführen

Aktiviere deine virtuelle Umgebung und führe aus:

```bash
sphinx-build -b html docs/source docs/build/html
```

Die fertige Startseite liegt anschließend hier:

```text
docs/build/html/index.html
```

Öffne sie direkt im Browser oder starte einen lokalen Server:

```bash
python -m http.server 8000 --directory docs/build/html
```

Öffne danach:

```text
http://localhost:8000
```

Alternativ kannst du die mit `sphinx-quickstart` erzeugten Befehle verwenden:

Linux/macOS:

```bash
make -C docs html
```

Windows:

```bat
docs\make.bat html
```

## 19. Sphinx-Warnungen kontrollieren

Während des Builds meldet Sphinx fehlende Seiten, falsche Links und andere Probleme. Für eine strenge Prüfung kannst du verwenden:

```bash
sphinx-build -W -b html docs/source docs/build/html
```

`-W` behandelt Warnungen als Fehler. Das ist besonders nützlich im automatischen GitHub-Actions-Build.

Ein häufiger Fehler ist ein Eintrag in `toctree`, für den keine Datei existiert. Wenn in `index.rst` beispielsweise steht:

```rst
arduino/pwm
```

muss diese Datei vorhanden sein:

```text
docs/source/arduino/pwm.md
```

oder:

```text
docs/source/arduino/pwm.rst
```

## 20. `.gitignore` erstellen

Erstelle im Hauptverzeichnis:

```text
.gitignore
```

Mit diesem Inhalt:

```gitignore
.venv/
__pycache__/
*.pyc

# Sphinx-Build-Ergebnisse
docs/build/

# Editor- und Betriebssystemdateien
.vscode/
.idea/
.DS_Store
Thumbs.db
```

Die virtuelle Python-Umgebung und die lokal erzeugten HTML-Dateien müssen normalerweise nicht in Git gespeichert werden.

## 21. `README.md` erstellen

Im Hauptverzeichnis kannst du folgende Datei anlegen:

```markdown
# Arduino- und ESP32-Dokumentation

Technische Dokumentation zu Arduino, ESP32, Embedded C/C++ und
Mikrocontroller-Projekten.

Die veröffentlichte Dokumentation ist über GitHub Pages erreichbar.

## Lokal bauen

```bash
python -m pip install -r docs/requirements.txt
sphinx-build -b html docs/source docs/build/html
```
```

## 22. Dateien erstmals zu Git hinzufügen

Prüfe den Status:

```bash
git status
```

Füge die Dateien hinzu:

```bash
git add .
```

Erstelle einen Commit:

```bash
git commit -m "Initiale Sphinx-Dokumentation erstellen"
```

Übertrage ihn zu GitHub:

```bash
git push -u origin main
```

Wenn Git nach deinen Zugangsdaten fragt, verwende je nach GitHub-Konfiguration einen Personal Access Token oder GitHub Desktop.

## 23. GitHub-Actions-Workflow erstellen

Erstelle die Datei:

```text
.github/workflows/sphinx.yml
```

Verwende folgenden Workflow:

```yaml
name: Build and deploy Sphinx documentation

on:
  push:
    branches:
      - main
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Repository auschecken
        uses: actions/checkout@v4

      - name: Python einrichten
        uses: actions/setup-python@v5
        with:
          python-version: "3.x"

      - name: Python-Abhängigkeiten installieren
        run: |
          python -m pip install --upgrade pip
          pip install -r docs/requirements.txt

      - name: Sphinx bauen
        run: |
          sphinx-build -W -b html docs/source docs/build/html

      - name: GitHub Pages konfigurieren
        uses: actions/configure-pages@v5

      - name: HTML als Pages-Artefakt hochladen
        uses: actions/upload-pages-artifact@v3
        with:
          path: docs/build/html

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}

    runs-on: ubuntu-latest
    needs: build

    steps:
      - name: Auf GitHub Pages veröffentlichen
        id: deployment
        uses: actions/deploy-pages@v4
```

Der Workflow verwendet GitHub Actions, um die statischen Dateien zu bauen und mit GitHub Pages zu veröffentlichen. GitHub dokumentiert dafür eigene Workflows und die erforderlichen Pages-Berechtigungen.

## 24. GitHub Pages aktivieren

Nachdem du den Workflow übertragen hast:

1. Öffne dein Repository auf GitHub.
2. Wähle `Settings`.
3. Öffne links `Pages`.
4. Wähle als Quelle `GitHub Actions`.
5. Öffne den Bereich `Actions`.
6. Prüfe, ob der Workflow erfolgreich abgeschlossen wurde.

GitHub Pages kann entweder aus einem Branch oder über einen benutzerdefinierten GitHub-Actions-Workflow veröffentlichen. Für Sphinx ist der Workflow-Ansatz empfehlenswert, weil die HTML-Dateien automatisch bei jedem Push erzeugt werden.

Die Webseite ist anschließend typischerweise hier erreichbar:

```text
https://DEIN-BENUTZERNAME.github.io/arduino-dokumentation/
```

## 25. Standard-Arbeitsablauf bei Änderungen

Der normale Ablauf ist:

1. Virtuelle Umgebung aktivieren.
2. Markdown- oder reStructuredText-Datei ändern.
3. Webseite lokal bauen.
4. Warnungen und Links prüfen.
5. Änderung mit Git committen.
6. Änderung zu GitHub pushen.
7. GitHub Actions und anschließend die Pages-Webseite prüfen.

Beispiel:

```bash
source .venv/bin/activate
sphinx-build -W -b html docs/source docs/build/html
git add .
git commit -m "PWM-Kapitel ergänzen"
git push
```

Unter Windows PowerShell ersetzt du die Aktivierung durch:

```powershell
.venv\Scripts\Activate.ps1
```

## 26. Git-Befehle im Überblick

```bash
git status
git add .
git commit -m "Beschreibung der Änderung"
git push
```

Neue Änderungen aus GitHub holen:

```bash
git pull
```

Die Historie ansehen:

```bash
git log --oneline
```

Einen Versionsstand markieren:

```bash
git tag v1.0
git push origin v1.0
```

## 27. Mehrere Dokumentationsbereiche

Für umfangreiche Lehrmaterialien ist diese Aufteilung sinnvoll:

```text
source/
├── index.rst
├── grundlagen/
├── arduino/
├── esp32/
├── elektronik/
├── embedded-cpp/
├── fehlerbehebung/
├── projekte/
└── glossar.rst
```

Beispiele für Kapitel:

- Grundlagen digitaler Ein- und Ausgänge.
- Analog-Digital-Wandler.
- PWM und Motorsteuerung.
- UART, I2C und SPI.
- ESP32-WLAN und Webserver.
- Stromversorgung und Pegelwandler.
- Fehlersuche mit serieller Ausgabe.
- Messungen mit Multimeter und Oszilloskop.
- Beispielprojekte für Studierende.

## 28. Querverweise zwischen Seiten

In reStructuredText kannst du Labels setzen:

```rst
.. _pwm-grundlagen:

PWM-Grundlagen
==============
```

Auf diese Stelle verweist du so:

```rst
Weitere Informationen findest du unter :ref:`pwm-grundlagen`.
```

In MyST-Markdown kannst du auch Labels verwenden:

```markdown
(sec-pwm)=
# PWM-Grundlagen
```

Verweis:

```markdown
Siehe [PWM-Grundlagen](#sec-pwm).
```

## 29. API- oder Quellcode-Dokumentation

Wenn du später C/C++-Bibliotheken oder Python-Code dokumentieren möchtest, brauchst du zusätzliche Erweiterungen. Für die erste Version deiner Arduino-Dokumentation ist es einfacher, den Quellcode manuell in gut erklärten Codeblöcken zu präsentieren.

Eine didaktisch gute Beispielseite enthält:

1. Lernziel.
2. Benötigte Hardware.
3. Schaltplan oder Anschlussübersicht.
4. Vollständigen Code.
5. Erklärung der wichtigsten Codeabschnitte.
6. Erwartetes Verhalten.
7. Häufige Fehler.
8. Erweiterungsaufgabe.

## 30. Versionsverwaltung der Dokumentation

GitHub Pages veröffentlicht zunächst eine Version. Für mehrere Versionen wie `v1.0`, `v2.0` und `latest` brauchst du zusätzliche Automatisierung.

Mögliche Lösungen sind:

- verschiedene Branches und separate Deployments,
- Git-Tags mit einem Versionsworkflow,
- Read the Docs mit aktivierter Versionsverwaltung,
- ein Versionsverzeichnis wie `v1/` und `v2/`.

Für den Start genügt eine aktuelle Version. Versionierung solltest du einführen, wenn sich deine Arduino- oder ESP32-Beispiele tatsächlich inkompatibel ändern.

## 31. Eigene Domain später hinzufügen

Eine eigene Domain ist nicht notwendig. Wenn du später eine Domain besitzt, kannst du sie in den GitHub-Pages-Einstellungen hinterlegen und eine DNS-Konfiguration vornehmen.

Bis dahin verwendest du einfach die kostenlose GitHub-Pages-Adresse.

## 32. Typische Fehler

### `sphinx-build` wird nicht gefunden

Aktiviere zuerst die virtuelle Umgebung:

```bash
source .venv/bin/activate
```

Oder installiere über den aktiven Python-Interpreter:

```bash
python -m pip install sphinx sphinx-rtd-theme myst-parser
```

### Ein Eintrag im Inhaltsverzeichnis fehlt

Prüfe, ob der Pfad in `toctree` exakt zum Dateinamen passt. Groß- und Kleinschreibung ist insbesondere in GitHub-Actions unter Linux wichtig.

### Das CSS oder Logo wird nicht angezeigt

Prüfe:

- liegt die Datei wirklich unter `source/_static/`?
- ist `html_static_path = ["_static"]` gesetzt?
- stimmt der Dateiname exakt?
- verwendet der Pfad Schrägstriche `/`?

### Der GitHub-Actions-Build schlägt fehl

Öffne im Repository den Bereich `Actions`, wähle den fehlgeschlagenen Lauf und lies die erste konkrete Fehlermeldung. Häufige Ursachen sind:

- fehlende Datei in `requirements.txt`,
- falscher `toctree`-Eintrag,
- falsche Einrückung in YAML,
- Groß-/Kleinschreibung eines Dateinamens,
- Warnungen durch `sphinx-build -W`.

Wenn die strenge Warnungsprüfung am Anfang zu viele Probleme verursacht, kannst du vorübergehend bauen mit:

```yaml
sphinx-build -b html docs/source docs/build/html
```

Später solltest du `-W` wieder aktivieren.

## 33. Minimaler Startumfang

Für den ersten funktionierenden Prototyp benötigst du nur diese Dateien:

```text
arduino-dokumentation/
├── .github/
│   └── workflows/
│       └── sphinx.yml
├── docs/
│   ├── requirements.txt
│   └── source/
│       ├── conf.py
│       └── index.rst
├── .gitignore
└── README.md
```

Beginne mit einer Startseite und zwei Kapiteln. Ergänze Navigation, Bilder, Downloads und CSS erst, wenn der erste Build funktioniert.

## 34. Empfohlene Konfiguration für dein Vorhaben

Für eine deutschsprachige Arduino-/ESP32-Lehrdokumentation ist dieser Stack besonders passend:

```text
Sphinx
├── sphinx_rtd_theme
├── myst-parser
├── Markdown für Lehrtexte
├── reStructuredText für Navigation
├── GitHub für Quellen und Versionierung
├── GitHub Actions für den automatischen Build
└── GitHub Pages für das Hosting
```

Du kannst damit Markdown-Dateien verwenden, wie du sie bereits aus Marp kennst, und gleichzeitig das professionelle Sphinx-Navigationslayout nutzen.

## 35. Checkliste

### Vorbereitung

- [ ] Git installiert.
- [ ] Python installiert.
- [ ] GitHub-Repository erstellt.
- [ ] Lokales Repository geklont.
- [ ] Virtuelle Python-Umgebung erstellt.

### Sphinx

- [ ] Sphinx installiert.
- [ ] `sphinx-rtd-theme` installiert.
- [ ] `myst-parser` installiert.
- [ ] `conf.py` konfiguriert.
- [ ] `html_theme = "sphinx_rtd_theme"` gesetzt.
- [ ] `index.rst` erstellt.
- [ ] `toctree` geprüft.

### Inhalte

- [ ] Grundlagenkapitel erstellt.
- [ ] Arduino-Kapitel erstellt.
- [ ] ESP32-Kapitel erstellt.
- [ ] Codeblöcke eingefügt.
- [ ] Bilder und Downloads getestet.
- [ ] Links geprüft.

### Veröffentlichung

- [ ] `.github/workflows/sphinx.yml` erstellt.
- [ ] Dateien nach GitHub gepusht.
- [ ] GitHub Pages auf `GitHub Actions` gestellt.
- [ ] Workflow erfolgreich ausgeführt.
- [ ] Webseite im Browser getestet.

## 36. Ergebnis

Mit dieser Struktur besitzt du eine wartbare technische Dokumentation, deren Inhalte in GitHub liegen und deren Webseite automatisch aus den Quellen erzeugt wird. Das Layout basiert auf dem offiziellen Read-the-Docs-Sphinx-Theme; du kannst es zunächst unverändert nutzen und später mit Logo, CSS, Kapiteln und Versionsverwaltung erweitern.
