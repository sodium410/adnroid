Static analysis is a technique used to evaluate the security of an application by examining its source code without executing it.  
Benefits -- early detection, complete coverage - app code not just the features  

## Types of Vulnerabilities that Static Analysis can detect  
Insecure storage - data stored in locations other apps or users can access  
Hard-coded sensitive information -- such as passwords api keys  
Insecure communication -- use of HTTP  
Insufficient cryptography -- Use of tls1.0 weak cyphers  
Insecure permissions -- requesting more perms than necessary giving app access to sensitive data  

**APK Sources**  
Google play store, Manufacturers play stores -- samsung galaxy store, amazon appstore, vivo app store etc  

## APK extraction  
To install from sd card or unknown source -- enable  Settings -> Security -> Unknown sources  
APK download from Third party app stores  -- https://apkcombo.com  
Extraction using third-party tools --  APK Export  - install this to export any app apk file  
Extracting The APK From The Device -- /data/app/com.example.myapp-1/base.apk  
Access to the /data/app/ directory is restricted for non-rooted users, which makes it difficult to explore or guess full package paths.  
If package name is known get the path --  adb shell pm path com.example.myapp  
if package name is not known try adb shell pm list packages | grep myapp  // to list package name first  
Once path confirmed download the apk -- adb pull /data/app/com.example.myapp-1/base.apk .  

## Disassembling the APK   
analyze the content of an APK file by using tools to decompile the app's source code to a more human-readable language,  
then decode the assets and resources to read any configuration files.  
APKTool allows us to decode the APK's resources, also supports rebuilding the APK after changes have been made.  
apktool d myapp.apk  //disassemble the apk with dir of same name in current dir with decooded files  
why apk why not open with 7zip ? because example - manifest file is encoded in APKs and cannot be read by simply unzipping the APK  

## Understanding the Manifest  
mousepad myapp/AndroidManifest.xml  
**Package Name** -- package="com.example.myapp" -- uniquely identifies the application  
also reveals target SDK version, different Android SDK versions may present different security implications.  

**Permissions** -- app request users to allow specif perms those declared here, poor security hygiene may request more permissions than necessary  
<uses-permission android:name="android.permission.INTERNET"/>  -- many others related to camera, storage, location, sms, contacts etc google  

**Application Class**  -- has many declarations one of the important ones are  
android:name="com.example.myapp.Init" means the Init class will run immediately when the app starts and before the main activity or user interacts.  
Developers usually use this for initializations but if vulnerable 3rd party is loaded 1st then a finding.  
Network Security config - is another one -- android:networkSecurityConfig="@xml/network_security_config  
specify custom trusted Certificate Authorities, permit or deny cleartext (HTTP) traffic, and other network-related settings  
vim myapp/res/xml/network_security_config.xml  -- shows the custom cert in use  
if cert pinning in place defined in this file as -- <pin digest="SHA-256">7HIpactkIAq2Y49orFOOQKurWxmmSFZhBCoQYcRhJ3Y=</pin>  
cert pinning is to ensure app talks to legit app server, ensures integrity of comms. need to modify this to use burp for interception add its cert  
ls -l myapp/res/raw  //network config file points to where cert is located  

**Components**  -- shows the activities of the application.  
<activity android:exported="true" android:name="com.example.myapp.MainActivity">  -- name of activity  
exported=true  activity is accessible from outside the app - can be launched from adb  
<action android:name="android.intent.action.MAIN"/> -- indicates main entry point of the application, the first screen launched when the user taps the app icon  

Followed by content providers and a service and use of Deep links(which are declared as activity with intent filters)  

**Application resources**  
The directory res contains resources like the app's layout, UI strings, images, and other static files - network setting  
notice the directory values within the res directory. This directory contains XML files with hardcoded strings and integers,  
such as Strings.xml, which stores UI strings as key-value pairs -- check for any sensitive creds, keys etc  
Also examine under res layout, drawabale, xml sub directories  

## Understanding Smali  
When an Android application is compiled, the Java compiler (javac) compiles .java files into Java bytecode (.class files).  
Likewise, Kotlin source files are compiled into .class files by the Kotlin compiler (kotlinc).  
These are then translated into Dalvik bytecode, and the output is bundled into .dex (Dalvik Executable) files.  
then packaged into apk with compiled code, resources, and manifest  
in APKtool baksmali is used to disassemble the Dalvik bytecode into a human-readable format known as Smali.  
Smali is a low-level, assembly-like language used by the Dalvik Virtual Machine and Android Runtime  

To be continued switching to Dynamic analysis  
























