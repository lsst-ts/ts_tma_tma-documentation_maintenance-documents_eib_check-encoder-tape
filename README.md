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
- Force sdiLIMAZWPOS and sdiLIMAZWNEG
  - It is also recommend to force  
- Check that the variables are forced
- Start axis commissioning in the Indraworks
  - If commanding of the motors is not possible because there is PLC program active, stop the PLC application
- Activate one AZCW axis
- Control the brake
- Move to 550 deg (this is 360+190 deg)
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
   


Download the [Configuration file](configFiles/config_std_EIB8791_withAdcValues.txt) from the repo.


## Elevation Tape
