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









 
