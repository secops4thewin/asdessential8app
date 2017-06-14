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

