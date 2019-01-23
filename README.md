# bq769x0 Arduino Library

Note - Testing only done on BQ7693003 WITH CRC - holy damn, check if your chip requires CRC, it makes everything a pain in the ass

Arduino-compatible library for battery management system based on Texas Instruments bq769x0 IC (bq76920, bq76930 and bq76940).

The library offerst most features for a simple BMS (including automatic fault handling and balancing). See also BMS48V hardware files.


## Example Arduino sketch

```C++
#include <bq769x0.h>    // Library for Texas Instruments bq76920 battery management IC
#define BMS_ALERT_PIN 7     // attached to interrupt INT0
#define BMS_BOOT_PIN 8      // connected to TS1 input
#define BMS_I2C_ADDRESS 0x08 //0x08 for  part nums ending in BQ769300?DBT with ? being 0-3. For 6 or 7, use 0x18. Chip from LCSC was 3DBT

bq769x0 BMS(bq76930, BMS_I2C_ADDRESS);    // battery management system object

unsigned long previousMillis = 0;        // will store last time LED was updated
// constants won't change :
const long interval = 250; 
 
void setup()
{
  Serial.begin(115200);
  Serial.println(0x08);
  Serial.println("0x08");
  Serial.println("Starting BMS object");
  int err = BMS.begin(BMS_ALERT_PIN,BMS_BOOT_PIN);
  Serial.println("BMS object started");

  BMS.setTemperatureLimits(-20, 45, 0, 45);
  Serial.println("Temps set");
  BMS.setShuntResistorValue(5);
  BMS.setShortCircuitProtection(14000, 200);  // delay in us
  BMS.setOvercurrentChargeProtection(8000, 200);  // delay in ms
  BMS.setOvercurrentDischargeProtection(8000, 320); // delay in ms
  Serial.println("overcurrent dischargeprotection set");
 // BMS.setCellUndervoltageProtection(2600, 2); // delay in s
 // BMS.setCellOvervoltageProtection(4200, 2);  // delay in s
Serial.println("cell voltage protection levels set");
 // BMS.setBalancingThresholds(0, 3300, 20);  // minIdleTime_min, minCellV_mV, maxVoltageDiff_mV
  Serial.println("Balancing thresholds set");
 // BMS.setIdleCurrentThreshold(100);
 // BMS.disableAutoBalancing(); //ensure balancing is off so we don't fry anything during testing
 // BMS.enableDischarging();
 Serial.println("BMS Settings set");

#define SYS_STAT        0x00

 
}

void loop()
{
   unsigned long currentMillis = millis();
  if(currentMillis - previousMillis >= interval)
  {
    previousMillis = currentMillis;
    BMS.update();  // should be called at least every 250 ms
    Serial.println("Update complete, starting print register");
   BMS.printRegisters();
   // Serial.println("register output complete");
   Serial.print("cell 1 voltage: ");
   Serial.println(BMS.getCellVoltage(1));
   delay(15);
   Serial.print("cell 2 voltage: ");
   Serial.println(BMS.getCellVoltage(2));
   delay(15);
   Serial.print("cell 3 voltage: ");
   Serial.println(BMS.getCellVoltage(3));
   delay(15);
      Serial.print("cell 4 voltage: ");
   Serial.println(BMS.getCellVoltage(4));
   delay(15);
      Serial.print("cell 5 voltage: ");
   Serial.println(BMS.getCellVoltage(5));
      Serial.print("cell 6 voltage: ");
   Serial.println(BMS.getCellVoltage(6));
      Serial.print("cell 7 voltage: ");
   Serial.println(BMS.getCellVoltage(7));
      Serial.print("cell 8 voltage: ");
   Serial.println(BMS.getCellVoltage(8));
      Serial.print("cell 9 voltage: ");
   Serial.println(BMS.getCellVoltage(9));
      Serial.print("cell 10 voltage: ");
   Serial.println(BMS.getCellVoltage(10));
   


  
   Serial.print("Batt voltage: ");
   Serial.println(BMS.getBatteryVoltage());

  }
}
```

## To Do list

- Proper SOC estimation and coloumb counter implementation
- Testing for ICs with more than 5 cells
