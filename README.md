# Expansion-File-Android

#### Google Play Developer Console has a limitation that doesn’t allow users to upload the APK file of more than 100 MB size. However, some android apps need more space for high-fidelity graphics, media files, or other large assets. Previously, if your app exceeded 100 MB, you had to host and download the additional resources yourself.
#
## How to make expansion file ?
#### Make a zip folder of your assets like audio files, video files, images or anything you want to use in your app. Rename your zip file into below format.

```java [main/patch].<expansion-version>.<package-name>.obb ```

#### For example, your APK version is 1000 and your package name is com.example.app. If you upload a main expansion file, the file name should be main.1000.com.example.app.obb

#### Now your zip file is ready to upload.
#
## Storage location
#### When Google Play downloads your expansion files to a device, it saves them to the system’s shared storage location. The specific location for your expansion files is: <shared-storage>/Android/obb/<package-name>/

#### When you download an app with expansion file from play store, your expansion file will be downloaded to ../Android/obb/main.1000.com.example.app.obb location.
 #
 ## Declaring user permissions Required
####In order to download the expansion files, the Downloader Library requires several permissions that you must declare in your application’s manifest file. They are:
```java
  <manifest ...>
    <uses-permission android:name="com.android.vending.CHECK_LICENSE" />
      <!-- Required to download files from Google Play -->
     <uses-permission android:name="android.permission.INTERNET" />
     <!-- Required to keep CPU alive while downloading files
         (NOT to keep screen awake) -->
     <uses-permission android:name="android.permission.WAKE_LOCK" />
      <!-- Required to poll the state of the network connection
        and respond to changes -->
    <uses-permission
         android:name="android.permission.ACCESS_NETWORK_STATE" />
      <!-- Required to check whether Wi-Fi is enabled -->
     <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
     <!-- Required to read and write the expansion files on shared storage -->
    <uses-permission
         android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
     ...
 </manifest>
 ```
## How to import zip file library?
#### Zip_file library is used to extract your expansion file in your sdcard. Import it from below SDK path.

#### Eclipseandroid-sdkextrasgoogleplay_apk_expansionzip_file

#### How to use your expansion file from MainActivity.java

#### Below function is used to extract your expansion file using ZipHelper.java and create directory of extracted data from your expansion file.
```java
        public void ZipHelperNew() {

        ZipResourceFile expansionFile = null;

        try {

             expansionFile = APKExpansionSupport.getAPKExpansionZipFile(

             getApplicationContext(), versioncode, 0);

             ZipEntryRO[] zip = expansionFile.getAllEntries();

             Log.e("", "zip[0].isUncompressed() : " +  zip[0].isUncompressed());

             Log.e("","mFile.getAbsolutePath() : "+ zip[0].mFile.getAbsolutePath());

             Log.e("", "mFileName : " + zip[0].mFileName);

             Log.e("", "mZipFileName : " + zip[0].mZipFileName);

             Log.e("", "mCompressedLength : " + zip[0].mCompressedLength);

             String hallostring = zip[0].mFileName;

             String asubstring = hallostring.substring(0, 40);

             File file = new File(Environment.getExternalStorageDirectory().getAbsolutePath() + "/dirName");

             ZipHelper.unzip(zip[0].mZipFileName, file);

                  if (file.exists()) {

                  Log.e("", "unzipped Success: " + file.getAbsolutePath());
                  }

             } catch (IOException e) {
             e.printStackTrace();
             }
        }
```
#### Extract your expansion file using ZipHelper.java

#### This ZipHelper.java is used to extract the expansion file with above code.
```java
public class ZipHelper {

static boolean zipError = false;

public static boolean isZipError() {

return zipError;
}

public static void setZipError(boolean zipError) {

   ZipHelper.zipError = zipError;
}

public static void unzip(String archive, File outputDir) {

   try {

       Log.d("control", "ZipHelper.unzip() - File: " + archive);

       ZipFile zipfile = new ZipFile(archive);

       for (Enumeration<? extends ZipEntry> e = zipfile.entries(); e

               .hasMoreElements();) {

           ZipEntry entry = (ZipEntry) e.nextElement();

           unzipEntry(zipfile, entry, outputDir);
         }

   } catch (Exception e) {

       Log.d("control", "ZipHelper.unzip() - Error extracting file "

               + archive + ": " + e);

       setZipError(true);
   }

}

private static void unzipEntry(ZipFile zipfile, ZipEntry entry,

File outputDir) throws IOException {

   if (entry.isDirectory()) {

       createDirectory(new File(outputDir, entry.getName()));

       return;
   }

File outputFile = new File(outputDir, entry.getName());

   if (!outputFile.getParentFile().exists()) {

       createDirectory(outputFile.getParentFile());

   }

Log.d("control", "ZipHelper.unzipEntry() - Extracting: " + entry);

   BufferedInputStream inputStream = new BufferedInputStream(

           zipfile.getInputStream(entry));

   BufferedOutputStream outputStream = new BufferedOutputStream(

           new FileOutputStream(outputFile));

try {

       IOUtils.copy(inputStream, outputStream);

} catch (Exception e) {

       Log.d("control", "ZipHelper.unzipEntry() - Error: " + e);

       setZipError(true);

} finally {

       outputStream.close();

       inputStream.close();

}
}

private static void createDirectory(File dir) {

   Log.d("control",

           "ZipHelper.createDir() - Creating directory: " + dir.getName());

   if (!dir.exists()) {

       if (!dir.mkdirs())

           throw new RuntimeException("Can't create directory " + dir);

   } else

       Log.d("control",

               "ZipHelper.createDir() - Exists directory: "

                       + dir.getName());
   }

}
```
#### Your expansion file has now been extracted to your SD card.

#### How to get count of images, mp4 and mp3 stored in SD card directory.

#### Try below code to get a total number of count from sdcard directory.
```java
 String filePath = "/mnt/sdcard/dirName/";

File file = new File(filePath);

    if (file.exists()) {

    File fileCount = new File("/mnt/sdcard/dirName/);

    File[] list = fileCount.listFiles();

    for (File f : list) {

    String name = f.getName();

          if (name.endsWith(".jpg") || name.endsWith(".mp3") || name.endsWith(".mp4"))

          Sdcardcount++;

    }
}
```
#### Above code will get you a list of files stored in particular directory.
