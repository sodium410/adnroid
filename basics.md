Android -- is a mobile operating system created for touchscreen devices like phones now for TVs.  
Based on a modified version of the Linux Kernel.  
Pmary hardware architecure used in majority android devices is ARM - Advanced RISC(reduced instruction set computing) machine  
ARM is a type of chip architecture that focuses on being physically small and using the least amount of electricity as possible to do the work it is instructed.  

**Android Plaform stack** consists of 6 components  
Linux kernel -- wifi, display, audio, keypad, bluetooth, USB, camera  
Hardware Abstraction layer(HAL) -- software layer for interacting with the hardware components  
Android Runtime -- 
is the managed runtime environment used by the Android operating system to execute applications.  
can execute apps packaged in .dex dalvik executable format - maintains backward compatibility with applications originally built for Dalvik  
Dalvik was the default runtime environment in Android versions prior. It was eventually replaced by the Android Runtime (ART)  
Native C/C++ libraries -- Android components like ART and HAL are created using native code, and in order for these components to work,  
access to native libraries is needed. Applications can access these libraries through the Java Native Interface (JNI)  
java APi framework -- This component provides software tools and interfaces for building Android applications example: activity, resource,locataion manager, content providers  
System apps -- preinstalled apps - phone, messages, play store, calendar etc  

**Rooting**  
Android flash storage has 2 portions -- /system/ OS parition, and /data/ -- used for user data and application installs  
root -- full access enhanced capabilities - also more susceptable to malware since rooting disables some built-in security  

**Important Directories**  --these now vary from device to device  
/system/app  -- contains the pre-installed apps  
/data/app -- apks installed by user  
/data/data -- contains all apps installed by the user  
/system/bin -- contains binary files  
/data/local/temp -- world writable directory  

Kotlin and java are the 2 primary languages used to develop Android apps  
Kotlin is design to interopereate fully with java runs on JVM  
Google use Kotlin as the default programming language for app development  

**Android Security Features**  
Each Android application runs within its own isolated security sandbox, enforced by the underlying Linux-based architecture.  
Android is a multi-user Linux system where each application is treated as a separate user.  
By default, the system assigns each app a unique Linux user ID (UID). This UID is used by the system for access control, but is not exposed to the app itself.  
File system permissions ensure that only the app assigned a particular UID can access its own files.  
Android enforces the principle of least privilege, meaning apps only receive the permissions necessary to perform their core functionality.  
Additional privileges must be explicitly declared in the app's manifest and approved by the user.  

UID examples -- ls -al /data/data/  ---- com.android.chrome, com.android.shell  
app1 with Process 1466 can only access /data/data/com.android.app1 directory not that of other  

**Application signing**  
To install an application on a device or upload it to the Play Store, the APK file must be signed.  
Signing the APK is crucial for security, as it protects the package from malicious modifications.  

Insecure signing --- Jar signing v1 - some parts not covered by this signature  
Secure -- V2 and V3  
How to sign -- jarsigner/apksigner, play app signing, android studio -  google for procedure  

**APK Structure**  -- unzip and check apk file contents  
APK - android package - archive that contains all the components needed for app to run. 
when an android app is compiled the java/kotlin source code is first converted into java bytcode which is then transformed into a dex file.  
these dex files are executable an dcan be interpreted by Dalvik virtual machine or Android runtime ARTs  
In addition to compiled code, APK files include resources such as assets, images, UI layouts, and the AndroidManifest.xml file  

**Unzipped APK contains following**  
META-INF -- certificate/signatures - used in code signing  
assets -- application assets  -- images,videos,docs,dvs and other raw files  
lib -- native code  -- written in c/C++ -- Native code is typically included in the application as shared libraries (.so files).  
These libraries can then be invoked from Java or Kotlin using the Java Native Interface (JNI).
res -- application resources  -- unlike assets cannot be modified by user at runtime. UI layouts, fonts, screen orientations and more  
AndroidManifest.xml  -- Metadata about the app - overall template of what app does  
classes.dex - compiled java code  - Large applications may include multiple DEX files  
resources.arsc - resource information  -- maps resource identifiers in the code to their actual values -- such as strings, colors, layouts and styles  
kotlin -- for apps written in kotlin and contains kotlin specific metadata  

**Android App and Development**  
Android Studio -- Official IDE for android application development  
google how to install on kali/windows  

**Types of Applications**  
Native apps -- specifically built for particular OS  
Web apps -- build using HTML, CSS and JS similar to native apps uses mobile browser to disaply and interact with app web pages  
Hybrid -- both combined  

WebView are components that allow developers to emded and display web content directly within app.  
webview.loadUrl("file:///android_asset/html/index.html")  
setJavaScriptEnabled(true)  - means that the WebView is allowed to execute Javascript code -- XSS  

**Application Frameworks**  
An application framework is a set of libraries that provides developers with a structured way to build applications using pre-built components and tools. 
These often include UI elements, security and authentication mechanisms, error handling, logging systems, and more.  

Flutter -- use IDE like Android studio  
Xamarin -- cross platform supports Android IOS and desktop apps  
React Native, Apache Cordova and others  


















