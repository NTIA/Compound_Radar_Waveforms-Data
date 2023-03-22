# Data for NTIA Technical Memorandum TM 23-XXX
### Examining the Effects of Resolution Bandwidth when Measuring Compound Radar Waveforms
[![DOI](https://zenodo.org/badge/540043368.svg)](https://zenodo.org/badge/latestdoi/XXXXXXXXXX)

## Description
The data associated with NTIA TM-23-XXX is contained in this repository. The data directories follow a format similar to the report with individual directories for the P0N, Q3N, and compound test cases. Each of these directories contains the measured spectrum data contained in the "Stepped" files and a folder containing all of the figures used in the Tech Memo.

## Data Format

### Stepped Measurement Files

The spectrum data is contained in Matlab files (.mat) that each contain the waveform number and the word 'Stepped' in the filename. The additional tests that were not given a waveform name will have text in the filename indicating which test case was associated with that file.
Each RBW was collected as an "event" in the Stepped measurement file so there are a total of 12 "events" for each test case. A description of the Stepped measurement algoritm is provided in NTIA Technical Report 05-420.

The .mat files may be loaded into Matlab via the command:

```Matlab
load('path\to\file\filename.mat)
```

The variables contained within the measurement files follow this format:
1. **CalPathandFileName**: provides the directory and filename for the calibration file used to apply calibration corrections to the saved data as a single string
    1. Will be empty (‘ ‘) if calibration corrections were not applied to the data
    2. Ex. ‘C:\temp\Calfile1.mat’
  
2. **Comments**:  contains comments entered by the user using the edit menu in the Stepped measurement

3. **CompleteMeasMessage**: String indicating how the measurement arrived at completion. There are three possibilities:
	1. ‘The measurement completed successfully’ – indicates that it ran and completed normally
	2. ‘The measurement stopped prematurely due to error’ – indicates that a severe error occurred which required the measurement to stop
		1. Data may not be saved properly
		2. Most predictable errors are handled seamlessly by the measurement software, but some, such as instrument communication failures, can’t be dealt with
	3. ‘The measurement was stopped prematurely by user’ – indicates that the user stopped the measurement before it was complete
		1. All data up to the time the measurement was stopped will be saved however, there may be some preallocated variables that won’t contain information regarding sweeps that never got completed

4. **ErrorLog**: (# of errors)x(2) cell array containing information regarding errors that occurred during the measurement
	1. First column: MException object of the error, which contains a lot of information regarding the error.
	2. Second column: time and date stamp from the RSMS-5G computer indicating when the error occurred
	
5. **event**: struct containing all of the data collected for each measurement event as specified in the event table. 
   Each of the 12 events in the file is associated with a different resolution bandwidth. 
   
	| Event # | Bandwidth |
	|:-------:|:---------:|
	|1        |   100 Hz  |
	|2        |   300 Hz  |
	|3        |   1 kHz   |
	|4        |   3 kHz   |
	|5        |  10 kHz   |
	|6        |  30 kHz   |
	|7        |  100 kHz  |
	|8        |  300 kHz  |
	|9        |    1 MHz  |
	|10       |    3 MHz  |
	|11       |    6 MHz  |
	|12       |    8 MHz  |
		
	1. Data for each event is indexed as such: event(\<event number\>).\<Field Name\>
		1. Fields:
			1. **ManualAttenEnabled**: vector of Booleans indicating whether or not the measurement was in manual attenuation mode or not for each corresponding step
				1. This can be changed while an event is running so this keeps track
			2. **MeasNotes**: a string containing the text entered into the measurement notes edit text box in the Stepped measurement GUI while the measurement was running
			3.  **FreqMHz**: the frequency vector, in MHz, for the specified event
			4.  **UnCorrectedMagdBm**: raw data, without primary preselector or spectrum analyzer attenuation accounted for
			5.  **AttenCorrectedMagdBm**: data that has been corrected for primary preselector or spectrum analyzer attenuation level
			6.  **ExceptionPoints**: vector of Booleans that indicate whether or not a specific step had an error
				1. Errors can be caused by zero-dynamic range problem (measurement unable to determine proper attenuation level) or overloads when no more attenuation is available or measurement is in manual attenuation mode
			7.  **Atten**: vector containing the primary preselector or spectrum analyzer attenuation used for each point.
			8.  **CompletionTime**: time and date stamp from RSMS-5G computer indicating when the event completed
			9.  **RawMagTraceMatrix**: Contains a matrix of all the spectrum analyzer sweeps used to create the overall stepped measurement spectrum.
			10. **CalCorrectedMag**: data that has been corrected for primary preselector or spectrum analyzer attenuation level and calibration system gain data
			11. **PulseParamTraceData**: struct containing data taken when the pulse parameter feature is used. It will be empty, [], if pulse data is not taken.
				1. Fields:
					1. **PeaksMatrix**: (# of peaks)x(3) matrix
	
						| Column # | Value                                   |
						|:---------|:----------------------------------------|
						| 1        | frequency value                         |
						| 2        | magnitude value                         |
						| 3        | index of the peak in the FreqMHz vector |
	
					2. **AntPatternTimeVect**: time vector for the antenna pattern trace
					3. **AntPattAttenCorrMagData**: magnitude data, for the antenna pattern trace, with primary preselector or spectrum analyzer attenuation taken into account
					4. **RotationRateVal**: average rotation rate in seconds
					5. **PRITimeVect**: time vector for the PRI trace
					6. **PRIAttenCorrMagData**: magnitude data, for the PRI trace, with primary preselector or spectrum analyzer attenuation taken into account
					7. **PRIVal**: measured average pulse repetition interval in seconds
					8. **PWTimeVect**: time vector for the trace used to calculate single pulse parameters
					9. **PWAttenCorrMagData**: magnitude data, for the trace used to calculate single pulse parameters, with primary preselector or spectrum analyzer attenuation taken into account
					10. **PWVal**: pulse width in seconds
					11. **tRise**: pulse rise time in seconds
					12. **tFall**: pulse fall time in seconds
				2. If the measurement is unable to determine any of these fields, the affected field will contain the empty set, [].
	
	
6. **EventParamIdx**: struct with fields containing indices into **EventTableData** for each of its parameters
	
	| Field          | Index |
	|:---------------|------:|
	| fStartMHz      |  1    |
	| fStopMHz       |  2    |
	| RBWMHz         |  3    |
	| VBWMHz         |  4    |
	| Det            |  5    |
	| SweepTimems    |  6    |
	| NumPoints      |  7    |
	| PreampOn       |  8    |
	| AutoSteps      |  9    |
	| NumSteps       | 10    |
	| Presel1AntPort | 11    |
	| Presel1Path    | 12    |
	| Presel2AntPort | 13    |
	| Presel2Path    | 14    |
	| Presel2Atten   | 15    |
	
7. **EventTableData**: (# events)x(10) cell array containing all the information in the event table of the Swept measurement
	1. Row: event number (1-12)
	2. Columns follow this format:	
		| Column # | Value                                                                                                  |
		|:--------:|:-------------------------------------------------------------------------------------------------------|
		| 1        | start frequency (MHz)                                                                                  |
		| 2        | stop frequency (MHz)                                                                                   |
		| 3        | resolution bandwidth (MHz)                                                                             |
		| 4        | video bandwidth (MHz)                                                                                  |
		| 5        | spectrum analyzer detector                                                                             |
		| 6        | sweep time (ms)                                                                                        |
		| 7        | number of sweep points                                                                                 |
		| 8        | preamp state                                                                                           |
		| 9        | Auto steps state (when checked, the number of steps is automatically calculated from the span and RBW) |
		| 10       | number of steps                                                                                        |
		| 11       | primary preselector antenna port                                                                       |
		| 12       | primary preselector filter type                                                                        |
		| 13       | secondary preselector antenna port                                                                     |
		| 14       | secondary preselector filter type                                                                      |
		| 15       | secondary preselector static attenuation level                                                         |
		
		
8. **FileNumber**: a number assigned by the measurement to this saved data file (determined based on other existing Stepped data files in the same directory)

9. **HardwareConfig**: struct containing instrument IDs of all the connected instruments. Unused instrument fields will contain the string ‘None’.
	| Field      | Description           |
	|:-----------|:----------------------|
	| SpecAn     | spectrum analyzer     |
	| Presel1    | primary preselector   |
	| Presel2    | secondary preselector |
	| YIGTracker | YIG tracker           |
	
10. **MeasStartTime**: a string containing the time and date (from computer) that the measurement was started (ex. 05-Apr-2013 17:54:42)

11. **MeasType**: string indicating the type of measurement, ‘Stepped’

12. **NumEvents**: number of events run by the measurement



## Sponsor

## References ##

* [NTIA Technical Memorandum 23-XXX](<URL>)
* [NTIA Technical Report 05-420](https://its.ntia.gov/umbraco/surface/download/publication?reportNumber=TR-05-420.pdf)

## Contact ##

For questions, contact Geoff Sanders, (720) 552-7567, gesanders@ntia.gov
