# Use My Old Password in Big Sur
Apple released the new MacOS, Big Sur, AKA Bug Surprise.  
After upgrading to the new OS, I was pissed off by the new features:
- Touch ID didn't work.
- Old password didn't work.

Lock the screen and re-sign in, I was prompted to change password! My password is a 6-digit simple password, and it required my new password with compex combinations.

Thank God! There's a solution.  
1. Start Terminal;
2. Type follow command:  
   ```bash
   pwpolicy -clearaccountpolicies
   ```
3. You will be asked to enter your password for your admin Profile.  
   ⚠ Terminal WILL NOT show the password as you type it.
4. You're all done! Now go to System Preferences -> Users & Groups to reset your password.