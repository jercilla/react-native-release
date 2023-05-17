```
npx react-native@0.71.8 init AwesomeProject --version 0.71.8 
mv AwesomeProject/* .
rm -r AwesomeProject
```

#Run
Terminal 1 (better outside vscode)
```
cd ~/dev/android-studio-2022.1.1.19-linux/android-studio/bin
./studio.sh

# + Create/Start new device emulator (Andoroid v33) on Device Manager'
```

Terminal 2 (better outside vscode)
```
npx react-native start
```

Terminal 3 (better outside vscode)
```
npx react-native run-android
```

#Release

https://reactnative.dev/docs/signed-apk-android
```
keytool -genkeypair -v -storetype PKCS12 -keystore app/upload-to-play-store.keystore -alias google-play-store-key -keyalg RSA -keysize 2048 -validity 10000
```

+ Edit gradle.properties
```
MYAPP_UPLOAD_STORE_FILE=upload-to-play-store.keystore
MYAPP_UPLOAD_KEY_ALIAS=google-play-store-key
MYAPP_UPLOAD_STORE_PASSWORD=4ndr01d
MYAPP_UPLOAD_KEY_PASSWORD=4ndr01d
```

>Note about using git
>
>Saving the above Gradle variables in ~/.gradle/gradle.properties instead of android/gradle.properties prevents >them from being checked in to git. You may have to create the ~/.gradle/gradle.properties file in your user's >home directory before you can add the variables.

+ android/app/build.gradle (OJO: not android/build.gradle)
```
...
android {
    ...
    defaultConfig { ... }
    signingConfigs {
        release {
            if (project.hasProperty('MYAPP_UPLOAD_STORE_FILE')) {
                storeFile file(MYAPP_UPLOAD_STORE_FILE)
                storePassword MYAPP_UPLOAD_STORE_PASSWORD
                keyAlias MYAPP_UPLOAD_KEY_ALIAS
                keyPassword MYAPP_UPLOAD_KEY_PASSWORD
            }
        }
    }
    buildTypes {
        release {
            ...
            signingConfig signingConfigs.release
        }
    }
}
...
```

```
cd android
./gradlew bundleRelease
``` 

>The generated AAB can be found under android/app/build/outputs/bundle/release/app-release.aab, 
>and is ready to be uploaded to Google Play.

(uninstall any previous from emulator)
```
npx react-native run-android --mode=release
```

(Proguard to reduce the size of the APK)
```
/**
 * Run Proguard to shrink the Java bytecode in release builds.
 */
def enableProguardInReleaseBuilds = true
```

>By default, INTERNET permission is added to your Android app as pretty much all apps use it. 
>SYSTEM_ALERT_WINDOW permission is added to your Android APK in debug mode but it will be removed in production.

#Release github actions

https://www.obytes.com/blog/react-native-ci-cd-github-action


Better npm publish
```
npm i np
npm i react-native-version
```

package.json
```
    scripts {
        ...
        "np": "np --no-publish",
        "postversion": "react-native-version"
    }

    "repository": {
        "type": "git",
        "url": "git+https://github.com/jercilla/react-native-release.git"
     }

```

>We added react-native-version command as a postversion script because np can't >update ios and android versions. react-native-version will help sync package.json >version with android and ios as well as increment the build number automatically. 


