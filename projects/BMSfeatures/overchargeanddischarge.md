# Overcharge and Overdischarge Protection

## Description
Lithium-ion cells typically have a voltage range between 2.5V and 4.2V (depending on the specific chemistry) and operating cells outside of this range can significantly reduce their lifespan and degrade their state of health (SOH). Knowing this, it is crucial that each cell is kept within its appropriate voltage threshold and that charging is consistent across the each cell. Overcharge and overdischarge protection in battery management systems are generally simple to understand, they are systems put in place to maintain voltages within these ranges to maximize their lifespan. 

In their simplest form, they consist of a MOSFET and some sort of IC which kicks in and switches the MOSFET off such that the device stops charging/discharging in the event that the battery cells exceed some threshold voltage range. 

## Current version
In our current iteration of the BMS, we are using a BQ77915-02 IC from Texas instruments whose voltage thresholds are hardware dependent, meaning that the cell voltage range cannot be changed unless you get a different BQ77915 IC. The board uses 2 N-MOSFETs connected to a DSG and CHG pin on the IC, and will turn off and render it non-conducting in the event that one of the cells exceeds or dips below the overcharge and overdischarge threshold voltage, this prevents the .