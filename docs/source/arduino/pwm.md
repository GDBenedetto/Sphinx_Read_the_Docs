# PWM mit Arduino

PWM steht fuer Pulsweitenmodulation. Damit laesst sich beispielsweise die
Helligkeit einer LED oder die Drehzahl eines geeigneten Motors steuern.

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

`pinMode()` konfiguriert den Anschluss als Ausgang. `analogWrite()` erzeugt
das PWM-Signal; der Wert `128` liegt bei einer 8-Bit-PWM ungefaehr in der Mitte.
