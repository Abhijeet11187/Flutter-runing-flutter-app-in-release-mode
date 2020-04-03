## Doc file instructions to run flutter app in relase mode


# Reference Links are

1 . https://youtu.be/i574lUpnt0g
<br>
2 . https://youtu.be/dR04ArAhxd4

<br><br><br>

Signing the app :


1.Create Key Store :

- To create a key store run command for windows
keytool -genkey -v -keystore D:\KeyStore\key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias key

- May be this command doesnt run on your cmd.This command is a part of Andriod Studio(java).For concrete path run flutter doctor -v and locate the path printed after ‘Java binary at:’.

- Make sure that  you have created the ‘KeyStore’folder in D drive.as per the keytool command or change the path in the command where key.jks is going to be created.

2.Reference Keystore from the App :

-  Create file key.properties in the andriod folder(this file exists with setting.gradle,gradle.properties files)

-   Contents in that file are

storePassword= Password while creating keystore
keyPassword= Password while creating keystore
keyAlias=key
storeFile=D:/KeyStore/key.jks(Path Where key.js is stored)



3. Configure signing in gradle :
  
-  Open file build.gradle from andriod/app/guild.gradle.

-  Above  defaultConfig {	
… 
…
}
                    Paste :

  
 def keystoreProperties = new Properties()
   def keystorePropertiesFile = rootProject.file('key.properties')
   if (keystorePropertiesFile.exists()) {
       keystoreProperties.load(new FileInputStream(keystorePropertiesFile))
   }




 			-  In the same file in thbuildTypes  block  change signingConfigs to  signingConfigs.release or simply paste the code :











signingConfigs {
       release {
           keyAlias keystoreProperties['keyAlias']
           keyPassword keystoreProperties['keyPassword']
           storeFile keystoreProperties['storeFile'] ? file(keystoreProperties['storeFile']) : null
           storePassword keystoreProperties['storePassword']
       }
   }
    buildTypes {
        release {
            // TODO: Add your own signing config for the release build.
            // Signing with the debug keys for now, so `flutter run --release` works.
           signingConfig signingConfigs.release
        }
    }



Reviewing the app mainfest :

-  Goto path /andriod/app/src/main open AndroidMainfest.xml file and if your app is going to use the internet add internet permission for that in mainfest file.

    <uses-permission android:name="android.permission.INTERNET" />






Building release level app :

         -  Run command : flutter build apk in the cmd of project files.
         -  Goto path : 				 build\app\outputs\apk\release\app-release.apk
            Where you get the release level .apk file.




      
