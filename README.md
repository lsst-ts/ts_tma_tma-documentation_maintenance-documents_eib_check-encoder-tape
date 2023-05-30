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
- 

Download the [Configuration file](configFiles/config_std_EIB8791_withAdcValues.txt) from the repo.


## Elevation Tape
