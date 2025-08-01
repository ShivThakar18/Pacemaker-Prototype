# Pacemaker Prototype
In this project, we developed a functional pacemaker simulation system using microcontrollers featuring Python GUI and MATLAB Simulink state flows for pacing mode visualization

## Pacemaker Design 
To implement the pacemaker on Simulink, the system is broken down into different modules:

- Serial Communication Receiving 
- Serial Communication Transmitting 
- Rate Adaptive 
- Input Conversions 
- Hardware Output
- Hardware Inputs
- Pacemaker Stateflow

### Serial Communication Receiving (COM_IN)
This module is responsible for communicating with the Device Controller Monitor (DCM). The implementation uses a serial receive UART block that sends RX data and status to the chart. The chart contains four states: 

- **State 1**: Set initial parameters for default mode
- **State 2**: Standby Mode, wait for data to be received from the UART serial monitor
- To transition into the next state, a hexadecimal 16 and 12 must be received from the DCM
- Once received, the type and its corresponding variable name is set for each value
- If two hexadecimal 16s are received consecutively, the parameters must be echoed back

### Serial Communication Transmitting (COM_OUT)
This module is used to package the parameters using a demultiplexer and byte packaging, which is sent back to the DCM using UART serial communication.

### Rate Adaptive 
This module is used to specify the pacing rate at a given time based on the detected activity level. The calculation is performed for modes ranging from 4 to 10, with input parameters including Mode, Maximum Sensor Rate, Reaction Time, Response Factor, Activity Threshold, Recovery Time, and Lower Rate Limit.

### Input Conversions
This subsystem is used to convert some of the inputs to their ideal units required for the Pacemaker. The following conversions are required: 

- Lower Rate Limit
- Atrial and Ventricle Sensing
- Atrial and Ventricle Amplitude Selection

### Hardware Outputs
This module is used to detect if a pulse is being received from the atrium and the ventricle using a digital read input pin. 

- ATR_CMP_DETECT stores a boolean value if a signal is detected in the atrium
- VENT_CMP_DETECT stores a boolean value if a signal is detected in the ventricle

### Hardware Inputs 
The hardware inputs allocate all of the variable names to its corresponding digital write output pin. The inputs are determined from the Pacemaker Stateflow and Input Conversions. 

### Pacemaker Stateflow 
The pacemaker stateflow is implemented using a state chart and multiple sub-charts. All of the variables i nthe state chart are treated as inputs from previous modules. The state chart is broken down into 7 states:

- Initial State
- AOO & VOO State
- AAI & VVI State
- DOO State
- AOOR & VOOR State
- AAIR & VVIR State
- DOOR State

## Device Controlled Monitor Design
