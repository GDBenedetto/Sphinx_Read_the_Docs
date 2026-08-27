# Analogwerte

Mit `analogRead()` wird ein analoger Eingang gemessen. Der genaue Wertebereich
haengt vom verwendeten Board und seiner ADC-Konfiguration ab.

```cpp
const int sensorPin = A0;

void setup()
{
	Serial.begin(115200);
}

void loop()
{
	int sensorValue = analogRead(sensorPin);
	Serial.println(sensorValue);
	delay(250);
}
```
