# ASD Essential 8 Add On for Splunk
## Overview
This add on was created to allow customers of Splunk to use their data to report on Essential 8.  This add on contains data models that are used in the ASD Essential 8 App for Splunk.

## Deployment
Reach out to the canberra team via email canberra[@]splunk[.]com to have our engineering team discuss implementation with you.


### ASD App Steps

### Windows TA Modification
Modified tags.conf of Windows TA
[eventtype=windows_privileged_service_call]
process = enabled
execute = enabled
start = enabled
privileged = enabled
authentication = enabled # Added

[eventtype=windows_privileged_object_operation]
resource = enabled
execute = enabled
start = enabled
privileged = enabled
authentication = enabled # Added

#### Hash out privileged in windows logon explicit
[eventtype=windows_logon_explicit]
authentication = enabled
privileged = disabled

#### Correct Props for Windows TA
Modified Props.conf for Windows TA
EVAL-src_user=mvindex(Security_ID,0)
EVAL-dest_user=mvindex(Security_ID,1)

[XmlWineventlog:Security]
rename SubjectUserSid as src_user, TargetSid as dest_user






