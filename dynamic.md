Dynamic Analysis is a method for evaluating the security and performance of an Android application by examining its behavior during runtime.  

## Enumerating Local Storage  
**Internal storage** refers to the private area allocated to each application  
If not rooted, This area is inaccessible to other apps and users, ensuring high security and privacy  
**External storage** refers to storage spaces that are not exclusively tied to the app, like SD cards or shared internal partitions.  

**Database** -- Android applications use databases to store structured data.  
root:/# ls -l /data/data/com.hackthebox.mypassmanager  //shows db in use  
sqlite3 /data/data/com.hackthebox.mypassmanager/databases/SecureVault.db  
sqlite> .tables  
sqlite> select * from credentials;  

**Shared Preferences** is another common storage method used for storing key-value pairs in an xml file  
root:/# ls -l /data/data/com.hackthebox.mypassmanager2/  //lists the shared_prefs dir  
root:/# cat /data/data/com.hackthebox.mypassmanager2/shared_prefs/app_properties.xml  //pdate the value of pin use fro true to false and restart app  
The passcode authentication step has been successfully bypassed.  

**App-Specific External Storage** --  to save larger datasets or share data between applications.  
root:/# ls -l /sdcard/Android/data/com.hackthebox.mynotebook/files/MyPersonalNotes/  //notes stored on sdcard  
These .txt files contain the saved notes that users cannot see directly from the app without knowing the pin.  
This highlights the importance of securing external storage, as unprotected files can be accessed easily on rooted devices.  

in short -- does it store any sensitive info in local db, any access control attributes in shared preferences,  
any files on external storage which are pin protected by app but directly accessible on external storage without pin, bypassing protections  

## Exported Activities  
By default, activities are only accessible within their app.  
However, when the attribute exported of an activity is set to true, then it can be invoked by other applications or components.  
Exported activities that lack adequate security can often become the target of malicious intents, eventually leading to unauthorized access.  

**Exploiting Insecure Exported Activities**  
the main activity has a pin check to access notes which are encrypted on external storage  
however the sub activity viewnotes is exported and doesnt check pin - just opens given filename decrypting contents  
manifest has - <activity android:name="com.hackthebox.myapp.NoteContentActivity" android:exported="true"/>  
adb shell am start -n com.hackthebox.myapp/.NoteContentActivity --es filename "Note_347233492.txt"  
check exported activities in manifest and if they can be exploited in some way to bypass security checks  

## Insecure Logging  
sensitive information is unintentionally recorded through the application’s logs  
his may include user credentials, payment details, or other personal data handled by the app  
three main log types..  
main	Stores most application logs  system	Stores messages originating from the Android OS crash	Stores crash logs  
adb logcat '*:D' | grep 'Debug note: '   start the logcat and start the activity that is logging and exported  
adb shell am start -n com.hackthebox.myapp/.NoteContentActivity --es filename "Note_-2074205319.txt" --es userPin "00000000"  
this case is vuln because the app is logging decrypted contents before pin check thus revealing decrypted data without pin  
jadx to check the code and look for log.d calls  

## Exploiting WebViews  
WebView is a component that allows Android apps to display web content as part of the application's layout. example gyftr app  
Injecting Javascript Code to Exploit WebViews


## Intercepting HTTP/S Requests  
to uncover how an app nteracts with remote servers and external resources.  
and run API/Web attacks  

**Intercepting API/HTTP/s calls**  
will use Burp Suite to intercept the network traffic of an Android application.  
configure your Android Virtual Device. Go to Settings → Internet → Android Wi-Fi, tap the pencil icon   
expand Advanced options, and under Proxy, choose Manual. Enter the burp IP and port  
IDOR, infor disclosure and other API/Web attacks can be tried based on the request responses  

**SSL/TLS Certificate pinning bypass** 
ensures that the app communicates only with a trusted server by verifying the server’s certificate against a copy embedded in the app.  
is a security measure designed to prevent man-in-the-middle (MITM) attacks. or say burp interception for testers  
has a hash of the cert in network config file under res/xml directory  
When the app makes a network request, it compares the received server's certificate (or public key) with the one it has pinned.  
pinning can still be bypassed using dynamic instrumentation tools like Frida.  
These allow testers to modify the app's behavior at runtime, including bypassing security checks.  

steps..
export the burp CA cert in DER format  
adb push ~/Desktop/BurpCA.cer /sdcard/Download/  //copy the burp cert to device  
install the certificate by navigating to Settings -> Security and privacy -> More security & privacy -> Encryption & credentials -> Install a certificate -> CA certificate -> INSTALL ANYWAY, and tap the file BurpCA.cer  
use APKtool to extract APk and check Manifest file to read network security config and custom cert path   

patch the application and replace this certificate with the one we exported from Burp  - APK extracted dir  

cp BurpCA.cer ChatApp/res/raw/certificate.der  

Next, re-build and sign the app using the following commands.  
<img width="510" height="92" alt="image" src="https://github.com/user-attachments/assets/07cc1273-ce02-4f50-82d9-3a0c7e5c09d1" />  

Uninstall ChatApp and install the patched myChatApp  
if sha/base64 hash of cert is stored in network config file easier to modify along with cert,  
however if defined in the codebase,  
use Frida to hook into specific method that checks the cert hash and send the burps 
in this instance the code has CertificatePinner.Builder().add("www.chatapp.com", certHash());  
googling the method we find its picking the hash coming from certHash function  
write a code snippet.js to first read the value returned by the function certHash  
then calculate the hash of imported burp cert using below command..  
openssl x509 -in BurpCA.cer -pubkey -noout | openssl rsa -pubin -outform der | openssl dgst -sha256 -binary | openssl enc -base64  
rewrire the frida snippet.js code to make the function return the sha value of burp cert  
this way, cert pinning is bypassed  ..  

frida -U -l snippet.js -f com.hackthebox.chatapp  

**In short**..
replace app cert with burp cert, make device trust burp cert, find hash value returned by function, use frida to return hash of burp cert  
so this requires reading the code with jadx and writing snipped code to handle inject app with Frida  

There are other methods - cert pinning is very important question to ask during scoping  










 
