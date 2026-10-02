# ESPHome Growatt Modbus

ESPHome-Integration für Growatt-Hybridwechselrichter über RS485/Modbus RTU. Sie stellt native Home-Assistant-Entities bereit, liest den Growatt-Smartmeter über Function Code `0x20`, erkennt einen nicht erreichbaren Wechselrichter und verifiziert Schreibbefehle per Rücklesen.

> Community-Projekt, nicht von Growatt unterstützt oder autorisiert. Schreibzugriffe können Lade- und Betriebsparameter verändern. Registeradressen müssen zum verwendeten Wechselrichter und Firmwarestand passen.

## Funktionen

- native ESPHome-API, kein MQTT erforderlich
- Wechselrichter-, PV-, Netz- und Batteriewerte
- Batterie-Lade-/Entladeparameter
- AC-Charging-Schalter
- neun Growatt-Zeit-/Prioritätsfenster
- Smartmeter-Abfrage über Growatt FC32 / `0x20`
- Heartbeat vor der vollständigen Abfrage
- Werte werden in HA ungültig, wenn der Wechselrichter offline ist
- Write → Readback → Verify mit Wiederholungen
- korrigierte FC32-ByteCount-Auswertung

## Getestete Hardware

Dieses Projekt wurde auf einem **Waveshare ESP32-S3-Relay-6CH** entwickelt und getestet. Das Modul eignet sich für diesen Einsatzzweck besonders gut, da ESP32-S3, isolierte RS485-Schnittstelle, Weitbereichs-DC-Versorgung und sechs Relais in einem für die Hutschienenmontage geeigneten Modul kombiniert sind.

- **Board:** Waveshare ESP32-S3-Relay-6CH
- **Verwendetes Produkt:** [Amazon.de – B0FNVWFZ4Z](https://www.amazon.de/dp/B0FNVWFZ4Z)
- **Herstellerdokumentation:** [Waveshare ESP32-S3-Relay-6CH Dokumentation](https://docs.waveshare.com/ESP32-S3-Relay-6CH)
- **ESPHome:** getestet mit 2026.9.1
- **Growatt-Kommunikation:** Modbus RTU, Slave-Adresse `1`
- **UART:** 38400 Baud, 8N1
- **RS485-Pins des getesteten Waveshare-Moduls:** GPIO17 TX / GPIO18 RX

> **Platzhalter für Hardware-Foto**  
> Hier wird noch ein Foto des installierten Waveshare ESP32-S3-Relay-6CH / Growatt-Controllers ergänzt.
> Vorgeschlagene Datei: `docs/images/waveshare-growatt-controller.jpg`

Über die Substitutions kann das Package auch an andere ESP32-S3-/RS485-Hardware angepasst werden. Entwickelt und getestet wurde es jedoch mit dem Waveshare ESP32-S3-Relay-6CH. Bei anderen Growatt-Modellen oder Firmwareständen können außerdem Register abweichen. Bestätigte Kombinationen können gerne über Issues gemeldet werden.

## Installation

Am einfachsten kopierst du [`examples/growatt-example.yaml`](examples/growatt-example.yaml) in dein ESPHome-Verzeichnis, legst anhand von [`examples/secrets.example.yaml`](examples/secrets.example.yaml) eine lokale `secrets.yaml` an und passt Pins, Geräteadresse und Gerätenamen an.

Das Beispiel lädt die Dateien direkt von `https://github.com/LogixDE/esphome-growatt-modbus` und ist auf `v1.0.0` festgesetzt. Dadurch verändert ein späteres Update von `main` nicht ungefragt eine funktionierende Installation.

## Standard-Pins

| ESP32-S3 | RS485 | Funktion |
|---|---|---|
| GPIO17 | DI / TX | Senden |
| GPIO18 | RO / RX | Empfangen |
| GPIO21 | DE + /RE | Richtungssteuerung |
| GND | GND | gemeinsame Masse |

## Schreibbefehle

Schreibbare Einstellungen werden nicht nur gesendet. Der Wert wird anschließend wieder aus dem Wechselrichter gelesen und geprüft. Bei einer Abweichung wird wiederholt; bei endgültigem Fehlschlag wird in Home Assistant wieder der tatsächlich gelesene Wert angezeigt.

## Offline-Verhalten

Register 0 dient als Heartbeat. Antwortet der Wechselrichter nicht, werden die umfangreichen Folgeabfragen nicht gestartet. `Growatt Modbus Online` geht auf aus und abhängige Werte werden ungültig, damit Home Assistant keine alten Werte als aktuelle Messung darstellt.

## Smartmeter / FC32

Der Smartmeter wird über den Growatt-spezifischen Function Code `0x20` gelesen. Die Antwort enthält vor den Nutzdaten ein ByteCount-Feld, das vor der 32-Bit-Auswertung entfernt wird.

## Mitarbeit

Fehlerberichte und bestätigte Registerinformationen für weitere Growatt-Modelle sind willkommen. Bitte Wechselrichtermodell, Firmwarestand, ESPHome-Version und passende DEBUG-Logauszüge angeben.

## Lizenz

MIT, siehe [`LICENSE`](LICENSE).
