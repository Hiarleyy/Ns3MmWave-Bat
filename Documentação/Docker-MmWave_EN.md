# Docker NS3-MmWave

### Language | Idioma
**English** | [Português](Docker-MmWave.md)

tags: #fleeting #5g #ns3 #project 
Created:2024-06-20, Thu Jun 4 - 19:31
Week-number:25

## Summary:
This document describes the requirements and tools needed to use the NS3-MmWave Docker image for 5G network simulations. The image provides a complete environment with all necessary dependencies pre-configured.

## How to use the image?
To use the Docker image, pull it from Docker Hub and run it with appropriate configurations for your simulation environment.

## **Compilers and Development Tools**:

- **GCC**: GNU Compiler Collection (GCC) version 4.9 or higher. The recommended version is GCC 9.3.0 or higher.
- **G++**: C++ compiler associated with GCC.
- **Python**: Python 3.6 or higher for configuration and execution scripts.
- **Ranger**: Customized terminal to facilitate file visualization and modification
- **Fish**: Terminal used to complete Linux commands
- **Vim**: Text editor for code changes - (Can be replaced by Nano)

## **Required Libraries**:

- **GNU C Library** (glibc) version 2.27 or higher.
- **CMake** version 3.10 or higher to build some optional modules.
- **pkg-config** to facilitate configuration of dependent libraries.
- **libsqlite3-dev** for SQLite support.
- **libxml2-dev** for XML support.
- **libgtk-3-dev** and **libgtk2.0-0** for graphical interface (optional).
- **libpcap-dev** for packet capture support.
- **libgsl-dev** for GNU Scientific Library libraries.
- **libbz2-dev** for bzip2 compression support.
- **libboost-all-dev** for various support functionalities (optional).

## **Optional Tools**:

- **Netanim**: For simulation animation, requires Qt4 or Qt5 library.
- **GNUplot**: For graph generation.
- **Tcpdump**: For packet capture and analysis.

## Shell Scripts for NS3
- The project image also adds two files to facilitate program usage:
	reload.sh -- Renews the Packet.cc file to be built again
	move-files.sh -- Moves output files to a folder called dataframe

## **Development Modules**
- Ns3-Mmwave: (https://github.com/nyuwireless-unipd/ns3-mmwave) -- **Version 8.0** 
- ---
Reference:

https://ieeexplore.ieee.org/document/8344116/