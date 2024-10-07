# Overcharge and Overdischarge Protection

## Description
Lithium-ion cells typically operate within a voltage range of 2.5V to 4.2V, depending on the specific chemistry. Operating outside this range can significantly reduce their lifespan and degrade their state of health (SOH). To ensure longevity, it is crucial to keep each cell within its appropriate voltage threshold and maintain consistent charging across all cells. Overcharge and over-discharge protection systems in battery management are designed to maintain voltages within these safe limits, maximizing battery lifespan.

In their simplest form, these systems consist of a MOSFET and an integrated circuit (IC) that monitors the cell voltage. When a cell exceeds or falls below the specified threshold, the IC activates, switching off the MOSFET to stop charging or discharging, thereby preventing damage to the cells.

## Current version
In our current iteration of the BMS, we are using the [BQ77915-02](https://www.ti.com/product/BQ77915?dcmp=dsproject&hqs=#order-quality) IC from Texas Instruments. The voltage thresholds of this IC are hardware-dependent, meaning the cell voltage range cannot be adjusted unless a different version of the BQ77915 is used. The board employs two N-MOSFETs connected to the IC's discharge (DSG) and charge (CHG) pins. If a cell's voltage exceeds or drops below the overcharge or over-discharge thresholds, the IC turns off the corresponding MOSFET, rendering it non-conducting. This effectively stops the device from operating in a way that would push the battery further outside its healthy voltage range, thus helping to preserve the overall health of the battery. Future iterations of the BMS could include a "smart chip" whose voltage thresholds can be adjusted using an external programmer in the case a different battery is used. 

<div style="display: flex; justify-content: center; align-items: center;">
    <img src="/assets/img/BMS/BQ77915.png" alt="BQ77915 chip" style = "width = 90%; height = auto;">
</div>

<div style="display: flex; justify-content: center; align-items: center;">
    <img src="/assets/img/BMS/BQ77915diagram.gif" alt="BQ77915 diagram" style = "width = 90%; height = auto;">
</div>

<div style="display: flex; justify-content: center; align-items: center;">
    <img src="/assets/img/BMS/overdsgandchgcircuit.JPG" alt="ODSG + OCHG diagram" style = "width = 90%; height = auto;">
</div>