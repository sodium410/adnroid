Static analysis is a technique used to evaluate the security of an application by examining its source code without executing it.  
Benefits -- early detection, complete coverage - app code not just the features  

**Types of Vulnerabilities that Static Analysis can detect**  
Insecure storage - data stored in locations other apps or users can access  
Hard-coded sensitive information -- such as passwords api keys  
Insecure communication -- use of HTTP  
Insufficient cryptography -- Use of tls1.0 weak cyphers  
Insecure permissions -- requesting more perms than necessary giving app access to sensitive data  

**APK Sources**  
Google play store, Manufacturers play stores -- samsung galaxy store, amazon appstore, vivo app store etc  

**APK extraction**   
To install from sd card or unknown source -- enable  Settings -> Security -> Unknown sources  
APK download from Third party app stores  -- https://apkcombo.com  
Extraction using third-party tools --  APK Export  - install this to export any app apk file  
Extracting The APK From The Device -- /data/app/com.example.myapp-1/base.apk  
Access to the /data/app/ directory is restricted for non-rooted users, which makes it difficult to explore or guess full package paths.  
If package name is known get the path --  adb shell pm path com.example.myapp  
if package name is not known try adb shell pm list packages | grep myapp  // to list package name first  
Once path confirmed download the apk -- adb pull /data/app/com.example.myapp-1/base.apk .  

**Disassembling the APK**  
