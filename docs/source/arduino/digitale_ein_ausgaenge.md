# Digitale Ein- und Ausgaenge

Digitale Pins arbeiten mit den Zustaenden `HIGH` und `LOW`. Ein Taster
kann als Eingang gelesen und eine LED als Ausgang geschaltet werden.

```cpp
const int ledPin =  LED_BUILTIN;
const int buttonPin = 2;

void setup()
{
	pinMode(ledPin, OUTPUT);
	pinMode(buttonPin, INPUT_PULLUP);
}

void loop()
{
	digitalWrite(ledPin, digitalRead(buttonPin) == LOW ? HIGH : LOW);
}
```
