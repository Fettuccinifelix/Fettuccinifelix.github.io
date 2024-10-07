# Overcurrent protection
Overcurrent sensors are crucial for protecting lithium-ion batteries, as well as for measuring their state of charge (SOC) and state of health (SOH). Overcurrent faults can occur, for example, if there is an unexpected short circuit, causing an excessive current to flow through the battery, which can lead to overheating, damage to battery cells, or even thermal runaway. To prevent these scenarios, current is typically sensed and measured, an MCU then uses this information to calculate charge flow over time, whether during discharging (charge leaving the battery) or charging (charge entering the battery), which can then be used to accurately estimate the state of charge of the battery and predict remaining runtime. However, note that there are fuel gauge ICs available in the market that handle SOC monitoring, which can greatly simplify this process.

For current sensing, two common types of sensors are used: Hall effect sensors and current sense (shunt) resistors. Hall effect sensors utilize magnetic fields to measure current flow and are more suitable for high-power systems, handling currents above 100A. While they are costlier, they provide the added benefit of electrical insulation. On the other hand, current sense resistors, typically with values in the milliohm range, are precise and are commonly used in low-power systems. 

<div style="display: flex; justify-content: center; align-items: center;">
    <img src="/assets/img/BMS/Current-Sensing-Design-Technique.jpg" alt="sensing types for overcurrent" style = "width = 90%; height = auto;">
</div>

> It is essential to ensure that the calculated power dissipation does not exceed the resistor's power rating, and that the resistor tolerance is within acceptable limits usually ±1%. The ideal current sense resistors have low resistance values, high power ratings, and minimal tolerance to reduce power loss.

The current sensing circuit is placed in series with the load, as current remains constant through series components. The output from the sensing circuit must be amplified before being fed to the MCU, as the sensor detects changes in voltage rather than current, and the signal is typically too weak without amplification. However, in some cases, if the MCU has built-in amplification capabilities, this external step can be avoided.

<div style="display: flex; justify-content: center; align-items: center;">
    <img src="/assets/img/BMS/shunt.png" alt="Shunt resistor for overcurrent" style = "width = 90%; height = auto;">
</div>


> In addition to current sensors, voltage sensors are used for overvoltage protection and SOC measurement. Voltage sensing circuits often include an amplifier, a buffer, and a resistive divider to adjust the output signal. This adjusted signal is then fed to an ADC, which converts it to a digital format for processing by the MCU, allowing the system to monitor and react to changing battery conditions effectively.

## Current implementation
Overcurrent protection is currently being facilitated by the slave board, using a shunt resistor, the BQ77915 has set voltage ratings such that if the voltage across the shunt resistor increases past 70mV (in the case of an overcurrent fault), MOSFETs on the board will cut current flow from the batteries to the rest of the system. SOC and SOH calculations are done by a fuel gauge on the slave board, and communicates via I2C with the STM32 on the master board to be displayed. 

<div style="display: flex; justify-content: center; align-items: center;">
    <img src="/assets/img/BMS/overcurrentcircuit.JPG" alt="OC circuit" style = "width = 90%; height = auto;">
</div>

source: https://components101.com/article/comparison-between-shunt-hall-and-ic-based-current-sensing-designs 