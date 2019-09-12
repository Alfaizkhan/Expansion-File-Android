# Expansion-File-Android

Google Play Developer Console has a limitation that doesn’t allow users to upload the APK file of more than 100 MB size. However, some android apps need more space for high-fidelity graphics, media files, or other large assets. Previously, if your app exceeded 100 MB, you had to host and download the additional resources yourself.
#
## How to make expansion file ?
Make a zip folder of your assets like audio files, video files, images or anything you want to use in your app. Rename your zip file into below format.

[main/patch].<expansion-version>.<package-name>.obb

For example, your APK version is 1000 and your package name is com.example.app. If you upload a main expansion file, the file name should be main.1000.com.example.app.obb

Now your zip file is ready to upload.
#
## Storage location
When Google Play downloads your expansion files to a device, it saves them to the system’s shared storage location. The specific location for your expansion files is: <shared-storage>/Android/obb/<package-name>/

 When you download an app with expansion file from play store, your expansion file will be downloaded to ../Android/obb/main.1000.com.example.app.obb location.
 #
 ## Declaring user permissions Required
In order to download the expansion files, the Downloader Library requires several permissions that you must declare in your application’s manifest file. They are:
####  <manifest ...>
####     <!-- Required to access Google Play Licensing -->
####     <uses-permission android:name="com.android.vending.CHECK_LICENSE" />
####      <!-- Required to download files from Google Play -->
####     <uses-permission android:name="android.permission.INTERNET" />
####      <!-- Required to keep CPU alive while downloading files
####         (NOT to keep screen awake) -->
####     <uses-permission android:name="android.permission.WAKE_LOCK" />
####      <!-- Required to poll the state of the network connection
####         and respond to changes -->
####     <uses-permission
####         android:name="android.permission.ACCESS_NETWORK_STATE" />
####      <!-- Required to check whether Wi-Fi is enabled -->
####     <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
####      <!-- Required to read and write the expansion files on shared storage -->
####     <uses-permission
####         android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
####     ...
####  </manifest>
