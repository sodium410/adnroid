**Android Studio and Emulator**  
wget https://redirector.gvt1.com/edgedl/android/studio/ide-zips/2024.3.1.14/android-studio-2024.3.1.14-linux.tar.gz  
tar xvzf android-studio-2024.3.1.14-linux.tar.gz  
sh android-studio/bin/studio.sh  

**Android Debug Bridge - ADB**  
apt-get install adb  
adb install .\myapp.apk  
adb root  
adb shell  
pm list packages | grep myapp  //get app identifier com.htb.myapp    
ls -l /data/data | grep com.hackthebox.myapp  //uid is the user id of app run as user  




