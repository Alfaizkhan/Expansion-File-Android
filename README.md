# Expansion-File-Android

Google Play Developer Console has a limitation that doesn’t allow users to upload the APK file of more than 100 MB size. However, some android apps need more space for high-fidelity graphics, media files, or other large assets. Previously, if your app exceeded 100 MB, you had to host and download the additional resources yourself.
#
## How to make expansion file ?
Make a zip folder of your assets like audio files, video files, images or anything you want to use in your app. Rename your zip file into below format.

[main/patch].<expansion-version>.<package-name>.obb

For example, your APK version is 1000 and your package name is com.example.app. If you upload a main expansion file, the file name should be main.1000.com.example.app.obb

Now your zip file is ready to upload.
