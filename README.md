# Check Encoder Tape

| **Requested by:** | **AURA** |
|-------------------|----------|
| **Doc. Code**     |        |
| **Editor:**       | A. Izpizua         |
| **Approved by:**  | J. Garcia         |

## Index

- [Check Encoder Tape](#check-encoder-tape)
  - [Index](#index)
  - [Introduction](#introduction)
  - [Azimuth Tape](#azimuth-tape)
    - [Procedure](#procedure)
    - [Execution](#execution)
      - [Configure the EIB](#configure-the-eib)
  - [Elevation Tape](#elevation-tape)

## Introduction

This document shows how to proceed with the telescope to get data from the EIB and send to Heidenhain to check the status
of the encoder tape.

To check the tape all the tape must be recorded, so a movement that makes heads to travel the tape must be performed.
This movement is performed in a different way to check azimuth or elevation, so in this document there is section for each axis.

## Azimuth Tape

In this section the azimuth encoder tape status check is explained

### Procedure

The following steps must be performed

- Configure the EIB using the [This configuration file](configFiles/config_std_EIB8791_withAdcValues.txt).
- Configure the data capture in the EIB software
- Start the capture in the EIB software
- Move the telescope 360 deg using the ACW
- Stop the capture in the EIB software

### Execution

#### Configure the EIB

First the telescope must be positioned in the star position.

- Start the OSS using the EUI
- Open the PAS4000 and the last version of the TMA safety project (TMA_IS1). The support PC, has the last version of this project hosted in this [repo](https://gitlab.tekniker.es/aut/projects/3151-LSST/SafetyCode/TMA_IS.git).
- Open the variable list and run the observing of the variables
  ![Open Variable List](media/OpenVariableList.png)
  ![Start Observing variable list](media/StartObserving.png)
- Force sdiLIMAZWPOS and sdiLIMAZWNEG
  ![Force ACW Variables](media/ForceACWVariables.png)
  - It is also recommend to force  
- Check that the variables are forced
  ![Check the variables are forced](media/CheckVariablesAreForced.png)
- Start axis commissioning in the Indraworks
  ![Axis commissioning](media/AxisCommisioning.png)
  - If commanding of the motors is not possible because there is PLC program active, stop the PLC application
  ![Axis Commanding Not Possible](media/AxisCommanginNotPosible.png)
  ![Stop PLC Aplication](media/StopPLCProgram.png)
- Activate one AZCW axis
- Release the Azimuth brake putting the bDebugAzBrake to True
- Move to 550 deg (this is 360+190 deg) or to 170 (360-190 deg) the one is closest to the actual position [^1].
[^1]: The AZCW shows the azimuth position +360 deg. So when moving AZCW to 550 or 170 deg the Azimuth axis is moved to 190 or -190 deg.
- Engage the brake
- Connectg to the EIB using the Heidenhain application
- Load the configuration file to the application
- Change the IP and the mac for the UDP host
- Write the configuration to the EIB
- Enable the PTM
- Check the communication
- Configure the UDP
- Start the UDP dumping
- Release the brake
- Start movement with ACW
  - 0.04rmp=0.24deg/s --> 1500s
  - 1200000lines/1500s=800lines/s=800Hz per line. At least 8kHz.
  - Start going to 400 deg
  - When the axis is close to 400 deg change the setpoint to 300 deg
  - When the axis is close to 360 deg change the setpoint to 170 deg
- When the axis stops because the last position (170 deg) is reached, stop the UPD dumping
- Engage the brake
- Move AZCW to 173 deg
- Unforce all the forced variables
- Switch off the drive
- Start the PLC application
- Check the Bosch Telemetry is running properly
  - Balancing system
- Close the Indraworks
- Exit the EUI
- Reboot the AxesPXI
  - Connect to the AxesPXI using ssh and check that the connected PXI is the AxePXI
```bash
$ ssh admin@139.229.171.26
NI Linux Real-Time (run mode)

Log in with your NI-Auth credentials.

admin@139.229.171.26's password:
Last login: Tue May 30 16:20:11 2023 from 139.229.171.5

 █████  ██   ██ ███████ ███████       ██████  ██   ██ ██
██   ██  ██ ██  ██      ██            ██   ██  ██ ██  ██
███████   ███   █████   ███████ █████ ██████    ███   ██
██   ██  ██ ██  ██           ██       ██       ██ ██  ██
██   ██ ██   ██ ███████ ███████       ██      ██   ██ ██

```
  - Reboot the PXI
```bash
  admin@AxesPXI:~# reboot
```
- Wait for AxesPXI to restart. Check with a ping.
- Check that the AxesPXI restarted properly
```bash
admin@AxesPXI:~# cat /var/log/messages | grep LabVIEW_Custom
2023-05-30T16:12:23.216+00:00 AxesPXI LabVIEW_Custom_Log: Main Started
2023-05-30T16:12:28.216+00:00 AxesPXI LabVIEW_Custom_Log: Main axes task launched
2023-05-30T16:12:28.217+00:00 AxesPXI LabVIEW_Custom_Log: Encoder task launched
2023-05-30T16:12:28.221+00:00 AxesPXI LabVIEW_Custom_Log: Timed Loops Processors: Control Loop Azimuth: 5
2023-05-30T16:12:28.225+00:00 AxesPXI LabVIEW_Custom_Log: Timed Loops Processors: Control Loop Elevation: 6
2023-05-30T16:12:28.818+00:00 AxesPXI LabVIEW_Custom_Log: Timed Loops Processors: Trajectory Loop Azimuth: 4
2023-05-30T16:12:31.410+00:00 AxesPXI LabVIEW_Custom_Log: Timed Loops Processors: Trajectory Loop Elevation: 4
2023-05-30T16:12:34.902+00:00 AxesPXI LabVIEW_Custom_Log: Timed Loops Processors: Monitoring Loop 1: 2
2023-05-30T16:12:35.401+00:00 AxesPXI LabVIEW_Custom_Log: Timed Loops Processors: Encoder UPD Loop: 7
```
- Start the EUI
- Open the Encoder window and start the encoder and check it works correctly, without faults. Perhaps it needs a couple of reset-power actions.

Download the [Configuration file](configFiles/config_std_EIB8791_withAdcValues.txt) from the repo.


## Elevation Tape
