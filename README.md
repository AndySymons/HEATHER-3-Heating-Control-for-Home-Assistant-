# HEATHER Release 3
## Heating Control for Home Assistant


Project HEATHER provides a method for implementing a comprehensive smart heating system for a home of any size, based on Home Assistant.

This is Release 3 of the HEATHER project components - blueprints, scripts and example YAML code.
Because Release 3 has fundamentally changed Release 2, and to avoid losing the historical context, it is created as a separate repository. 


## Potted history

### HEATHER 1
The first release of the 'Heating X' blueprint in February 2023 introducted the idea of operating one or more home heating radiators in a room from a calendar in which the required temperature was set in the description field between two hash symbols. e.g. #21.5#.
It had energy-saving options to turn off the heating if a door or wondow was opened, or if the room was unoccupied for a while.
It introduced an associated 'Heating X Code Generator' to help create the YAML code for a multi-room installation.

The name 'HEATHER 1' was coined retrospectively to show the continuity of the project. 


### HEATHER 2 
After a year's experience with 'Heating X', the second release 'Heating X2' was made in February 2024 in which more attention was given to dealing with TRVs that did not repond correctly. 
It also introduced logging, so that operational problems could be analysed after the fact. 
To 'Heating X2' was added 'Calendar Y2', which does much the same but for on/off switches such as immerson heaters rather than TRVs that require a temperature. 
Additionally 'Zone Z2' was introduced for multi-zone installations or boilers that require an explicit heat demand switch. 

These were known collectively as 'Heating XYZ', and retrospectively also as HEATHER 2. 
The associated 'Heating XYZ Code Generator' was updated to cover all the components.

In subsequent upgrades, improvements were made such as the addition of an Away Mode and specification of a Background Temperature to be used when no calendar event was active.  


### HEATHER 3
The main purpose of HEATHER 3, released in February 2025, is to go even further in overcoming recalcitrant TRVs or switches. Rather than just reporting a failure, several attempts are made to correct it. It has been found after two years' experience that devices that do not respond the first time often respond on a second attempt. For a heating system this can be important.

However, implementation of this feature further complicates an already complex system, so the components have been split and restructured to facilitate easier testing and management. 

#### New Component Structure
- the former 'Heating X2' has been split into two parts: 
  - **R3 Room Heating Control** determines the _required_ temperature based on the Calendar, Away mode, doors or windows, occupancy and manual override, but that is all. It reports the setting reason to the dashboard as before. 
  - **T3 TRV Control** actually operates the TRV, checking the response, and if the response is not correct, retrying up to a set maximum number of times, at set intervals. It detects manual override, which it signals to R3, and it reports the TRV status separately for the dashboard. 
  - Whereas Heating X2 had one instance (automation) per room, there is now one instance of **R3 Room Heating Control** for the room, and one **T3 TRV Control** for _each of the TRVs_ in the room. This at least doubles the number of automations required, but is actually easier for me to maintain.
    
- the former 'Calendar Y2' has been similarly split into two parts: 
  - **C3 Calendar Control** determines the _required_ switch state based on the Calendar, Away mode, and manual override, but that is all. It reports the setting reason to the dashboard as before. 
  - **S3 Secure Switch Control** actually operates the switch, checking the response, and if the response is not correct, retrying up to a set maximum number of times, at set intervals. It detects manual override, which it signals to C3, and it reports the switch status separately for the dashboard. 
  - Whereas Calendar Y2 had one instance (automation) per device, there is now one instance of **C3 Calendar Control** for the setting, and one **S3 Secure Switch Control** for _each_ of the associated switch devices (which is generally just one in practice, but could be more if required). This doubles the number of automations, but is easier to maintain and allows **S3 Secure Switch Control** to be re-used also for zone switches...
    
- the former 'Zone Z2' has been similarly split into two parts, but one is re-used: 
  - **Z3 Zone Control** determines the _required_ zone switch state based on Heat Demand for the zone and an optional Kill Switch, but that is all. It reports the setting reason to the dashboard as before. Manual override is not permitted for a zone switch, so if S3 reports a manual override, it reports a warning and does not implement it. 
  - **S3 Secure Switch Control** (as above) actually operates the zone valve switch, checking the response, and if the response is not correct, retrying up to a set maximum number of times, at set intervals. It reports the switch status separately for the dashboard. It signals manual interventon to Z3 as usual, but Z3 will block it.
  - Whereas 'Zone Z2' had one instance (automation) per zone, there is now one insance of **Z3 Zone Control** to detrmine the setting for the zone, and one **S3 Secure Switch Control** for _each_ of the associated switch devices (which is generally one in practice, but could be more if required). This inconveniently doubles the number of automations, but is easier to maintain and allows **Heating S3** to be re-used with **C3 Calendar Control**, or indeed as a stand-alone secure switching facility.  

#### New Installation Method 
With this new component structure, the Code Generator became too difficult to make and maintain, so is dropped. Instead, I now advocate the Home Assistant [Packages](https://www.home-assistant.io/docs/configuration/packages/) facility. 
I use a common base folder _/packages/heating_, then add a YAML file (subpackage) for each room, for each piece of switched equipment such as immersion heaters, and for each zone. 
- For rooms, the YAML contains the numerous helper definitions and template sensors for R3 and one or more T3s as applicable.
- For switched equipment, the YAML file contains the numerous helper definitions for C3 and one S3 (or more if applicable). 
- For zones, the YAML file contains the numerous helper definitions for Z3 and one S3 (or more if applicable). 

With this structure it is easy, for each room / equipment / zone, to copy an example from this repository and use your favourite YAML editor to 'replace all' for the room / equipment / zone name. 

Whereas the installation manual was previous provided as a PDF file, it is now in the Wiki for this repository for ease of access, maintenance, and comment (pending). 

