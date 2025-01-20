# HEATHER Release 3
## Heating-Control-for-Home-Assistant-

This is Release 3 of the HEATHER project components. Since it has fundamentally changed (and to avoid losing the historical context) it is create as a new repository.

### Potted history

#### HEATHER 1
The first release of 'Heating X' in February 2023 introducted the idea of operating one or more home heating radiators in a room from a calendar in which the required temperature was set in the description field between two hash symbols. e.g. #21.5# 
It had energy-saving options to turn off the heating if a door or wondow was opened, or if the room was unoccupied for a while.  
It introduced an associated 'Heating X Code Generator' to help create the YAML code for a multi-room installation. 
The name 'HEATHER 1' was only added retrospectively to show the continuity of the project. 

#### HEATHER 2 
After a year's experience with 'Heating X', the second release 'Heating X2' was made in February 2024 in which more attention was given to dealing with TRVs that did not repond correctly. 
It also introduced logging, so that operational problems could be analysed after the fact. 
To 'Heating X2' was added 'Calendar Y2', which does much the same but for on/off switches such as immerson heaters rather than TRVs that require a temperature. 
Additionally 'Zone Z2' was introduced for multi-zone installations or boilers that require an explicit heat demand switch. 
These were known collectively as 'Heating XYZ', and retrospectively also as HEATHER 2. 
The associated 'Heating XYZ Code Generator' was updated to cover all the components.
In subsequent upgrades, improvements wer emade such as the addition of an Away Mode and specification of a Background Temperature to be used when no calendar event was active.  

#### HEATHER 3
The main purpose of HEATHER 3 is to go even further in overcoming recalcitrant TRVs or switches. Rather than just reporting a failure, several attempts are made to correct it. It has been found after two years' experience that devices that do not respond the first time often respond on a second attempt. For a heating system this can be important. 
Implementation further complicates and already complex system, so it was restructured to facilitate easier testing and management. 
. Heating X2 was split into two parts: 
  . Heating R3 determines the _required_ temperature based on the calendar, Away mode, doors or windows and occupancy, but that is all. It reports the setting reason to the dashboard as before. 
  . Heating T3 atually operates the TRV, checking the response, and retrying up to a set maximum number of times, as set intervals, if the response is not correct.
