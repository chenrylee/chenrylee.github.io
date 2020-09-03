# Step-by-Step guide to rename Active Directory Domain Name
> Can I rename the domain name of an Active Directory? Of course you can!  
  
Following are the critical points you need to consider before AD rename.
## Prerequisites
1. **Forest Function Level** – Forest Function level must be windows server 2003 or higher to perform AD rename.
2. **Location of the Domain** – in forest it can have different level of domains. Those can be either complete different domains or child domains. If you going to change the location of the dc in the forest you must need to create trust relationships between domains to keep the connectivity.
3. **DNS Zone** – DNS Zone files must be created for the new domain name prior to the rename process in relevant DNS servers.
4. **Folder Path Change** – if DFS folder services or roaming profiles are setup, those paths must change in to server-based share or network share.
5. **Computer Name Change** – Once the domain is renamed the computers host names will also renamed. So if those are configured to use by applications or systems make sure you prepare to do those changes.
6. **Reboots** – Systems will need to reboot twice to apply the name changes including workstations. So be prepare for the downtime and service interruptions.
7. **Exchange Server Incompatibility** – Exchange server 2003 is the only supported version for AD rename. All other versions are not supported for this. Also there can be other applications in environment which can be not supported with rename. Make sure you access these risks.
8. **Certificate Authority (CA)** – if CA is used make sure you prepare it according to https://technet.microsoft.com/en-us/library/cc816587


## Steps
Once your infrastructure is ready, to perform the rename process we need an administrative computer or server. It must be a member of domain and should not a DC. It must have **Remote Server Administration Tools** installed. 


In demo, I am going to rename contoso.com domain to canitpro.local domain. It is runs with windows server 2012 R2.

### Install The Tools
I have prepare a server which runs windows server 2012 R2 as member server to perform the rename. You can install **Remote Server Administration Tools** by *Server manager* > *Add roles and features*. Make sure you select **AD DS and AD LDS tools** under the RSAT.
![](http://www.rebeladmin.com/wp-content/uploads/2015/05/rename1.png)  

Before we start the rename make sure forest domain activities are stopped. Such as adding new DC, changing forest configuration etc.

### Add DNS Zone
Also I went ahead and create the relevant DNS zone for new domain name in primary DNS server.   
![](http://www.rebeladmin.com/wp-content/uploads/2015/05/rename2.png)  

Then in the member server log in as domain admin and open the command prompt with admin rights.

### Rename Domain
First we need to create a report which explains the current forest setup. To do that type `rendom /list` and press enter.  
![](http://www.rebeladmin.com/wp-content/uploads/2015/05/rename3.png)


This will create an xml file with name **Domainlist.xml** in the path above command is executed. In my demo its *C:\Users\Administrator.CONTOSO*.  
![](http://www.rebeladmin.com/wp-content/uploads/2015/05/rename4.png)


To proceed it need to be edited to match with the new domain name. Make sure you save the file after edits.  
![](http://www.rebeladmin.com/wp-content/uploads/2015/05/rename5.png)


Then type `rendom /upload` command from same folder path.  
![](http://www.rebeladmin.com/wp-content/uploads/2015/05/rename6.png)


To check the domain readiness before the rename process type `rendom /prepare`.  
![](http://www.rebeladmin.com/wp-content/uploads/2015/05/rename7.png)


Once its pass with no errors, execute `rendom /execute` to proceed with rename. It will reboot all domain controllers automatically.  
![](http://www.rebeladmin.com/wp-content/uploads/2015/05/rename8.png) 

![](http://www.rebeladmin.com/wp-content/uploads/2015/05/rename9.png)


All workstations and servers will needs to reboot twice to apply changes. Username and password will not change, but the domain name will be new one.

### Rename Domain Controllers
With rename process domain controllers will not be renamed. Those need to change manually.  
![](http://www.rebeladmin.com/wp-content/uploads/2015/05/rename10.png)


It can do using command `netdom computername DC.contoso.com /add:DC.canitpro.local`  
![](http://www.rebeladmin.com/wp-content/uploads/2015/05/rename11.png)

Then type `netdom computername DC.contoso.com /makeprimary:DC.canitpro.local`, once complete, reboot the DC.  
![](http://www.rebeladmin.com/wp-content/uploads/2015/05/rename12.png)


We can see it’s changed after reboot.  
![](http://www.rebeladmin.com/wp-content/uploads/2015/05/rename13.png)


### Fix GPOs
The next thing we need to fix is the group policies. It’s still uses the old domain name.  
![](http://www.rebeladmin.com/wp-content/uploads/2015/05/rename14.png)

To fix this type and enter `gpfixup /olddns:contoso.com /newdns:canitpro.local`  
![](http://www.rebeladmin.com/wp-content/uploads/2015/05/rename15.png)  
Then run `gpfixup /oldnb:CONTOSO /newnb:canitpro`  
![](http://www.rebeladmin.com/wp-content/uploads/2015/05/rename16.png)


We done with that too. The only thing we need to run is `rendom /end` to stop the rename process and unfreeze the DC activity.  
![](http://www.rebeladmin.com/wp-content/uploads/2015/05/rename17.png)


This ends the rename process and we have a dc now with a new domain name.
