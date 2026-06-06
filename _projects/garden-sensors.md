---
layout: single
title: "Remote Garden Soil Sensor Project"
excerpt: "Personal Project - Summer 2025"
header:
    teaser: /assets/images/gardensensor/proofofconcept.jpg
date: 2025-08-15
author_profile: true
#classes: wide
---
[← Back to Projects](/projects)
<br>
## Project Overview

The goal of this project was to allow my wife to monitor the moisture levels of her vegetable garden soil during the summer without needing to go outside and physically check it.

![Proof of concept](/assets/images/gardensensor/proofofconcept.jpg){: width="600px" }
<br><small>Proof of concept<br></small>

[Hardware](#hardware)
[Electrical](#electrical)
[Software](#software)
[Outcomes](#outcomes)

### Highlights
- ESP-NOW wireless communication between ESP32 microcontrollers
- Solar powered base station mounted outside
- SR Latching cooling fans in base station
- Custom soldered PCB board
- Interior display
- Rasberry PI server

## Hardware

The major hardware portion of this project consisted of an outdoor enclosure to house the electronics required for the soil moisture sensors. It consisted of a exterior shell with exhaust fan port, and interior enclosure with a silica gel top to control moisture, and two electronics sleds that held the custom PCB and solar battery controller.

The enclosure was 3D printed in a white PETG for weather and UV resistance. The housing itself was mounted to an amazon bird house stand that could raise the enclosure to deck height to ensure adequate sun exposure for the solar panels as well as reduced wire lengths.

![CAD](/assets/images/gardensensor/cadbase.jpg){: width="600px" }
<br><small>CAD Assembly of base station<br></small>

![Assembled Front](/assets/images/gardensensor/basefront.jpg){: style="width: 600px; height: 600px; object-fit: cover; object-position: top;" }
<br><small>Assembled base station front view<br></small>

![Assembled Rear](/assets/images/gardensensor/baserear.jpg){: width="600px" }
<br><small>Assembled base station rear view<br></small>

<small>[Top](#project-overview)</small>

## Electrical

The outdoor base station was powered by a solar power module that charged a single 18650 battery and provided the required 5V for the ESP32 board. A temperature and humidity sensor were added to monitor the temperature of the enclosure. Two cooling fans were controlled by NPN transistors and an SR latch. This allowed the fans to be turned on and held on at a set temperature without needing to keep the ESP32 awake between sensor readings saving power. To measure the soil moisture levels, 5 capacitive soil sensors were used. 

![Fritzing](/assets/images/gardensensor/fritzing.jpg){: width="600px" }

![Fritzing Zoomed](/assets/images/gardensensor/fritzingzoomed.jpg){: width="600px" }
<br><small>Wiring Layout<br></small>

![PCB Top](/assets/images/gardensensor/pcbtop.jpg){: width="600px" }

![PCB Bottom](/assets/images/gardensensor/pcbbottom.jpg){: width="600px" }
<br><small>PCB Board<br></small>

![Enclosure](/assets/images/gardensensor/enclosure.jpg){: width="600px" }
<br><small>Electrical Enclosure<br></small>

<small>[Top](#project-overview)</small>

## Software

[Github](https://github.com/HalJamieson/Garden_Sensor)

The firmware for the ESP32s was written using the Platform IO extension for VS Code. The exterior ESP32 was programmed to wake from sleep every 20 minutes an obtain an enclosure temperature with the 5 soil sensors being read on each hour. This data would then be transmitted to the interior ESP32 in a struct containing the soil readings, enclosure temperature and humidity, as well as the two cooling fan states. To save power, the struct would not be transmitted every wake cycle, a transmission would only occur if there was a temperature change of 5 degree Celsius, new soil values or a change in cooling fan states.

The interior ESP32 controlled a display that cycled between an overview page and zone reporting. The overview page showed fan states, temperature, humidity, time since last packet and colour coded zone numbers (red for dry, yellow for medium, and green for wet). The zone reporting page showed the exact capacitive values being reported by the sensors.

![Display](/assets/images/gardensensor/display.jpg){: width="600px" }

![Display](/assets/images/gardensensor/display2.jpg){: width="600px" }
<br><small>Interior Display<br></small>

Once the interior ESP32 received the values, it would update its display and push the updated information to a small local web server being hosted on a Raspberry PI. The web server would pair the data with current weather conditions (temperature, humidity, cloud cover) using an openweathermap.org API and retain it in a csv log. 

![Raspberry Pi](/assets/images/gardensensor/dashboard.jpg){: width="600px" }
<br><small>Raspberry PI Dashboard Server <br></small>

<small>[Top](#project-overview)</small>

## Outcomes

The most significant issue with this project surrounded the soil sensors. They struggled with the outdoor conditions despite efforts to seal the electronics in silicone water proofing. Moisture accumulation resulted in two of the sensors failing with a few weeks of use. Future iterations could benefit from a conformal coating, however, the sensors are primary composed of PCB board and would likely result in failures from moisture regardless. The should almost be treated as a consumable component with future designs centred around ease of servicing and replacing each sensor as the fail. Resistors were later added on each sensors ground wire as the failure of the sensor resulted in a dead short that caused the ESP32 to shutdown completely. 

The outdoor enclosure performed well otherwise. It was able to maintain a reliable charge with varying weather conditions and did not experience any shut downs due to power loss in the 4 weeks it was run. 

<small>[Top](#project-overview)</small>