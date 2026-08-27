# Arduino- und ESP32-Dokumentation

Technische Dokumentation zu Arduino, ESP32, Embedded C/C++ und
Mikrocontroller-Projekten.

## Lokal bauen

Virtuelle Umgebung aktivieren und Abhängigkeiten installieren:

```bash
source .venv/bin/activate
python -m pip install -r docs/requirements.txt
```

HTML-Dokumentation erzeugen:

```bash
sphinx-build -W -b html docs/source docs/build/html
```

Die fertige Startseite liegt anschließend unter
`docs/build/html/index.html`.