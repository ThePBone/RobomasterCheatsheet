<h1 align="center">
  Robomaster Cheatsheet
  <br>
</h1>
<p align="center">
  <a href="#verbindungsaufbau">Verbindungsaufbau</a> •
  <a href="#chassis-api">Chassis</a> •
  <a href="#robotic-arm-api">Robotic arm</a> •
  <a href="#gripper-api">Gripper</a> •
  <a href="#distance-sensor-api">Sensor</a> •
  <a href="#vision-api">Camera</a> •
  <a href="#led-api">LED</a>
</p>


## Verbindungsaufbau

```python
from robomaster import robot

if __name__ == '__main__':
    robot = robot.Robot()
    robot.initialize(conn_type="sta", sn="3JKDH2T001ULTD")
	# [...]
    robot.close()
```

## Schnittstellen

### Chassis API

Kommentierte Beispielskripte:
* [Einfache Bewegungen](chassis.py)
* [Rechteck mit Drehung](rechteck.py)

| Funktion | Beschreibung |
| ------------------------------------------------------------ | ---- |
| `drive_speed(x, y, z, timeout)` | Fahre mit best. Geschwindigkeit; `x`,`y` = m/s; `z` = °/s; `timeout` = s |
| `move(x, y, z, xy_speed, z_speed)` | Fahre best. Länge;  `x`,`y` = m; `z` = Grad °; `xy_speed` = m/s; `z_speed` = °/s |
| `drive_wheels(w1, w2, w3, w4, timeout)` | Bestimme Geschwindigkeiten der Räder; `w1..w4` = rpm |

### Robotic arm API

| Funktion       | Beschreibung                        |
| -------------- | ----------------------------------- |
| `move(x, y)`   | Relative Armbewegung; `x`, `y` = mm |
| `moveto(x, y)` | Absolute Armbewegung; `x`, `y` = mm |
| `recenter()`   | Arm zur Mitte zurückbewegen         |

### Gripper API

Beispielskript, welches ein Objekt greift, transportiert und ablegt: [`gripper.py`](gripper.py)

| Funktion       | Beschreibung                         |
| -------------- | ------------------------------------ |
| `open(power)`  | Öffne Gripper; `power` = [1, 100]    |
| `close(power)` | Schließe Gripper; `power` = [1, 100] |
| `pause()`      | Stoppe Bewegung                      |


### Distance sensor API

Beispielskript, welches den Sensor zum Bremsen verwendet: [`sensor-drive.py`](sensor-drive.py)

| Funktion       | Beschreibung                         |
| -------------- | ------------------------------------ |
| `sub_distance(hz, cb)`  | Entfernungsdaten abonnieren; `hz` = Rate, `cb` = Callback-Funktion (Int-Array mit je 4 Werten als Parameter) |
| `unsub_distance()` | Entfernungsdaten deabonnieren|

### Vision API

Beispielskript, welches das Vehalten des Roboters (sowie die LEDs) je nach Marker ändert: [`camera-marker.py`](camera-marker.py)

| Funktion       | Beschreibung                         |
| -------------- | ------------------------------------ |
| `sub_detect_info(name, color, cb)`  | Bilderkennungsdaten abonnieren; `name` = Modus (siehe Tabelle unten), `color` = Farbe der Marke/Linie, `cb` = Callback-Funktion |
| `unsub_detect_info(name)` | Bilderkennungsdaten deabonnieren; `name` = Modus  |

| Modus       | Callback-Parameter | Beschreibung                         |
| -------------- | -------------- | ------------------------------------ |
| `person`  | `[x, y, Breite, Höhe]` | Personenerkennung |
| `gesture` | `[x, y, Breite, Höhe]` | Gestenerkennung |
| `line`    | `[x, y, Tangentenwinkel (theta), Krümmung]` | Linienerkennung |
| `marker`  | `[x, y, Breite, Höhe, Name d. Markers]` | Markererkennung |
| `robot`  | `[x, y, Breite, Höhe]` | Robotererkennung |

### LED API

| Funktion       | Beschreibung                         |
| -------------- | ------------------------------------ |
| `set_led(comp, r, g, b, effect, freq)` | Setze LED-Farbe; `comp` = Bereich, `r,g,b` = Farbe; `effect` = Lichteffekt; `freq` = Frequenz für `flash`-Effekt |

Mögliche Bereiche: `all, top_all, bottom_left, bottom_right, ...`

Mögliche Effekte: `on, off, flash, breath, scrolling`
_________

Quelle: <https://robomaster-dev.readthedocs.io/>
