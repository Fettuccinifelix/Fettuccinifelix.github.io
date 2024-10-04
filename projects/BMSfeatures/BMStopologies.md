# BMS Topologies 
In designing battery management systems, choosing the appropriate topology is crucial for balancing efficiency, reliability, and scalability. Various topologies each present unique advantages and challenges. This page explores these different BMS topologies, examining their core characteristics, benefits, and limitations, as well as what we ultimately went with for our application and why. 

<div style="display: flex; justify-content: center; align-items: center;">
    <img src="/assets/img/BMS/alltop.PNG" alt="All topologies" style = "width = 90%; height = auto;">
</div>


## Distributed 
The distributed topology involves using small voltage and discharge monitor sockets that communicate directly with a master controller. This approach allows for each cell to be individually monitored, ensuring a high degree of precision in battery management. It is both simple and reliable, providing straightforward data collection for the entire battery system. However, implementing this topology requires a large number of small printed circuit boards (PCBs), and mounting them on each individual cell can be challenging. The increased complexity of managing many small boards, especially when considering physical space and connections, adds to the implementation effort.

<div style="display: flex; justify-content: center; align-items: center;">
    <img src="/assets/img/BMS/Distributed-BMS.webp" alt="Distributed topology" style = "width = 90%; height = auto;">
</div>

## Centralized
On the other hand, a centralized structure uses a master controller unit that directly manages all the batteries in the system. This approach consolidates monitoring and management into a single unit, simplifying system architecture and reducing the number of components. The centralized structure can lead to easier maintenance and fewer parts, but it may lack the granularity of control offered by other setups. Additionally, a centralized controller could become a single point of failure, which poses a risk if redundancy is not incorporated into the design.

<div style="display: flex; justify-content: center; align-items: center;">
    <img src="/assets/img/BMS/Centralized-BMS.webp" alt="centralized topology" style = "width = 90%; height = auto;">
</div>

## Modular
Lastly, the modular structure, which employs multiple slave units that gather data from individual batteries and forward this information to a central master controller. This setup offers flexibility, as it allows the system to be easily expanded or modified by adding or removing slave units. However, reliable communication between the master controller and the slave units can be complex, introducing potential points of failure and challenges in synchronization, especially in environments with a lot of electrical noise. 

<div style="display: flex; justify-content: center; align-items: center;">
    <img src="/assets/img/BMS/Modular-BMS.webp" alt="modular topology" style = "width = 90%; height = auto;">
</div>

## Our choice
We chose to implement a modular-like structure to address the redundancy requirements typically associated with a centralized system, while also benefiting from its relative simplicity—since not every cell requires its own dedicated board. The current topology consists of two boards: a master and a slave. The [slave board](/projects/BMSfeatures/Slave.md) contains protection circuitry to mitigate the aforementioned faults and sensing hardware to collect battery pack data. This data is then sent to the [master board](/projects/BMSfeatures/Master.md), which is responsible for aggregating and displaying the information.

<div style="display: flex; justify-content: center; align-items: center;">
    <img src="/assets/img/BMS/bms-Page-2.drawio.png" alt="current BMS architecture" style = "width = 90%; height = auto;">
</div>

sources: https://www.mdpi.com/1996-1073/14/14/4279; https://tritekbattery.com/3-topologies-of-battery-management-system/