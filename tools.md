## Android Studio -- SDK plus Emulator  
wget https://redirector.gvt1.com/edgedl/android/studio/ide-zips/2024.3.1.14/android-studio-2024.3.1.14-linux.tar.gz  
tar xvzf android-studio-2024.3.1.14-linux.tar.gz  
sh android-studio/bin/studio.sh  

PS C:\Users\cs410\AppData\Local\Android\Sdk\platform-tools> ..\emulator\emulator.exe -list-avds  //to list installed devices on windows  
PS C:\Users\cs410\AppData\Local\Android\Sdk\platform-tools> ..\emulator\emulator.exe -avd 'Pixel_9a'  //start the device emulator without starting studio  

## Android Debug Bridge - ADB - shell to interact with emulated/physical device  
apt-get install adb  //lin  Below windows its already part of studio SDK install  
C:\Users\<username>\AppData\Local\Android\Sdk\platform-tools  -- this didnt work - so install it using scoop  

C:\> Set-ExecutionPolicy RemoteSigned -Scope CurrentUser  
C:\> iex (New-Object System.Net.WebClient).DownloadString('https://get.scoop.sh')  
C:\> scoop bucket add extras  
C:\> scoop install adb  

adb install .\myapp.apk  
adb root  
adb shell  
pm list packages | grep myapp  //get app identifier com.htb.myapp  
If package name is known get the path --  adb shell pm path com.example.myapp  
adb pull /data/app/com.example.myapp-1/base.apk .   //download the apk installed  
ls -l /data/data | grep com.hackthebox.myapp  //uid is the user id of app run as user  

## APKTool  -- disassembler, decoder and reassembler  
https://apktool.org/docs/install  
apktool d myapp.apk  //disassemble the apk - creates a dir with apps name with extracted and decoded files  





