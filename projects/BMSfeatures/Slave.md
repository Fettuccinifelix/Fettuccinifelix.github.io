# Overview of Slave Board
This page delves more into detail the specifics of the slave board of the BMS, including descriptions of faults and specifications, copied from previous documentation - thus some information may be redundant

## Battery protection (BQ77915)
The battery protection feature of this BMS stems from the use of a BQ7791502 protection IC to prevent the batteries from various over faults. The circuit used to integrate the IC heavily references the BQ77915 evaluation module.

The BQ77915 is a low-power battery management system (BMS) IC designed to provide a comprehensive suite of voltage, current, temperature protections and smart cell balancing for battery packs without the use of a microcontroller. The BQ77915 IC provides pack protection through integrated CHG and DSG low-side NMOS FET drivers. The IC is also capable of providing smart, passive cell balancing through integrated FETs for cell balancing currents up to 50 mA. Specifically, the BQ77915 protects against the following: overvoltage, undervoltage, overcurrent charge, overcurrent discharge, short circuit discharge, and over/under-temperature while charging and discharging.

<div style="display: flex; justify-content: center; align-items: center;">
    <img src="/assets/img/BMS/protsch.jpg" alt="prot schematic" style = "width = 90%; height = auto;">
</div>

<div style="display: flex; justify-content: center; align-items: center;">
    <img src="/assets/img/BMS/protalllayer.JPG" alt="protall" style = "width = 90%; height = auto;">
</div>

<div style="display: flex; justify-content: center; align-items: center;">
    <img src="/assets/img/BMS/protlayer 1.JPG" alt="prot1" style="width: 45%; height: auto; margin-right: 10px;">
    <img src="/assets/img/BMS/prot layer 2.JPG" alt="prot2" style="width: 45%; height: auto;">
</div>


<div style="display: flex; justify-content: center; align-items: center;">
    <img src="/assets/img/BMS/prot3d.JPG" alt="prot3d" style = "width = 90%; height = auto;">
</div>



### Overvoltage protection
> OV detection occurs when at least one of the cell voltages is measured above the OV threshold. In response, the CHG FET is turned off until the voltage of the cell that triggered the fault is below the OV threshold minus the OV hysteresis. 

### Undervoltage protection
> UV detection occurs when at least one of the cell voltages is measured below the UV threshold. In response, the DSG FET is turned off, preventing further supply of power to any electronics in GRASP. The UV fault recovers and returns to normal operation when the voltage of the cell in fault goes above the UV threshold plus UV hysteresis

### Open-Wire protection
> OW fault detection occurs when the voltage difference between consecutive cells falls below the OW threshold. In response, CHG and DSG FETs are both turned off, and the fault recovers when the voltage difference exceeds the OW threshold plus hysteresis. 

### Overcurrent Discharge protection
> An OCD fault is when the discharge load is high enough such that the voltage across the shunt resistor is measured below the OCD voltage threshold. In response to this, both CHG and DSG FETs are turned off, and the fault recovers through load removal, the voltage falls below the threshold for the duration of the overcurrent recovery timer, or a combination of both.

### Short Circuit Discharge protection
> An SCD fault is when the discharge load is high enough such that the voltage across the shunt resistor is measured below the SCD voltage threshold. In response to this, both CHG and DSG FETs are turned off. Similar to OCD recovery, the fault is recovered when the load is removed or if the voltage is above the threshold for the duration of the recovery timer, or a combination of both.

### Overcurrent Charge protection
> An OCC fault is when the discharge load is high enough such that the voltage across the shunt resistor is measured above the OCC voltage threshold during charging. In response to this, both CHG and DSG FETs are turned off. Similar to OCD recovery, the fault is recovered when a load is detected or if the voltage is below the threshold for the duration of the recovery timer, or a combination of both.

### Over and Under-Temperature protection
> These faults occur when the temperature, as measured by an NTC thermistor, exceeds the over temperature threshold or falls below the under temperature threshold for the respective delay periods
> - OTC: Only the CHG FET is turned off.
> - OTD: Both CHG and DSG FETs are turned off.
> - UTC: Only the CHG FET is turned off.
> - UTD: Both CHG and DSG FETs are turned off.

> Recovery occurs when the temperature returns within normal operating ranges plus hysteresis for the respective delay periods. For OTD and UTD faults, load removal may also be required for recovery.

### Load Detection and Removal Detection
> The load detection and removal detection features are implemented with the LD pin. When no undervoltage fault and current fault conditions are present, the LD pin is held in an open-drain state. Once any UV, OCD1, OCD2, OCC, or SCD fault occurs and load removal or detection is selected as device of the recovery conditions, a high impedance pulldown path to VSS is enabled on the LD pin. With an external load still present, the LD pin will be externally pulled high: It is internally clamped to VLDCLAMP and is resistor-limited through RLD externally to avoid conducting excessive current. If the LD pin voltage exceeds VLDT for tLD_DEG, it is interpreted as a load present condition and is one of the recovery mechanisms selectable for an OCC fault. When the load is eventually removed, the internal high-impedance path to VSS is sufficient to pull the LD pin below VLDT for tLD_DEG. This is interpreted as a load removed condition and is one of the recovery mechanisms selectable for UV, OCD1, OCD2, and SCD faults.

### Summary of protection responses and recovery summary
> The following table is from the BQ77915 datasheet, and summarizes how each fault condition affects the state of the DSG and CHG output signals, as well as the recovery conditions required to resume charging and/or discharging.

<div style="display: flex; justify-content: center; align-items: center;">
    <img src="/assets/img/BMS/faulttable.png" alt="faulttable" style = "width = 90%; height = auto;">
</div>

### Cell balancing
> Cell balancing is performed by comparing the cell voltages with respect to cell balancing threshold voltages, evaluating the results of the comparison and controlling the cell balancing FET, which over a period of time will allow for closer cell voltages, thereby extending battery pack life. 

## Fuel Gauging (BQ34110) 
>
The Slave Board of the BMS is also responsible for keeping track of SOC and SOH, this is mainly done through the BQ34110 battery gas gauge, which is implemented into the board based on the BQ34110EVM-796 evaluation module and can communicate information to the Master Board through an I2C communication protocol

The BQ34110 gas gauge uses Compensated End-of-Discharge Voltage (CEDV) technology to accurately predict battery capacity and other operational characteristics. It can be queried by a host processor to provide cell information such as remaining capacity, full charge capacity, and average current. The End of Service determination function of the BQ34110 monitors the health of the battery through the use of infrequent Learning Phases, which involves a controlled discharge of ~1% capacity, and provides an alert to the system when the battery is approaching the end of its usable service.

The battery current is monitored by measuring the voltage across a series resistor, RSENSE, which is placed in series with the battery pack and has a value of 5 mΩ. The BQ34110 device integrates two ADCs, one of which is dedicated to current measurement, and the second used for measurement of several other parameters, including temperature and voltage

Communication with the device is provided through an I2C interface, supporting rates up to 400 kHz. Dual ALERT pins are provided with programmable configuration, which enables them to be used for such functions as a host interrupt/alert or controlling the battery charger. To minimize power consumption, the bq34110 gauge has several power modes: NORMAL, SNOOZE, and SLEEP, which are under register or algorithm control. In addition, a separate chip enable (CE) pin is provided to control the internal LDO, which powers the bq34110 internal circuitry, and can put the device into SHUTDOWN mode. Information is accessed through a series of commands called Data Commands, which are indicated by the general format Command(). These commands are used to read and write information in the bq34110 device’s control and status registers, as well as its data flash locations. Commands are sent from the host to the bq34110 device via I2C and can be executed during application development, pack manufacture, or end-equipment operation. Cell information is stored in the bq34110 device in non-volatile flash memory. Many of the data flash locations are accessible during application development and pack manufacture. They cannot, generally, be accessed directly during end-equipment operation. Access to these locations is achieved by using the bq34110 device’s companion evaluation software, through individual commands, or through a sequence of data flash access commands. To access a desired data flash location, the correct data flash subclass and offset must be known. The bq34110 device provides 32 bytes of user-programmable data flash memory. This data space is accessed through a data flash interface.

(code is still WIP as of 10/05/2024)

<div style="display: flex; justify-content: center; align-items: center;">
    <img src="/assets/img/BMS/FGsch2.jpg" alt="fuel gauge schematic" style = "width = 90%; height = auto;">
</div>
<div style="display: flex; justify-content: center; align-items: center;">
    <img src="/assets/img/BMS/FGalllayer.JPG" alt="fgall" style = "width = 90%; height = auto;">
</div>

<div style="display: flex; justify-content: center; align-items: center;">
    <img src="/assets/img/BMS/FG1.JPG" alt="FG1" style="width: 45%; height: auto; margin-right: 10px;">
    <img src="/assets/img/BMS/FG2.JPG" alt="FG2" style="width: 45%; height: auto;">
</div>


<div style="display: flex; justify-content: center; align-items: center;">
    <img src="/assets/img/BMS/FG3d.JPG" alt="FG3D" style = "width = 90%; height = auto;">
</div>

## Technical specifications

### Battery protection
- OV threshold (mV): 4200; Hysteresis (mV): 200
- UV threshold (mV): 2900; Hysteresis (mV): 400
- OW threshold (nA): 100
- OCD threshold (mV): 70
- SCD threshold (mV): 120
- OCC threshold (mV): 70
- OTC temperature (degC): 45; OTD temperature (degC): 65; UTD temperature (degC): -10; UTC temperature (degC): 0
- Cell balancing V_start (V): 3.8

### General
- number of series cells: 4
- V_in (max): 25 V
- Overvoltage threshold: 4.2V
- Undervoltage (overdischarge) threshold: 2.9V


