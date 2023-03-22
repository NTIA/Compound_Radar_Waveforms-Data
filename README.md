# Data for NTIA Technical Memorandum TM 23-XXX
### Examining the Effects of Resolution Bandwidth when Measuring Compound Radar Waveforms
[![DOI](https://zenodo.org/badge/540043368.svg)](https://zenodo.org/badge/latestdoi/540043368)

The data associated with NTIA TM-23-XXX is contained in this repository. The data directories follow a format similar to the report with individual directories for the P0N, Q3N, and compound test cases. Each of these directories contains the measured spectrum data contained in the "Stepped" files and a folder containing all of the figures used in the Tech Memo. 

## Stepped Measurement Files ##

The spectrum data is contained in Matlab files (.mat) that each contain the waveform number and the word Stepped in the filename. The additional tests that were not given a waveform name will have text in the filename indicating what the test case was associated with that file.
Each RBW was collected as an "event" in the Stepped measurement file so there are a total of 12 "events" for each test case. A description of the Stepped measurement algoritm is provided in NTIA Technical Report 05-420.

The variables contained within the measurement files follow this format:

1.	CalPathandFileName: provides the directory and filename for the calibration file used to apply calibration corrections to the saved data as a single string
	a.	Will be empty (‘ ‘) if calibration corrections were not applied to the data
	b.	Ex. ‘C:\temp\Calfile1.mat’
2.	Comments:  contains the user comments entered by the user using the edit menu in the Stepped measurement
3.	CompleteMeasMessage: String indicating how the measurement arrived at completion
	a.	Three possibilities
	i.	‘The measurement completed successfully’ – indicates that it ran and completed normally
	ii.	‘The measurement stopped prematurely due to error’ – indicates that a severe error occurred that required the measurement to stop
		1.	Data may not be saved properly
		2.	Most predictable errors are handled seamlessly by the measurement but some, such as instrument communication failures, can’t be dealt with
	iii.	‘The measurement was stopped prematurely by user’ – indicates that the user stopped the measurement before it was complete
		1.	All data up to the time the measurement was stopped will be saved, however there may be some preallocated variables that won’t contain information regarding sweeps that never got completed
4.	ErrorLog: number of errors x 2 cell array containing information regarding errors that occurred during the measurement
	a.	First column = MException object of the error, which contains a lot of information regarding the error.
	b.	Second column = time and date stamp from the 5G computer indicating when the error occurred
5.	EventTableData: cell array containing all of the information contained in the event table of the Swept measurement
	a.	Rows = event numbers
	b.	Columns follow this format in this order:
		i.	Start frequency in MHz
		ii.	Stop frequency in MHz
		iii.	RBW in MHz
		iv.	VBW in MHz
		v.	Spectrum analyzer detector
		vi.	Sweep time in ms
		vii.	Number of sweep points
		viii.	Preamp state
		ix.	Auto steps state
			1.	When checked the number of steps is automatically calculated from the span and RBW
		x.	Number of steps
		xi.	Primary preselector antenna port
		xii.	Primary preselector filter type
		xiii.	Secondary preselector antenna port
		xiv.	Secondary preselector filter type
		xv.	Secondary preselector static attenuation level
6.	EventParamIdx: struct containing indices into the event table for specific parameters described in 2b(i-xv)
	a.	Fields:
		i.	fStartMHz = 1 (first column in event table)
		ii.	fStopMHz = 2 (second column in event table)
		iii.	RBWMHz = 3 (third column in event table)
		iv.	VBWMHz = 4 (fouth column in event table)
		v.	Det = 5 (fifth column in event table)
		vi.	SweepTimems = 6 (follows the pattern for first five as does everything that follows this)
		vii.	NumPoints = 7
		viii.	PreampOn = 8
		ix.	AutoSteps = 9
		x.	NumSteps = 10
		xi.	Presel1AntPort = 11
		xii.	Presel1Path = 12
		xiii.	Presel2AntPort = 13
		xiv.	Presel2Path = 14
		xv.	Presel2Atten = 15
7.	FileNumber: a number assigned by the measurement to this saved data file (determined based on other existing Stepped data files in the same directory)
8.	HardwareConfig: struct containing descriptions of all the connected instruments
	a.	Fields:
		i.	SpecAn: Spectrum analyzer
		ii.	Presel1: Primary preselector
		iii.	Presel2: Secondary preselector
		iv.	YIGTracker: YIG Tracker
	b.	Unused instrument fields will contain the string ‘None’
9.	MeasStartTime: a string containing the time and date (from computer) that the measurement was started (ex. 05-Apr-2013 17:54:42)
10.	MeasType: string indicating the type of measurement, ‘Stepped’
11.	NumEvents: number of events run by the measurement
12.	event: struct containing all of the data collected for each measurement event as specified in the event table 
	a.	Data for each event is indexed as such: event(<event number>).<Field Name>
		i.	Fields:
			1.	ManualAttenEnabled: vector of Booleans indicating whether or not the measurement was in manual attenuation mode or not for each corresponding step
				a.	This can be changed while an event is running so this keeps track
			2.	MeasNotes: a string containing the text entered into the measurement notes edit text box in the Stepped measurement GUI while the measurement was running
			3.	FreqMHz: the frequency vector, in MHz, for the specified event
			4.	UnCorrectedMagdBm: raw data, without primary preselector or spectrum analyzer attenuation accounted for
			5.	AttenCorrectedMagdBm: data that has been corrected for primary preselector or spectrum analyzer attenuation level
			6.	ExceptionPoints: vector of Booleans that indicate whether or not a specific step had an error
				a.	Errors can be caused by zero-dynamic range problem (measurement unable to determine proper attenuation level) or overloads when no more attenuation is available or measurement is in manual attenuation mode
			7.	Atten: vector containing the primary preselector or spectrum analyzer attenuation used for each point.
			8.	CompletionTime: time and date stamp from RSMS-5G computer indicating when the event completed
			9.	RawMagTraceMatrix: Contains a matrix of all the spectrum analyzer sweeps used to create the overall stepped measurement spectrum.
			10.	CalCorrectedMag: data that has been corrected for primary preselector or spectrum analyzer attenuation level and calibration system gain data
			11.	PulseParamTraceData: struct containing data taken when the pulse parameter feature is used – will be empty, [],  if pulse data is not taken
				a.	Fields:
					i.	PeaksMatrix: number of peaks x 3 matrix
						1.	Column 1 = Frequency Value
						2.	Column 2 = Magnitude Value
						3.	Column 3 = idx of the peak into the FreqMHz vector
					ii.	AntPatternTimeVect: time vector for the antenna pattern trace
					iii.	AntPattAttenCorrMagData: magnitude data, for the antenna pattern trace, with primary preselector or spectrum analyzer attenuation taken into account
					iv.	RotationRateVal: average rotation rate in seconds
					v.	PRITimeVect: time vector for the PRI trace
					vi.	PRIAttenCorrMagData: magnitude data, for the PRI trace, with primary preselector or spectrum analyzer attenuation taken into account
					vii.	PRIVal: measured average pulse repetition interval in seconds
					viii.	PWTimeVect: time vector for the trace used to calculate single pulse parameters
					ix.	PWAttenCorrMagData: magnitude data, for the trace used to calculate single pulse parameters, with primary preselector or spectrum analyzer attenuation taken into account
					x.	PWVal: pulse width in seconds
					xi.	tRise: pulse rise time in seconds
					xii.	tFall: pulse fall time in seconds
				b.	If the measurement is unable to determine any of these fields the affected field will contain the empty set, []

Each of the 12 events in the files is associates with a different RBW. Event 1 = 100 Hz, Event 2 = 300 Hz, Event 3 = 1 kHz, Event 4 = 3 kHz, Event 5 = 10 kHz, Event 6 = 30 kHz, Event 7 = 100 kHz, Event 8 = 300 kHz, Event 9 = 1 MHz, Event 10 = 3 MHz, Event 11 = 6 MHz, Event 12 = 8 MHz.

## Sponsor

## References ##

* [NTIA Technical Memorandum 23-XXX](<URL>)
* [NTIA Technical Report 05-420](https://its.ntia.gov/umbraco/surface/download/publication?reportNumber=TR-05-420.pdf)

## Contact ##

For questions, contact Geoff Sanders, (720) 552-7567, gesanders@ntia.gov
