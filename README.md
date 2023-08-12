# ScanIt, ad hoc sensor data acquisition and control for Tasmota related devices


ScanIt is a Windows application for ad hoc acquisition of data from Tasmota controlled devices. ScanIt provides a graphical user interface for this purpose. It does not communicate with Tasmota via MQTT, it uses the HTTP interface. Alternatively, the serial COM port can be used. No other software tools like Influxdb or Prometheus are required. ScanIt communicates autonomously with Tasmota via the HTTP or COM interface.

ScanIt also includes a console environment. This can be used to control your device  directly. A text file can be included here to control and automate the process. This is very useful for setting up your application, especially via the serial port.

The ScanIt application uses the DotNet framework on Windows. This makes it easy to port it to Linux. Design and development refer to the VisualStudio 2022 Community Edition. This IDE is free of charge. ScanIt consists of two main parts: **Logging** and **Console**.

# Logging part to collect data

An example of a data scan for temperature logging is shown below:

**Status 10 : DS18B20>Temperature, BMP280>Temperature#0**

The corresponding output in the log window is:

_20.03.2023  14:41:08 DS18B20 Temperature = 15.6 BMP280 Temperature = 15_

_20.03.2023 14:41:18 DS18B20 Temperature = 15.6 BMP280 Temperature = 15_

At the same time, ScanIt writes the log data to a file in CSV format. Each entry is separated by a tab. This makes it easy to import the file into **LibreOffice-Calc** or **Excel**.


The following diagram was created from a temperature measurement using LibreOffice:

&#173;

![image](https://user-images.githubusercontent.com/121858068/233852035-db47b8b7-1104-4104-a1ed-6e0c9ffda588.png)


# Console part
The **Console** may be used for direct communication between the user and the Tasmota firmware. For example, to change the setpoint, when a temperature control is acquired. 

A text file can be included to control and automate the process. This is very useful for setting up your application, especially via the serial port.

ScanIt specific commands are not sent directly. They are interpreted and specially processed by the console. These commands begin with the "@" character. The following table provides a brief overview.

&#173;

| Command                | Brief Description                                               |
|------------------------|-----------------------------------------------------------------|
| **Batch related**      ||
| @&#173;batch           |  Execute commands from a text file                              |
| @&#173;delay           |  Delay inserted between commands                                |
| @&#173;echo            |  Turns the command echo from batch on or off                    |
| @&#173;edit            |  Edit batch file                                                |
| @&#173;saveparameter   |  Save parameter on exit                                         |
| @&#173;shutdown        |  Exits ScanIt                                                   |
| @&#173;wait            |  Waits H:M:S between commands                                   |
| @&#173;rem             |  Output a comment                                               |
||
| **Tasmota related** ||
| @&#173;driver          |  Active drivers                                                 |
| @&#173;feature         |  Active features                                                |
| @&#173;gpio            |  Active GPIOs                                                   |
| @&#173;gpioa           |  All GPIOs                                                      |
| @&#173;i2c             |  I2c drivers                                                    |
| @&#173;option          |  Option settings                                                |
| @&#173;optiona         |  All option settings                                            |
| @&#173;sensor          |  Active sensors                                                 |
| @&#173;status          |  Status from 1 to 11                                            |
||
|  **Main Window related** ||
| @&#173;append          |  Append the data to the log file                                |
| @&#173;execution       |  Starts a scan with the parameters set                          |
| @&#173;interval        |  Set the interval time in the main window                       |
| @&#173;ip-address      |  IP address of the hardware                                     |
| @&#173;logfile         |  Set the filename for the log file                              |
| @&#173;scan            |  Scans to be executed in the main window                        |
| @&#173;show            |  Display data sets in the main window                           |
||
| **COM related** ||
| @&#173;com             |  List all the serial ports on the computer                      |
| @&#173;cominfo         |  List the properties of all the serial ports on your computer   |
| @&#173;comconfig       |  List / set the configuration of the serial port                |

&#173;

**Example:**

**@&#173;Feature** offers among others

```
...
Feature 7 bit vector 00C01000
Bit 12 = USE_SHELLY_DIMMER
Bit 22 = USE_TIMEPROP
Bit 23 = USE_PID
...
```

A command sequences from a text file can be executed with **@&#173;Batch**

```
<BatchID>
@Interval 10
@Ip-Address NodeMcu2
@Execution Start
PidSp 25
@Wait 0:20:0
PidSp 27
@Wait 0:20::0
PidSp 25
@Wait 600
@Execution Stop
```

# Windows command line interface

ScanIt has a Windows command line interface. This can be used to activate a batch file e.g. time-controlled by the Windows Task Scheduler. This feature allows an automated collection of data without further user intervention.

A Windows command line might look like this:

**C:\ScanIt\ScanIt.exe  Batch:C:\ScanIt\Batch.txt**


**Batch Example:**
```
<BatchID>
@Scan "Status 10 : PidSp#1, PidPv#2, PidPower, PidTi#0, PidTc, PidTd",  "Power2", "Power1"
@Interval 10
@Ip-Address NodeMcu2
@Logfile C:\Tasmota Batch Files\Batch.csv
@Show 0
@Append 1
@Execution Start
PidSp 25
@Wait 0:10: 0
PidSp 27
@Wait 10: 0
PidSp 25
@Wait 600
@Execution Stop
@ShutDown
```

# Language extensions

The ScanIt application is not bound to a specific language. It is possible to change the language at run time. This is done using external language packages. These can be modified using a simple text editor such as Notepad. This makes it easy to add new languages to ScanIt. 


# Installation

ScanIt does not need to be specially installed on Windows. It is sufficient to extract the contents of ScanIt.zip to a common folder.

![BlueBulb20](https://user-images.githubusercontent.com/121858068/235621574-5dc6ff71-64c6-4db3-9fc7-bf284ee63434.gif)
> **Windows may blocks ZIP files from the Internet to protect the system. Please remember to unblock the ZIP file using the Windows Explorer.**

You must enable the HTTP API in your Tasmota application.

&#173;
