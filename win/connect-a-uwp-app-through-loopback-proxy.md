# Connect A UWP App Through Loopback Proxy
On Windows 10, UWP apps are isolated from loopback proxy, but in some scenarios, loopback proxy is required, for instance, if you want to connect Unigram in China.  
Is there a way to connect UWP apps through a loopback proxy? Yes!  


There's a built-in command, `CheckNetIsolation`, with module `LoopbackExempt`, the explanation is:  
>controls the loopback exemption of AppContainers and Package Families to ease application development.  

OK, let's do it. The usage of this command is as following:
```
Usage:
   CheckNetIsolation LoopbackExempt [operation] [-n=] [-p=]
      List of operations:
          -a  -  Add the AppContainer or Package Family to the loopback
                 exempted list.
          -d  -  Delete an AppContainer or Package Family from the
                 loopback exempted list.
          -c  -  Clear the list of loopback exempted AppContainers and
                 Package Families.
          -s  -  Show a list of loopback exempted AppContainers and
                 Package Families.

      List of arguments:
          -n= - AppContainer Name or Package Family Name.
          -p= - AppContainer or Package Family Security Identifier (SID).
          -?  - Displays this help message for the LoopbackExempt module.
```
1. **Get the package family name**. We do it by a PowerShell cmdlet:
```powershell
Get-AppxPackage -Name *Unigram*
```
```
Name              : 38833FF26BA1D.UnigramPreview  
Publisher         : CN=D89C87B4-2758-402A-8F40-3571D00882AB  
Architecture      : X64 
ResourceId        : 
Version           : 3.14.2665.0  
PackageFullName   : 38833FF26BA1D.UnigramPreview_3.14.2665.0_x64__g9c9v27vpyspw  
InstallLocation   : C:\Program Files\WindowsApps\38833FF26BA1D.UnigramPreview_3.14.2665.0_x64__g9c9v27vpyspw  
IsFramework       : False  
PackageFamilyName : 38833FF26BA1D.UnigramPreview_g9c9v27vpyspw
PublisherId       : g9c9v27vpyspw  
IsResourcePackage : False  
IsBundle          : False  
IsDevelopmentMode : False  
NonRemovable      : False  
Dependencies      : {Microsoft.UI.Xaml.2.3_2.31912.11002.0_x64__8wekyb3d8bbwe,  
                    Microsoft.NET.Native.Framework.2.2_2.2.27912.0_x64__8wekyb3d8bbwe,  
                    Microsoft.NET.Native.Runtime.2.2_2.2.27328.0_x64__8wekyb3d8bbwe,  
                    Microsoft.VCLibs.140.00_14.0.27810.0_x64__8wekyb3d8bbwe…}  
IsPartiallyStaged : False  
SignatureKind     : Store  
Status            : Ok  
```

Now you can find the `PackageFamilyName` is `38833FF26BA1D.UnigramPreview_g9c9v27vpyspw`.
  
  
2. **Add to loopback exemption**. 
```bat
CheckNetIsolation loopbackexempt -a -n="38833FF26BA1D.UnigramPreview_g9c9v27vpyspw"
OK
```
  
  
3. **Confirm the result**.
```bat
CheckNetIsolation loopbackexempt -s

List Loopback Exempted AppContainers

[1] -----------------------------------------------------------------
    Name: 38833ff26ba1d.unigrampreview_g9c9v27vpyspw
    SID:  S-1-15-2-864468249-2750725007-4163465496-967253594-3023145886-2867458848-3864617232
```

Now, it works through loopback proxy.


If you want to isolate the app again, just delete it from the exemption:
```bat
CheckNetIsolation loopbackexempt -d -n="38833FF26BA1D.UnigramPreview_g9c9v27vpyspw"
OK
```
