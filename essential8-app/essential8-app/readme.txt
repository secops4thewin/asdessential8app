ASD App Steps

I have modified the Windows TA Default Eventgen which is reffering to some new usernames.  We will need to grab that.
I renamed the privilege 4674 event so that the admins are controlled

Modified props.conf of Windows TA for App Locker Events
Install Okta add on for eventgen.

#############
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

#Hash out privileged in windows logon explicit
[eventtype=windows_logon_explicit]
authentication = enabled
#privileged = enabled

#############
Modified Props.conf for Windows TA
EVAL-src_user=mvindex(Security_ID,0)
EVAL-dest_user=mvindex(Security_ID,1)

[XmlWineventlog:Security]
rename SubjectUserSid as src_user, TargetSid as dest_user

#############

#Group Search
|datamodel Change_Analysis Account_Management search | search TaskCategory="*Group Management"  |head 10| `drop_dm_object_name("All_Changes")` | eval src_user=mvindex(Security_ID,0) | eval dest_user=mvindex(Security_ID,1) | eval group=mvindex(Security_ID,2)| table _time,action, src_user,dest_user,group,EventCodeDescription | rename action as Action, src_user as "Source User", dest_user as "Destination User", group as Group, EventCodeDescription as "Activity Action"


##############
Lookup is not used.  It is meant to display potential values

#### 
To Do
Fix up priv eventgen - Done
Contact Jack Coates re - src_user / dest_user. - Done
Contact Jack Coates about updating privilege  - Done
Create a report category group for Advertisements Bluecoat
Create a report group for Macros
Update Okta to have eventgen that has hostnames and usernames that I have in Windows TA


####
Dump
| pivot Change_Analysis Accounts_Deleted count(Accounts_Deleted) AS "Count of Deleted Accounts" SPLITROW _time AS _time PERIOD auto SORT 100 _time ROWSUMMARY 0 COLSUMMARY 0 NUMCOLS 0 SHOWOTHER 1