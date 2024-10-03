# Cell Balancing

Cell balancing is a technique which ensures that each battery cell is charged simultaneously and maintains an approximately equal charge level with others during charging.  There are sometimes unavoidable aspects such as differences in internal resistances, within individual cells of a battery pack, which could result in inconsistent charging behavior. Additionally, some cells may reach their maximum voltage faster than others, which can lead to overheating and accelerated degradation. These issues underscore how important cell balancing is for maintaining battery health and maximizing performance, and this section is dedicated to displaying what I've learned about cell balancing and the rationale behind our design decisions regarding it. 

## Passive Balancing
One method for addressing these discrepancies is passive cell balancing, which is both the simplest and most cost-effective approach. In this method, each cell is equipped with a bypass resistor connected to a switch or transistor. When a cell reaches its maximum voltage, a MOSFET is activated, allowing excess charge to be dissipated as heat through the bypass resistor, thereby balancing all cells during charging. However, passive balancing has limitations: it does not consider cells that reach minimum voltage levels faster than others, leading to imbalances during discharge. Additionally, passive balancing is inefficient, as it wastes excess energy as heat rather than redistributing it effectively. 

<div style="display: flex; justify-content: center; align-items: center;">
    <img src="/assets/img/BMS/PassiveCB.png" alt="Passive cell balancing" style = "width = 90%; height = auto;">
</div>

## Active Balancing
Active cell balancing provides an alternative by redistributing excess charge from one cell to another, rather than dissipating it as heat. This method is more efficient because it maintains energy within the battery pack, thus reducing waste. One common implementation of active balancing involves the use of charge shuttles or flying capacitors, which transfer energy between adjacent cells. However, active balancing systems are more complex and expensive compared to passive balancing, and charge transfer is typically limited to adjacent cells, which can make the balancing process slower in some configurations. 

<div style="display: flex; justify-content: center; align-items: center;">
    <img src="/assets/img/BMS/ActiveCB.png" alt="Active cell balancing" style = "width = 90%; height = auto;">
</div>

## Our Choice
We mainly decided to go with passive balancing due to its simplicity and the nature of our application. As the project's power requirements are not fully concrete, there is not much need for the BMS to fully maximize battery charge through active balancing. However, this can be looked into for future iterations of the BMS as smaller batteries can be used - needing more efficient power distribution/battery management methods.


source: https://www.youtube.com/watch?app=desktop&v=q4wDa_m9-8E