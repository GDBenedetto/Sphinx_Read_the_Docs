# WLAN mit ESP32

Der ESP32 kann sich mit einem vorhandenen WLAN verbinden. Trage die
Zugangsdaten nur lokal ein und uebertrage sie nicht in ein oeffentliches
Repository.

```cpp
#include <WiFi.h>

const char* ssid = "MEIN_WLAN";
const char* password = "MEIN_PASSWORT";

void setup()
{
	Serial.begin(115200);
	WiFi.begin(ssid, password);
	while (WiFi.status() != WL_CONNECTED) {
		delay(500);
		Serial.print(".");
	}
	Serial.println(WiFi.localIP());
}

void loop()
{
}
```

```{warning}
Verwende fuer echte Projekte keine Zugangsdaten direkt im versionierten Quelltext.
```
