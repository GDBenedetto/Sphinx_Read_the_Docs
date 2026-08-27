# Erstes Arduino-Programm

Das folgende Programm sendet regelmaessig eine Meldung ueber die serielle
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

Oeffne anschliessend den seriellen Monitor mit einer Baudrate von `115200`.
