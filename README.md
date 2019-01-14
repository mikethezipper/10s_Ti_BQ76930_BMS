# 10s_Ti_BQ76930_BMS
10s Battery Monitor System

Design is for a 6-10s Battery Monitoring system - monitor only. Current rev has no balancing ability - as it will be done on an external board. TI BQ76930 - controlled and communication thru a Wemos D1 Mini Lite - aka ESP8266 or ESP8285

See current board design: 
https://easyeda.com/mikethezipper/10s-ti-cell-monitor
There are several known issues with the board as of 1-10-19 : need to route SDA and SCL i2c lines to D1 and D2
Need to add line from boot pin to MCU (The MCU


Cell qty is selected using jumpers
Boot is done by pressing boot switch

Code is based off of the LibreSolar arduino library:
https://github.com/LibreSolar/bq769x0_ArduinoLibrary

But upon first use it had issues. I will be keeping my code in this repository as it will be based off of the LibreSolar library, but with updates to function on the board and with the selected MCU.

