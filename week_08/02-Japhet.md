---
title: "02: Secure and User-Friendly Over-the-Air Firmware Distribution in
a Portable Faraday Cage by Martin Streigel, Johann Heyszl, Florian Jakobmeier, Yacov Matveev, Georg Sigil"
date: 2020-08-30
type: book
commentable: true

# Provide the name of the presenter
summary: "Presenter(s): Alvin Yang, Japhet Ye"

# Provide other tags that describe the paper
tags:
- teaching
- ee693e
---

***
## Paper Summary
The goal of this paper was to introduce a new method of securely deploying an internet-of-things (IoT) wireless sensor network by employing the use of a faraday cage that can set up the sensor nodes securely. The main motivation behind this product is that the main security threat of IoT sensor networks is the initial set up process where the nodes must connect to the central backend server. Since these types of networks are distributed networks, in some IoT protocols security information gets passed on through many nodes and an adversary may exploits that to gain access to the network. The authors created *Box* which is a Faraday Cage that prevents anyone from obtaining vital network connection through the initial network set up.

***

## Presentation
{{< youtube w7Ft2ymGmfc >}}

[PROVIDE A VIDEO RECORD OF YOUR PRESENTATION]
***

## Review
### Strengths
- Thorough explanations of the details behind the design and the motivation of the paper.
- Strong expiramentation process and case study on how different users interact and trust the product


### Weaknesses
- Security discussion is very lacking and provides no useful feedback and analysis of other types of security holes in the set up process.

### Detalied Comments
The researchers did an excellent job explaining the design and motivations by providing scenarios and business logic to why the *Box* has an economic place in the IoT industry via how wireless sensor network managers have very difficult time maintaining and expanding their networks because of the strict requirements of most IoT protocols and the fact that these types of networks may be deployed over such a vast distance that updating and deploying securely, especially when workers with no security fluency are deploying new nodes to the network. 

Their User Study was also very helpful as it demonstrated that even though the *Box* had such an important role in securing a wireless sensor network, workers who do not have a security background and those who do approach the *Box* with different view points, but ultimately both have the same conclusion about the product.

However, their security discussion is sorely lacking. While the background for the paper is solid, their discussion into the various other security questions do not have sustenance.


### Implementation
[PROVIDE DETAILED EXPLIANATION OF THE CODES/DATA PROVIDED BY THE PAPER] (>
200 WORDS)]
[PROVIDE LINK(S) TO THE CODES/DATA PROVIDED BY THE PAPER](https://github.com/gustybear-teaching/course_ee693e_2020_fall)

The main idea of the *Box* was to create a simple way for wireless node sensor network owners to deploy devices quickly to the field at scale. The problem is that the current architecture puts on so much weight on the owners and maintainers of the networks to deploy and update securely and this is where most of the main security concerns lay. Updating firmware requires some level of technical knowledge and can be dangerous to the security of the network if someone unqualified or not wanted were to do something wrong or malicious during this step. Along with the previously stated protocol problems of IoT protocols, wireless senor network owners take on so much burden in maintenance. 

*Box* is a cost effective solution that helps alleviate the problem and scales extremely well. Simply, place all the wireless sensor devices into the faraday cage and through over-the-air (OTA) transmission will either set up the devices with the requisite network information for initial deployment or updating to the correct firmware. To achieve this, the only requirement is to have an OTA capable bootloader so that the *Box* can update the firmware securely. This is feasible and cheap to the network owners as this places the expense on the manufacturer. 

It is composed of two parts: the faraday cage itself, and a Raspberry Pi 3. The faraday cage is made up of materials that can block many high power injection attacks and does not leak any EM radiation for a side-channel attack. The Raspberry Pi is connected to multiple things including a speaker, LEDs, and a magnet sensor. The magnet sensor prevents the Raspberry Pi from doing anything when the *Box* is not secured closed. The speakers and LEDs are the main interface in which directions and information will be presented to the user.

{{< figure src="https://github.com/gustybear-teaching/course_ee693e_2020_fall/raw/main/week_08/images/faradaycage.png" title="Box Hardware" width="300" >}}

The Raspbery Pi itself would be preprogrammed to contain the network information and any new firmware that needs to be installed onto the sensors. When the box is secured, the Raspberry Pi will create its own ad hoc network. From here, the Pi will then check to see if it needs to install any new firmware onto the devices. Any new firmware will need to also have been loaded onto the Pi before the *Box* was closed. If there is new firmware to install, it will then use the OTA capable bootloader of the device to securely flash the firmware onto it. Afterwards it then tranmits the network information for inital connection to the central backend before shutting down the sensors. From here, the sensors are ready to be set up in the field without any additional work from the operator.

{{< figure src="https://github.com/gustybear-teaching/course_ee693e_2020_fall/raw/main/week_08/images/boxstatemachine.png" title="State Machine of Box" width="300" >}}

### Experimentation

The authors tested the usability of their hardware solution. The reasearchers hypothesized that the *Box* solution was 1) faster than needing to fiddle around with wires and 2) intuitive as to not allow users to make security-critical errors. They gathered 31 subjects, 21 male and 10 female, in the age range of 23 - 53. Out of the 31 total people, 20 had computer science and electrical engineering backgrounds that they considered *exprets*. Those who do not have experience with security or those who possessed a degree that is not of computer science or electrical engineering were deemed *novices*. All of the participants were given the task of setting up a wireless sensor network using the box by programming sensor nodes.

The authors do note that due to the very small sample size, their findings are purely qualatative. All in all, both groups were able to program the nodes using the box faster than using a wired method while making fewer errors and being less hesitant in using the technology as shown in the figure below.

{{< figure src="https://github.com/gustybear-teaching/course_ee693e_2020_fall/raw/main/week_08/images/usability.png" title="Usability Study Results" width="300" >}}

When surveyed at the end of the end of the exprirament, everone expressed positivley about the *Box*, especially those who had industrial training. There were some individuals in the experts group were a bit more hesitant about the product as they were more curious to know the inner workings and tiny details about how the device worked, along with more detailed explanations in the instructions that they were given. In the end, the authors conclude that their product can help change an ever growing industry.

### Questions From the Audience