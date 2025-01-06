When using [Sysinternals Autologon](https://learn.microsoft.com/en-us/sysinternals/downloads/autologon) you may encounter a scenario in which you would like to prevent the end user from switching users or logging out of the Autologon account without jumping through some 
additional hoops.

1.) Open up regedit as administrator.

2.) Navigate to HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon .

3.) Create a new string with the name ForceAutoLogon. If the string entry is already present skip to step 4.

4.) Set the value of the new entry to 1.

5.) Exit regedit.

6.) Verify by signing out of the account. It should immediately force the autologon account to sign back in.
