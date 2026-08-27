# Webserver

Ein ESP32 kann Messwerte oder Steuerungen ueber eine kleine Webseite
bereitstellen. Die konkrete Netzwerkkonfiguration haengt vom Projekt ab.

```cpp
#include <WebServer.h>

WebServer server(80);

void handleRoot()
{
	server.send(200, "text/plain", "ESP32 ist erreichbar");
}

void setup()
{
	server.on("/", handleRoot);
	server.begin();
}

void loop()
{
	server.handleClient();
}
```

```{note}
Die Route `/` liefert hier nur eine einfache Textantwort. Eine vollstaendige
Anwendung kann dort HTML oder JSON ausgeben.
```
