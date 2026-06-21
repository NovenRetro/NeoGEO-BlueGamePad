# NeoGeo BlueGamepad NR - Dual Mode ESP32

Firmware adaptado por NovenRetro para un mando Neo Geo con:

- DPAD
- Neo Geo A + B + C + D
- START
- SELECT
- LED RGB / dual activo en bajo: azul para BlueGamepad/BlueRetro y rojo para Nintendo Switch
- Cambio de modo manteniendo START + SELECT durante 5 segundos

## Modos

| Modo | LED | Descripción |
| --- | --- | --- |
| BlueGamepad / BlueRetro | Azul | El ESP32 aparece como `NeoGeo BlueGamepad NR`. Pensado para usar con BlueRetro, PC o Android. |
| Nintendo Switch | Rojo | El ESP32 se presenta como mando tipo Pro Controller mediante RetroBlue. |

El primer arranque limpio queda en modo BlueGamepad/BlueRetro. Para cambiar de modo, mantener START + SELECT durante 5 segundos. El ESP32 guarda el último modo en NVS.

## Pinout ESP32 DevKit

Todos los botones son activo-bajo: cada botón debe cerrar entre el GPIO correspondiente y GND. El firmware usa `INPUT_PULLUP`, así que no hace falta resistencia externa de pull-up para las pruebas en DevKit.

| Función Neo Geo | GPIO ESP32 |
| --- | ---: |
| DPAD Arriba | GPIO 13 |
| DPAD Abajo | GPIO 14 |
| DPAD Izquierda | GPIO 16 |
| DPAD Derecha | GPIO 17 |
| A | GPIO 18 |
| B | GPIO 19 |
| C | GPIO 21 |
| D | GPIO 22 |
| START | GPIO 26 |
| SELECT | GPIO 27 |
| LED azul | GPIO 32 |
| LED rojo | GPIO 2 |

### LED activo en bajo

El código está preparado para LED RGB de ánodo común o LED dual cableado activo-bajo:

- ánodo común a 3.3 V
- canal azul a GPIO 32 con resistencia
- canal rojo a GPIO 2 con resistencia
- nivel bajo = LED encendido
- nivel alto = LED apagado

## Mapeo en modo BlueGamepad / BlueRetro

| Físico Neo Geo | HID BlueGamepad |
| --- | --- |
| A | Button 1 |
| B | Button 2 |
| C | Button 3 |
| D | Button 4 |
| SELECT | Button 11 |
| START | Button 12 |
| DPAD | Hat switch |

## Mapeo normal en Nintendo Switch

| Físico Neo Geo | Nintendo Switch |
| --- | --- |
| A | A |
| B | B |
| C | X |
| D | Y |
| START | + |
| SELECT | - |
| DPAD | DPAD |

## Atajos en Nintendo Switch

| Atajo físico | Acción en Switch |
| --- | --- |
| START + A + B | HOME |
| SELECT + DERECHA | Captura |
| SELECT + A | L |
| SELECT + B | R |
| START + SELECT durante 5 segundos | Cambia entre BlueGamepad/BlueRetro y Switch |

Cuando un botón se usa como modificador de atajo, se evita enviar también la acción base del botón para no disparar acciones dobles. Por ejemplo, `SELECT + A` envía `L`, no `-` + `A`.
