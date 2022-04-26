# Uninstall Microsoft AutoUpdate(MAU) on Mac
***
Sometimes, Microsoft AutoUpdate is really annoying, and you might want to kick it out of your computer. There're tons of guides, and this one will give you a detailed, full and all-in-one guide. OK, here we go!  
1. Check if MAU is running.  
   ```powershell
   PS > Get-Process -Name *Microsoft*
   ```
   If the result doesn't return MAU, it means MAU isn't running.
2. Remove the application
   MAU application is located in `/Library/Application Support/Microsoft/MAU2.0/Microsoft AutoUpdate.app`, so, in this step we will delete this folder.
   ```powershell
   PS > sudo rm -rf '/Library/Application Support/Microsoft/MAU2.0/Microsoft AutoUpdate.app'
   ```
   You may be prompted for password.
3. Deleted related files from System-Library  
   ```powershell
   PS > sudo rm -rf '/Library/LaunchAgents/com.microsoft.update.agent.plist'
   PS > sudo rm -rf '/Library/PrivilegedHelperTools/com.microsoft.autoupdate.helper'
   ```
4. Delete related files from User-Library  
   ```powershell
   PS > rm -rf '~/Library/Preferences/com.microsoft.autoupdate.fba.plist'
   PS > rm -rf '~/Library/Preferences/com.microsoft.autoupdate2.plist'
   PS > rm -rf '~/Library/Caches/com.microsoft.autoupdate.fba/'
   PS > rm -rf '~/Library/Caches/com.microsoft.autoupdate2/'
   ```
5. A reboot is recommended.
