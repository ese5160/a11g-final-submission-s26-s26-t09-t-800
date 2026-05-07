[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/Y5lYn2wb)

# a11g-final-submission

**Team Number: T-09**

**Team Name: T-800**

| Team Member Name | Email Address           | GitHub Handle |
| ---------------- | ----------------------- | ------------- |
| Rico Zhuang      | zzhuan13@seas.upenn.edu | Rico-Zhuang   |
| Peiyu Chen       | peiyuch@seas.upenn.edu  | chpeiyu       |

**GitHub Repository URL: https://github.com/ese5160/final-project-firmware-s26-t09-t-800.git**

## 1. Video Presentation

## 2. Project Summary

**2.1 Device Description**
Our device is a user-friendly long lasting LiPO powered IoT environmental monitoring device, which shows measured data locally on a OLED screen and remotely on Node-RED dashboard whtough Wi-Fi. It has solar panel charging, IP22 ingress protection rating, OTA, and device start/restart music.

**2.2 Device Functionality**
We first locate what functions are necessary for an IoT environmental device regarding power, sensors, and acturators. Then, we formulate reasonable HRS and SRS for the system, and check whether there are enough ports (I2C, PWM, UART, .etc) for the MCU mudule. Next, we pick on-board and off-board parts which satisfy our HRS and SRS. After that, we revise our system overall design when doing PCB design in Altium.

Our sensors are picked to have enough precision, low power consumption, and try to be a SMD sensor to save space. Our display is chosen to be an E-Ink display to save power, it can keep what is on the screen even if it is powered off. A solar panel is selected to charge the LiPO for long lasting purpose. A buzzer is chosen to play music when the system start/restart.

![block diagram](images\final-final-block.png)

**2.3 Challenges**

1. A hardware problem: Our MCU moudle on the PCB cannot be programmed/flashed. Solution: We have to use a dev board (2708A) as the controller to overcome it.
2. A hardware problem: Our E-Ink display has never arrived. Solution: We choose another small OLED display as an alternative to save power.
3. A firmware problem: We have multiple I2C sensor in series. Sensors have different startup time and I2C rate. It's hard to have them work together. Solution: We created another thread for SPS30 PM2.5 sensor.
4. A virtual machine deployment problem: The virtual machine cannot be assigned and deployed by MS AZURE. Solution: Create and deploy the virtual machine using Penn network.

**2.4 Prototype Learnings**
We’ve learned that we need to make room for early planning because early prototypes are rarely the final result, and shifts in plans happen more often than we think.

If we were to do this again, we would get rid of the speaker module and use a photoresistor instead of the phototransistors, and we would try to expose more unused GPIO pins.

**2.5 Next Steps & Takeaways**
We faced a lot of issues that were entirely out of our control, and if there is anything to improve, we would say that getting the MCU module on the PCBA to work would be a good place to start. After that, we could rearrange the ports leading to our peripherals so that the assembly is cleaner. Finally, I think we did not thoroughly consider thermal dissipation, and better placement of our thermally sensitive sensors would be a good next step.

During this semester’s ESE5160 course, we found that the homework assignments were the most helpful to our final project, as they were actually part of it. They helped us manage our time and schedule well, and they provided good insight into the actual workflow of the industry. The lectures were good supplements to the homework and provided us with knowledge that we could not have learned by ourselves otherwise.

**2.6 Project Links**
Node-RED:
Editor: http://20.94.215.208:1880/
Dashboard: http://20.94.215.208:1880/dashboard/t800
PCBA project: https://upenn-eselabs.365.altium.com/designs/82AC7F5B-175C-4F1C-8C58-DB57B92AD619

## 3. Hardware & Software Requirements

##### Updated HRS

| ID | Description | Have we met it? |
| :--- | :--- | :--- |
| HRS-01 | The device shall use the SIWG917Y121MGABA as the microcontroller and wireless communication IC. | Yes. |
| HRS-02 | The device shall use Node-RED for remote display and control. | Yes. Live data was shown on Node-RED. |
| HRS-03 | The device should operate for at least 1 month when powered by a fully charged single-cell 18650 Li-Ion battery. | Not met. Due to us using a screen with much higher current requirement, our device can last about 1 week without sunlight and other power sources |
| HRS-04 | An IMU shall be used to detect unexpected position changes of the device. | Yes, the IMU gives a z reading of roughly 0.98 when stationary, and if you shake it a bit the x and y readings changes as well.  |
| HRS-05 | The device should reach at least IP33 for dust-proofing and water-proofing. | We reached IP22 |
| HRS-06 | The temperature and relative humidity sensor shall have an accuracy of 0.5°C and 2.0% RH. | Verified with room temperature and bathroom humidity |
| HRS-07 | A set of sensors should be used to measure air pollutants: PM2.5, CO, CH4, and H2S. | H2S and CH4 not implemented |
| HRS-08 | The device should use a GNSS module for GPS positioning. | No. GPS was not implemented. |
| HRS-09 | The device shall adjust the angle of the solar panel once every 2 hours. | No. Solar panel adjustment was not implemented. |
| HRS-010 | A small fan shall run to keep the device well-ventilated during measurement and should stop once the measurement is done. | Yes, fan built into the box. |
| HRS-011 | An LPWAN module should be used for wireless communication when Wi-Fi is not available. | No. LPWAN backup was not implemented. |
| HRS-012 | Two photodiodes should be used to detect light intensity. | No. Photodiodes were not implemented. |
| HRS-013 | A servo motor should be used to rotate the solar panel. | No. Servo rotation was not implemented. |
| HRS-014 | A solar panel shall be used to charge the 18650 Li-Ion battery. | Yes. Solar charging path was tested. |
| HRS-015 | A microphone should be used to capture the user's voice command. | No. Voice command was not implemented. |
| HRS-016 | A speaker should be used to make sounds at least 50 dB. | yes, we have a speaker that plays startup music|
##### Updated SRS

| ID | Description | Have we met it? |
| :--- | :--- | :--- |
| SRS-01 | The air quality sensor values shall be read at a base frequency of every half hour. | Yes. Periodic sensor reading was verified. |
| SRS-02 | The system should store collected data, error logs, and other information on the cloud. | Partially met. Data was sent to Node-RED, but full logging was not implemented. |
| SRS-03 | The system shall monitor its battery level and reduce the monitoring frequency on low battery until the battery level recovers. | No. Adaptive sampling was not implemented. |
| SRS-04 | The system shall turn on the venting fan a few seconds before making any measurement and turn off the venting fan after detection is made. | Yes. Fan control is met. SPS30 turns on the fan 1 second mefore measurement starts, and stays on for a few seconds after measurement is completed|
| SRS-05 | The system should calculate the light intensity difference between the two photodiodes to find the best direction for the solar panel. | No. Light tracking was not implemented. |
| SRS-06 | The device should enter Deep Sleep mode between measurement intervals to save battery, waking up only by RTC or an interrupt. | Partially met. Low-power mode was considered but not fully validated. |

## 4. Project Photos & Screenshots
![f1](images\f41.jpeg)
![f2](images\f42.jpeg)
![f3](images\f43.jpeg)
![f4](images\f44.jpeg)
![f5](images\f45.jpg)
![f6](images\5.jpeg)
![f7](images\6.jpeg)
![f8](images\11.jpeg)
![f9](images\pcb-2d.png)
![f10](images\pcb-2d1.png)
![f11](images\pcb-3d1.png)
![f12](images\pcb-3d2.png)
![f13](images\f46.jpeg)
![f14](images\f47.png)
![f15](images\final-final-block.png)

## 5. Codebase
Final embedded C firmware codebase: https://github.com/ese5160/final-project-firmware-s26-t09-t-800.git

Node-RED dashboard code: https://github.com/ese5160/final-project-firmware-s26-t09-t-800/blob/main/wifi_http_otaf_soc/nodered_flows_t800.json


Do *not* commit any of your source code to this repository. Rather, provide links to the other GitHub repository you've already been using with your firmware.

- A link to your final embedded C firmware codebases
- A link to your Node-RED dashboard code
- Links to any other software required for the functionality of your device