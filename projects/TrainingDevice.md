# Biomedical Training Device

## Background and Overview
Lower Mainland Biomedical Engineering (LMBME) faces a growing demand for effective and flexible training of Biomedical Engineering Technologists (BMETs), who are responsible for maintaining over 110,000 medical devices across multiple hospitals. Current training options, such as factory training and in-house sessions, present significant challenges. Factory training is costly and limited in availability, while in-house training faces scheduling difficulties, limited equipment, and space constraints. As BMETs require up-to-date knowledge to maintain hospital devices properly, inadequate training could negatively impact hospital operations, patient care, and potentially increase healthcare costs.

The economic and operational consequences of insufficient training are substantial. Poorly maintained devices lead to frequent repairs, increased maintenance costs, and reduced hospital efficiency. Compromised patient care due to malfunctioning devices can also result in legal consequences, as per regulations like the Hospital Act of BC. This issue is critical not just for LMBME but also for the broader healthcare system, as malfunctioning devices can disrupt essential services and delay patient diagnostics.

This leads us to the aim of this project. To address the challenges of BMET training, we explored several alternative methods, including augmented reality (AR) simulations and online learning programs, in hopes of finding more cost-effective and flexible solutions. However, each of these methods encountered its own set of issues. For instance, while online options reduce costs and alleviate scheduling constraints, they often lack the hands-on interaction that is crucial for effective BMET training. In contrast, augmented reality faces challenges related to cost, as multiple headsets are required to accommodate several trainees, and the medical devices needed for training are frequently in use at hospitals. Consequently, we propose our solution: a training machine that uses a projector to display an interactive image onto a workbench. This approach combines the interactivity of traditional and augmented reality training while being significantly more cost-effective and accessible for widespread use.

This page discusses a Biomedical Engineering Technologist training device that was developed for BMEG 357 (Biomedical Engineering Design II). As the goal of the class is to mainly perform research on the problem at hand to determine the best solution, the final deliverable of this class is a report of our research and a critical function prototype, a barebones proof of concept. I will first be discussing the actual prototype we ended up with as well as my work that went into it, but if you are at any time interested in the research behind our solution [you can click here](#research). 

## Critical Function Prototype - The Holotrainer

When developing our prototype, we had 2 main factors to consider, projection and sensing. We needed a way to project the image and training instructions on our work surface and a way to sense that the user was interacting object projected. The 2 biggest sources of inspiration for our work are laser keyboards that are projected on your desk to be used normally - which was honestly just a novelty item, and those interactive floor projectors that malls will sometimes have for kids that project things like bubbles or fish that would move around if you stepped on them. 

To address our method of projection, we did first come across laser keyboards and wanted to base our design off of that. However, we found a little too late that unless you either 1. have a system of motors to move the laser diode around and outline the image, or 2. pass the beam through a specialized template like in traditional laser keyboards, you would not able to make complex shapes such as an infusion pump that BMETs would be training to use, and this isn't even taking into account the fact that the latter option only gives you a static image, making it unable to display the various training steps or different views of the machine. This led us move away from laser projection and towards an LCD projector based device for our solution proposal, more similar to the afforementioned interactive floor projection. This would enable us to display multiple moving images as needed during the training. 




## <a id="research"></a> Research 


## Key Features
- Feature 1
- Feature 2
